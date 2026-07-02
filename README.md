# BabyBot

**An AI agent that lives on your Android phone.**

You speak. BabyBot listens. It sees what's on your screen, drives your apps, reads your documents, remembers the things and people in your life, and brings your choice of LLM — cloud or local — to make the decisions.

> *"Send a WhatsApp to my brother saying I'll be 20 minutes late."*
>
> *— Phone wakes. Opens WhatsApp. Finds the contact. Types the message. Sends. Reads back: "Done."*

That's a real BabyBot session. Voice in, action out. **You don't touch the phone.**

> **Closed beta is open right now.** Hop into the Discord, join the testers group, install from Play Store, talk to your phone.

[![Discord](https://img.shields.io/badge/Discord-Join_the_community-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/qaSrVm2cAg)
[![Play Store](https://img.shields.io/badge/Play_Store-Already_a_tester-blue?style=for-the-badge&logo=googleplay)](https://play.google.com/store/apps/details?id=baby.bot.app)

---

## Why this is different

There are a lot of AI apps on the Play Store. Most are chat wrappers. BabyBot isn't.

| Capability | ChatGPT app | Gemini | Claude app | Bixby / Assistant | **BabyBot** |
|---|:-:|:-:|:-:|:-:|:-:|
| Cloud LLM chat | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Run on local LLM (Ollama)** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Sees your screen** | ❌ | partial | ❌ | partial | ✅ |
| **Drives other apps for you** | ❌ | partial | ❌ | partial (Google stack) | ✅ |
| **Voice-driven hands-free** | partial | ✅ | partial | ✅ | ✅ |
| **Reads PDF / Word / Excel / PowerPoint** | ❌ | partial | ❌ | ❌ | ✅ |
| **Persistent visual memory of your world** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Hybrid web image identification** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **No analytics, no ads, no tracking** | ❌ | ❌ | ❌ | ❌ | ✅ |

The combination is the point. Lots of apps do one or two cells. BabyBot does the whole table.

---

## What it feels like to use

A morning with BabyBot, roughly:

- *"What's on my calendar today and the weather?"* — answered in one breath
- *"Let's go home"* — turn-by-turn navigation starts in Google Maps
- *"Text my wife that I'll be late"* — opens Messages, types, sends
- *"Send a WhatsApp to my brother saying 'on my way'"* — opens WhatsApp, finds the contact, sends
- *"What's in this PDF?"* (attached) — summary in plain English, key points pulled out
- *"What component is this?"* (photo attached) — identifies the part, finds the spec sheet, summarizes
- *"Is this my dog?"* (photo attached) — checks against your saved photos, knows
- *"Take a photo and tell me what's in it"* — opens camera, you snap, it tells you
- *"Set a timer for 25 minutes"* — done, no taps
- *"Read what's on my screen and email it to me"* — accessibility-driven, opens email, types, sends
- *"Remember this as my motorbike"* — saved. Next month when you photograph it from a different angle, BabyBot knows it's yours.

No app switching. No copy-paste. No menus. Just talk to BabyBot.

---

## The headline capabilities

### 🤖 Hands on your phone

BabyBot reads what's on screen and takes real actions in any app. Open WhatsApp, type a message, hit send. Compose an email in Gmail. Drive turn-by-turn navigation. Set alarms and timers. Take a photo. Control volume. Make a phone call. Send an SMS. Look up a contact. Copy something to your clipboard. All from voice commands, with the phone in your pocket if you want.

Powered by Android's Accessibility Service, which lets BabyBot interact with apps that don't expose a public API. **This is why BabyBot can send WhatsApp messages with no Meta API access** — it drives the actual WhatsApp UI the way you would.

### 👁️ Eyes on your world

Attach a photo and a five-stage on-device vision pipeline runs **before** the LLM sees the image:

- **EXIF** — date, location (if you allow), device that took the shot
- **OCR** — Google ML Kit text recognition, multi-resolution
- **Content labels** — ML Kit subject categorization with confidence scores
- **Object detection** — ML Kit object localizer with bounding boxes
- **Visual embeddings** — optional MobileNet V3 Large download (~22MB), enables cross-angle / cross-source identification

Results compose into a structured digest fed to the model. The effect: even small local models (Gemma 4B, Phi 4, Qwen 3B) make confident identifications because they're not working from raw pixels alone.

For online matching, BabyBot uses a hybrid perceptual hash + embedding pipeline to find products, places, components, and objects in web search results. It then pulls the source page content too — so the answer isn't just "this is X," it's "this is X, sold for Y, with features A, B, C."

### 🧠 Visual memory of your world

This one's worth pausing on, because **nobody else ships this.**

Save photos of the things and people that matter to you — your pet, your car, your kids, the components in your workshop, your friends' faces. BabyBot builds a local visual memory unique to you. Show it a new photo later — could be days, months, years — and it knows whether that's your dog Max chilling on the sofa, or just *a* dog. Recognizes subjects across years, angles, lighting changes, growth.

No training required. No cloud round-trips. No accounts. Add a photo, it's instantly recognizable. Add 20 photos across different angles and lighting, it gets robust.

Cloud assistants forget your pet the moment the conversation ends. BabyBot remembers forever.

### 📄 Reads what you read

Attach a PDF, a Word doc, an Excel spreadsheet, a PowerPoint deck, source code in 80+ formats — BabyBot reads it natively. No copy-paste required.

> *"What does this contract say about termination?"* — reads the PDF
>
> *"Find the highest Q3 number in this spreadsheet."* — reads the Excel
>
> *"Summarize this slide deck."* — reads the PowerPoint
>
> *"What's the main argument in this report?"* — reads the Word doc
>
> *"Explain what this code does."* — reads the source

Plus document **generation** in the other direction: BabyBot can write a PDF from your conversation, save it to your Downloads folder, hand it back as a tappable file chip in chat.

### 🔍 Does real work

Beyond chat, BabyBot has a real tool layer that gets used during agentic runs:

- **Multi-stage web research** — query planning, parallel fetching, content extraction, ranking, deduplication. Not a toy search wrapper.
- **Planner / worker mode** — frontier model writes a spec, cheaper local model executes against it. Frontier-quality decisions at local-token cost.
- **Continue button** — long agentic tasks resume across sessions, app restarts, even days later
- **Persistent context** — your conversations save locally, encrypted at rest
- **Built-in file editor** — view, edit, save files inside BabyBot. IDE-lite on your phone.
- **Image generation** — synthesize new images when you ask for them
- **Live map rendering** — any location, inline in chat

### 🔌 Bring your own LLM

Six backends. Configure them all. Switch between them mid-conversation.

- **Anthropic** — Claude family
- **OpenAI** — GPT-4 family, GPT-5, o-series
- **Google Gemini** — Pro and Flash
- **OpenRouter** — access dozens of models through one API
- **Ollama** — point at any local server (LAN or DDNS), use any model: Gemma, Llama, Qwen, Mistral, GLM, anything
- **Custom** — any OpenAI-compatible endpoint

Drop a frontier model for hard reasoning, switch to a local model for cheap follow-ups, keep the same conversation and tools. The agent stack doesn't care which provider answers.

If you only use Ollama with web search disabled, **nothing leaves your network.**

---

## Everything here ships, working, polished

BabyBot doesn't roadmap features it hasn't built. Nothing in this README is a placeholder, a stub, or a half-finished experiment. If a capability is listed, it's because it survived the polish cycle before shipping to beta.

In a Play Store full of vibe-coded weekend AI apps where half the buttons don't work, that matters.

---

## 🥚 Easter eggs

This README covers the headline capabilities. **There's more.** A lot more. Some tools and behaviours are intentionally undocumented because finding them is part of the fun.

Just ask BabyBot for things. If a capability is there, it'll use it. If it isn't, BabyBot will tell you honestly. The full surface area is bigger than this page — discover the rest by talking to your phone.

---

## Privacy

- **No analytics from BabyBot itself.** No third-party SDK, no crash reporter that phones home, no advertising IDs.
- **No ads.** Not now, not planned, not ever.
- **Cloud LLM calls** go directly to whichever provider you configured. There is no BabyBot server in the middle.
- **Local mode** with Ollama + web search off = nothing leaves your network.
- **Location, contacts, photos** are opt-in per-feature. Default off.
- **API keys** are stored encrypted on-device.
- **Google ML Kit anonymous SDK telemetry** is present because it ships with ML Kit's bundled libraries — same as every other Android app that uses Google's on-device ML stack. Not user data, just SDK metrics. Planned to be replaced with raw TFLite in a future version to eliminate this.

---

## Roadmap

**Coming next:**
- Faster screen reading (the main UX bottleneck right now — multi-step automation can take 40-60s, target is under 15s)
- WorkManager-backed background ML downloads
- Per-component sub-image matching via on-device object detection

**On the horizon:**
- **Android Auto support** — voice-driven AI agent in your car. BYO LLM. Privacy-first alternative to Google Assistant in the dashboard.
- MobileCLIP integration for substantially better semantic image matching
- Improved local-model tool-call reliability across the Ollama model zoo

**Long-term:**
- iOS port (no committed timeline)
- Multi-modal voice (speak with feeling, not just transcribe)

---

## Join the closed beta

Fastest way to get BabyBot on your phone:

1. **Hop into the Discord** for chat, support, demos, and direct feedback → [discord.gg/qaSrVm2cAg](https://discord.gg/qaSrVm2cAg)
2. **Join the testers group** (required for Play Store closed-test access) → [groups.google.com/g/cutitas85](https://groups.google.com/g/cutitas85)
3. Wait ~5 minutes for the group access to propagate
4. **Open the Play Store link** → [play.google.com/store/apps/details?id=baby.bot.app](https://play.google.com/store/apps/details?id=baby.bot.app)
5. Install, configure a backend (point at your Ollama or paste an API key), say hi in Discord

**Looking for feedback specifically on:**
- Real-world image identification — products, places, plants, components, anything
- Voice-driven multi-app automation — WhatsApp, Gmail, Maps, alarms, contacts
- Local Ollama setup pain points
- Battery / performance with the embedding tier active
- Any tool-call weirdness across different local models
- Any document-reading edge cases (heavily-formatted PDFs, multi-sheet Excels)

Feedback channels: the **Discord** (fastest, best for back-and-forth), Play Store internal-testing feedback, or open an issue on this repo.

---

## About

BabyBot is built by [Robert Nilges / PixCats](https://github.com/PixCats)

Other projects:
- **[SteamWidget](https://github.com/PixCats/SteamWidget)** — Steam stats widget for Windows
- **MyLuck** — also on the Play Store

---

## License

**BabyBot is closed-source software.** This repository exists as a public face for the project — documentation, issue tracking, release notes, beta coordination. The application source code is not published.

Compiled APKs are distributed exclusively through Google Play (currently in closed testing). Sideloaded redistribution of the APK is not authorized. Reverse engineering, decompilation, and disassembly are prohibited except where expressly permitted by applicable law.

Copyright © 2025–2026 Robert Nilges / PixCats. All rights reserved.

---

<div align="center">

**Built in Android Studio, with too much coffee** ☕

</div>
