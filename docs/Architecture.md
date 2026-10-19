# Architecture

## Pipeline overview

```
 Story idea / source video
          │
          ▼
 ┌─────────────────┐   ┌──────────────────┐   ┌───────────────────┐
 │ StoryWriter     │──▶│ StoryTranslator  │──▶│ HookCreator       │
 │ (LLM)           │   │ (localisation)   │   │ (opening seconds) │
 └─────────────────┘   └──────────────────┘   └───────────────────┘
          │
          ▼
 ┌─────────────────────┐   ┌──────────────┐   ┌───────────────┐
 │ CharacterExtractor  │──▶│ SceneBuilder │──▶│ PromptCreator │
 │ (canonical looks)   │   │ (shot list)  │   │ (image prompts)│
 └─────────────────────┘   └──────────────┘   └───────────────┘
          │
          ▼
 ┌────────────────┐   ┌────────────────┐   ┌────────────────┐
 │ ImageGenerator │──▶│ VideoGenerator │   │ AudioGenerator │
 │                │   │ (image→clip)   │   │ (TTS narration)│
 └────────────────┘   └────────────────┘   └────────────────┘
                              │                    │
                              ▼                    ▼
                      ┌──────────────────────────────────┐
                      │ VideoComposer                    │
                      │ sync · subtitles · logo · outro  │
                      │ platform presets · rendering     │
                      └──────────────────────────────────┘
                                     │
                                     ▼
                       Shorts / Reels / TikTok exports
```

Optional stages (disabled by default): `YoutubeDownloader`, `SpeechToText` and `ScriptRewriter` for adapting existing videos, and `SEOOptimizer`, `ThumbnailCreator`, `PublishingScheduler` and `AnalyticsOptimizer` for publishing.

## Design principles

### 1. One module per stage
Each stage lives in `src/modules/<Stage>/` with a `*_main.py` entry point and its own internal helpers. Stages communicate only through files in a per-video output directory, never through shared in-memory state. Any stage can therefore be re-run on its own.

### 2. Configuration over code
A single `config.yaml` switches stages on or off (`status: true/false`) and selects providers, models, voices, resolutions and retry policies. Changing a provider or skipping a stage needs no code change.

### 3. Factories for external services
LLMs (`llm_factory`), image generators, video generators and audio generators are created through factory classes. Each provider implements the same interface, so the pipeline code never depends on a specific vendor.

### 4. Per-video state and resumability
Every video gets its own directory holding the story, scenes, prompts, images, clips, audio and a state file. If a step fails (rate limit, network error, bad generation), the next run continues from that step instead of starting over.

### 5. Background job queue
Video generation and composition are the slowest steps. They are queued as background jobs, so the pipeline can start writing the next story while clips render. A batch runner processes a list of story ideas end to end.

## Reliability

- **Retries with exponential backoff** on every external call, with configurable `max_retries` and `retry_delay`
- **LLM fallback chain:** if the primary model fails, the request is retried on an OpenAI-compatible endpoint
- **Validation steps:** story validation and checksum validation catch truncated or malformed outputs before they flow downstream
- **Prompt logging:** every prompt and response is logged for debugging and prompt iteration

## Rendering performance

The composer supports parallel subtitle rendering, configurable render threads and concurrent renders, plus bitrate and encoder-preset control. Long batches therefore render reliably on a single workstation.

## Testing

Modules are covered by pytest tests (`pytest`, `pytest-mock`, `pytest-cov`). External services are mocked, so the pipeline logic can be tested without spending API credits.
