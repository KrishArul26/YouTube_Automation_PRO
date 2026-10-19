# Tech Stack

## Core

| Area | Technology | Why |
|---|---|---|
| Language | **Python 3** | Rich ecosystem for AI, media and automation |
| Configuration | **PyYAML** | One readable file controls every stage and provider |
| Testing | **pytest**, pytest-mock, pytest-cov | Mocked external services keep tests fast and free |

## AI & language

| Area | Technology |
|---|---|
| LLMs | **Google Gemini** (primary), **OpenAI-compatible** endpoints (fallback) |
| Text-to-speech | **ElevenLabs** multilingual models (narration and dialogue) |
| Speech-to-text | **OpenAI Whisper**, whisper.cpp, **Vosk** |
| NLP utilities | spaCy, NLTK |
| Image generation | Pluggable providers, including a local **Stable Diffusion** server |
| Image-to-video | Pluggable providers behind a common interface |

## Media processing

| Area | Technology |
|---|---|
| Video editing & rendering | **MoviePy** |
| Audio processing | **pydub** |
| Subtitles | **pysubs2**, fontTools (for non-Latin scripts such as Tamil) |
| Images | **Pillow** |
| Video download | **yt-dlp** |

## Automation & infrastructure

| Area | Technology |
|---|---|
| Browser automation | **Playwright**, Selenium |
| HTTP & scraping | requests, BeautifulSoup, lxml |
| Local API | **Flask** (job status and control endpoints) |
| Data handling | pandas |
| Job processing | Background job queue with a batch runner |

## Engineering practices

- Factory pattern for every external provider
- Retry with exponential backoff on all network calls
- Structured per-video output directories with state files
- Centralised logging, including prompt and response logs
- Feature flags (`status: true/false`) for every stage
