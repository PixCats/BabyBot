# BabyBot

**An agentic AI interface for Android. Bring your own LLM — cloud or local — and get a full agent stack out of the box.**

BabyBot is a mobile-native agentic client. Pick a model (Claude, GPT, Gemini, or any local Ollama model running on your home server), and BabyBot wraps it with a real tool layer: web search and research, on-device computer vision, perceptual + semantic image matching against the open web, a planner/worker execution loop for long tasks, persistent context, and an expressive UI. It runs on your phone. Your photos, your prompts, and (if you want) your model all stay local.

It is **not** another chat wrapper.

> **Closed beta is open right now.** Join the testers group below to get the app on your phone.

[![Join the closed beta](https://img.shields.io/badge/Closed_Beta-Join_the_testers_group-2ea44f?style=for-the-badge)](https://groups.google.com/g/cutitas85)
[![Play Store](https://img.shields.io/badge/Play_Store-Already_a_tester-blue?style=for-the-badge&logo=googleplay)](https://play.google.com/store/apps/details?id=baby.bot.app)

---

## What makes it different

There are a hundred ChatGPT-clone apps on the Play Store. BabyBot isn't one of them.

| | Most mobile AI apps | **BabyBot** |
|---|---|---|
| **Backend** | Locked to one provider | Claude, OpenAI, Gemini, **and local Ollama** — switch per conversation |
| **Tools** | Whatever the provider ships | Built-in agent loop with a real tool catalog (search, vision, files, plans, location, more) |
| **Vision** | Send image to cloud, hope for the best | On-device multi-signal pipeline runs BEFORE the model sees the image — OCR, content labels, object detection, optional ML embedding, EXIF |
| **Web research** | A toy search wrapper | Multi-stage orchestrator: fan-out queries → fetch → extract → rank → deduplicate, fed to the model as structured context |
| **Image search** | Doesn't exist, or reverse-image only | Hybrid perceptual hash + semantic embedding matching against any web search result set, with cross-source recognition |
| **Long tasks** | Time out and start over | Continue button — resume any multi-step task across hours, sessions, app restarts |
| **Frontier + local together** | Pick one | Planner/worker split: expensive model writes the spec, cheap local model executes against it |
| **Local-only mode** | Requires desktop | Point at your home Ollama instance and the app works fully offline-from-cloud |

---

## Core features

### 🧠 Multi-backend LLM with hot-swap

Configure multiple backends and switch between them mid-conversation. Each gets its own settings (model, base URL, API key, parameters):

- **Anthropic** — Claude Sonnet, Opus, Haiku
- **OpenAI** — GPT-4 family, GPT-5, the o-series
- **Google Gemini** — Pro and Flash variants
- **Ollama** — point at any local server (LAN or DDNS), use any model it serves: Gemma, Llama, Qwen, Mistral, etc.

Backends share the same tool layer and the same conversation. Drop a frontier model for a hard reasoning step, switch to a local model for cheap follow-ups, keep the context.

### 🛠️ Agentic tool layer

Every backend gets the same tools. The model decides when to call them. Tools include:

- **`find_image_match_online`** — search the web for similar images, hybrid match by perceptual hash + (optional) ML embedding, return source pages with content
- **`browse_web`** / **search orchestrator** — fan-out queries, parallel fetch, content extraction, ranking, dedup
- **`make_plan`** / **plan state tracker** — multi-step task tracking with status, dependencies, and round-budget awareness
- **File operations** — read, write, replace within app sandbox
- **`show_map`** — render a Leaflet map with coordinates or named places
- **`get_location`** — Nominatim-based reverse geocoding (opt-in)
- **Shell execution** (Ollama-only, opt-in) — run commands on the device
- **Custom tools** — extensible via the `ToolRegistry` (Kotlin)

A **Continue** button surfaces when the model wraps up a multi-tool run partway through. One tap resumes from exactly where it stopped, with the full state intact — even after force-closing the app.

### 👁️ On-device vision pipeline

When you attach a photo, BabyBot doesn't just ship it to the model and wait. A **multi-signal interrogator** runs on-device first and feeds the model rich pre-extracted context:

- **EXIF** — date taken, device, GPS (opt-in)
- **OCR** — Google ML Kit text recognition, multi-resolution
- **Content labels** — ML Kit image labeling (subject categories, confidence scores)
- **Object detection** — ML Kit object localizer
- **Visual embeddings** — optional MobileNet V3 Large download (~22MB), enables cross-angle / cross-source image matching that perceptual hashing alone misses
- **EmbeddingMatchSignal / WholeImageMatchSignal / RegionMatchSignal** — extensible signal architecture for adding new vision modalities

The model sees the image AND a structured digest. Even small local models (Gemma 4B, Phi 4) make confident identifications they couldn't make from pixels alone.

### 🔍 Web research orchestrator

A from-scratch multi-stage search pipeline that runs every search through:

1. **Query planning** — break the user intent into parallel fan-out sub-queries
2. **Provider dispatch** — DuckDuckGo, Bing, parallel
3. **Content extraction** — site-aware extractors (Wikipedia, generic, with truncation guards)
4. **Ranking + dedup** — combined score over host authority, content match, snippet quality
5. **Digest formatting** — structured markdown the model can actually use

It also keeps a **per-conversation search cache** so follow-up questions don't waste fetches re-fetching the same pages.

### 🎭 Planner / worker (BabyAssist)

The "do something hard with cheap tokens" mode. Switch on planner/worker and:

- The **planner** (frontier model — Claude, GPT-5, Gemini Pro) reads your goal and produces a concrete spec
- The **worker** (cheap local model — Gemma, Llama, your choice) executes against the spec one step at a time
- A state-machine plan tracker keeps the worker honest, gates progress, prevents infinite loops, and routes hard sub-problems back to the planner

End result: frontier-quality decisions at local-model token cost.

### 💾 Persistent, transparent context

- Conversations save to local SQLite, encrypted at rest
- A user-facing context builder injects useful metadata into every system prompt: date, locale, location (opt-in), device — so the model never has to ask "what's your timezone" again
- Image memory: subjects you've identified are remembered across sessions
- Standing user preferences are stored separately and merged into context

### 🎨 Expressive UI

Yes, the eyes are still there. The eyes are great.

- Animated dual-eye widget responds to model output mood tags (`[mood:happy]`, `[mood:thinking]`, `[mood:error]`, etc.)
- Shape changes by activity state (idle, listening, working, error)
- Custom typography, dark theme, smooth Compose animations
- Optional thinking ticker shows what the model is doing during long tool runs
- Inline image rendering in chat — when the agent finds a relevant image online, it shows up in the bubble, not as a text URL

The personality layer is a thin shell over a serious agent stack — never the other way around.

---

## Architecture (under the hood)

For the curious. None of this matters to a user — but if you're considering building something similar, here's the stack:

- **Language**: Kotlin (100%), targeting Android 7+ (API 24), compileSdk 36
- **UI**: Jetpack Compose with Material 3
- **Orchestration**: a `ModelManager` running a tool-call loop, with hidden planner/worker traces, narration buffering, round budgets, per-file re-read brakes, and a deterministic finalization gate
- **Tool registry**: each tool is a Kotlin object implementing `Tool` with JSON schema metadata, surfaced to whichever backend is active
- **Vision signals**: each signal (`OcrSignal`, `LabelSignal`, `ObjectDetectionSignal`, `EmbeddingMatchSignal`, etc.) is an independent module that can fail or contribute without blocking the others
- **Embedder**: MediaPipe Image Embedder wrapper for MobileNet V3 Large, mutex-protected, ~30-80ms inference
- **Search**: a custom orchestrator with provider abstraction, parallel fetch via OkHttp, site-specific extractors, and persistent cache
- **Concurrency**: structured coroutines throughout, with a BackgroundWorkService keeping the agent alive across UI lifecycle events
- **No analytics, no tracking, no ads**

---

## Privacy

- **No telemetry from BabyBot itself.** No analytics SDK, no crash reporter that calls home, no third-party ads.
- **Google ML Kit telemetry** is present because it ships with ML Kit's bundled libraries (anonymous SDK metrics, not your data). Same as every other Android app that uses ML Kit. Will be replaced by raw TFLite in a future version.
- **Cloud LLM calls** go directly to the provider you configured (Anthropic, OpenAI, Google) — no proxy through any BabyBot server.
- **Local mode** sends nothing outside your LAN if you only use Ollama and disable web search.
- **GPS, photo location** are opt-in. Default OFF.
- API keys are stored encrypted on-device.

---

## Status & Roadmap

BabyBot is in **active development** and currently in **closed beta** on the Play Store. Things ship weekly, sometimes daily.

**Live and stable:**
- Multi-backend LLM with hot-swap
- Full agent tool loop with Continue button
- Multi-signal vision pipeline
- Web research orchestrator
- Planner/worker mode
- Optional ML embedding tier
- Hybrid image matching against web search

**Coming next:**
- WorkManager-backed background ML downloads
- MobileCLIP integration for substantially better semantic image matching
- Per-component sub-image matching via Object Detection
- Improved local-model tool-call reliability
- Multi-resolution OCR pass merging
- More custom site extractors

**On the long-term wishlist:**
- iOS port (no committed timeline)
- Skill / tool marketplace
- Voice mode

---

## Join the closed beta

The fastest way to get the app on your phone:

1. **Join the testers group** → [groups.google.com/g/cutitas85](https://groups.google.com/g/cutitas85)
2. Wait ~5 minutes for the group access to propagate
3. **Open the Play Store link** → [play.google.com/store/apps/details?id=baby.bot.app](https://play.google.com/store/apps/details?id=baby.bot.app)
4. Install, configure a backend (point at Ollama or paste a Claude/OpenAI/Gemini API key), say hi

**Looking for feedback specifically on:**
- Real-world image identification (products, places, plants, components, anything)
- Local Ollama setup pain points
- Battery / performance with the embedding tier active
- Tool-call reliability with different models
- Any weird UI states or crashes

Feedback channels are the Google Group, the Play Store internal feedback, or open an issue here on GitHub.

---

## About

BabyBot is built by [Robert Nilges / PixCats](https://github.com/PixCats) 

Other projects worth a look:
- **[SteamWidget](https://github.com/PixCats/SteamWidget)** — Steam stats widget for Windows
- **MyLuck** — also on the Play Store

---

## License

**BabyBot is closed-source software.** This repository exists as a public face for the project — documentation, issue tracking, release notes, and beta coordination. The application source code is not published.

Compiled APKs are distributed exclusively through the Google Play Store (currently in closed testing). Sideloaded redistribution of the APK is not authorized. Reverse engineering, decompilation, and disassembly are prohibited except where expressly permitted by applicable law.

Copyright © 2025–2026 Robert Nilges / PixCats. All rights reserved.

---

<div align="center">

**Built in Android Studio, with too much coffee, in the UK.** ☕

</div>
