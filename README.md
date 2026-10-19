# YouTube Shorts Automation 🎬🤖

**An end-to-end, LLM-driven pipeline that turns a story idea into a finished, narrated, subtitled YouTube Short. Documented for portfolio and technical review.**

> **Note:** The implementation lives in a private repository. This repository documents the architecture, design decisions and technical depth of the project. Code walkthroughs are available on request.

---

## 📋 Project Summary

Producing a single short-form video by hand means writing a script, generating consistent visuals, recording narration, editing, adding subtitles and exporting for each platform. This project automates the whole chain.

Given a story idea (or an existing video to adapt), the pipeline:

1. **Writes the story** with an LLM and localises it into the target language (e.g. Tamil)
2. **Creates a hook** for the first seconds of the video
3. **Extracts characters** and builds a visual "blueprint" for each, so they look the same in every scene
4. **Splits the story into scenes** with shot descriptions
5. **Generates image prompts and images** for every scene
6. **Animates images into video clips**
7. **Generates narration** with a multilingual text-to-speech model
8. **Composes the final video**: audio/video sync, subtitles, logo, subscribe outro, platform-specific export

The current production use case is a Tamil-language channel retelling stories from Indian epics such as the Mahabharata.

## 🧠 What Makes It Interesting

- **Character consistency across scenes.** A dedicated character-extraction stage builds canonical descriptions (appearance, clothing, weapons) that are injected into every image prompt. This is the hardest problem in AI-generated video.
- **Provider-agnostic design.** LLMs, image generators, video generators and TTS engines sit behind factory interfaces, so a provider can be swapped in `config.yaml` without touching pipeline code.
- **Resilient by default.** Retries with exponential backoff, multi-LLM fallback (Gemini → OpenAI-compatible) and per-video state tracking let a long batch resume from the step that failed.
- **Batch and background processing.** A job queue runs video generation and composition in the background while the next story is being written.
- **Multi-platform output.** One composition step exports presets for YouTube Shorts, Instagram Reels, TikTok and Facebook Reels.

## 🏗️ Documentation

| Document | Contents |
|---|---|
| [Architecture](docs/Architecture.md) | Pipeline stages, data flow, module boundaries, state handling |
| [Features](docs/Features.md) | What each stage does and how it is configured |
| [Tech Stack](docs/Tech_Stack.md) | Libraries, services and why they were chosen |

## 💼 What This Demonstrates

- **Pipeline and system design:** 15+ independent modules orchestrated through a single YAML configuration
- **LLM engineering:** prompt design, structured outputs, prompt logging, provider fallback
- **Media processing:** programmatic video editing, audio sync, subtitle generation, rendering performance tuning
- **Automation engineering:** browser automation, retry strategies, job queues, testable modules with pytest

## 👤 Author

**Kirsnaragavan Arudpiragasam**, Automation Test Engineer, Berlin
[Portfolio](https://krisharul26.github.io/KrishArul26/portfolio/index.html) · [LinkedIn](http://www.linkedin.com/in/arul-krisnaragavan) · [GitHub](https://github.com/KrishArul26)
