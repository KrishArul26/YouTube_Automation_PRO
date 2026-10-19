# Features

## Story & script

| Stage | What it does |
|---|---|
| **StoryWriter** | Generates a story from an idea with an LLM, or reads ideas from a file for batch runs. Includes a validator that rejects incomplete or off-format stories. |
| **StoryTranslator** | Localises the story into one or more target languages in parallel (configurable worker count). |
| **HookCreator** | Writes a short attention-grabbing opening, delivered as voice and/or on-screen text. |
| **CTAManager** | Adds a call to action (e.g. subscribe prompt) at the right moment. |
| **ScriptRewriter** | *(Optional)* Rewrites a transcript of an existing video into an original script. |

## Visual consistency

| Stage | What it does |
|---|---|
| **CharacterExtractor** | Identifies every character and builds a canonical blueprint: appearance, clothing, signature weapons or objects. |
| **SceneBuilder** | Breaks the story into scenes with a visual description, camera framing and narration segment for each. |
| **PromptCreator** | Combines scene descriptions with character blueprints and a theme style guide into image-generation prompts. |

## Media generation

| Stage | What it does |
|---|---|
| **ImageGenerator** | Produces one image per scene through a pluggable provider (cloud APIs or a local Stable Diffusion server). |
| **VideoGenerator** | Animates each image into a short clip through a pluggable image-to-video provider. |
| **AudioGenerator** | Generates narration with a multilingual text-to-speech model, with tunable voice, stability, clarity, style and speed. Supports single-voice narration and multi-speaker dialogue. |
| **SpeechToText** | *(Optional)* Transcribes source audio with Whisper, whisper.cpp or Vosk. |

## Composition & output

| Feature | Details |
|---|---|
| **Audio/video sync** | Clip lengths are fitted to the narration for each scene automatically. |
| **Subtitles** | Word-grouped subtitles (configurable words per line) burned into the video. |
| **Branding** | Logo overlay with position, size, margin and opacity settings, plus an animated subscribe outro. |
| **Transitions** | 11 transition types with history tracking so consecutive videos don't repeat the same effect. |
| **Platform presets** | YouTube Shorts (1080×1920, up to 180 s), Instagram Reels, TikTok and Facebook Reels, each with its own resolution, FPS and duration limits. |
| **Quality control** | Bitrate, audio bitrate, encoder preset and FPS are configurable. |

## Operations

- **Batch mode:** process a whole list of story ideas unattended
- **Theme system:** switch visual and narrative style per channel or series (e.g. `mahabharatha`)
- **Resumable runs:** per-video state means a failed run continues where it stopped
- **Prompt logging:** full audit trail of what was sent to every model
- **Publishing hooks:** SEO metadata, thumbnail creation and scheduled publishing are designed as optional final stages
