# BabyBot

**A fully agentic AI that lives on your Android phone.**

You speak. BabyBot listens. It plans multi-step tasks and carries them out — reading and writing documents, identifying what's in your photos, handling your email, researching the web, and remembering the things and people in your life — with your choice of LLM, cloud or local, making the decisions.

> *"Call my brother."*
>
> *— Finds the contact, spots that two saved numbers are the same, opens the dialer ready to go. You tap call.*

BabyBot does the legwork. **You stay in control of anything that leaves your phone.**

[![Discord](https://img.shields.io/badge/Discord-Join_the_community-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/qaSrVm2cAg)
[![Play Store](https://img.shields.io/badge/Get_it_on-Play_Store-blue?style=for-the-badge&logo=googleplay)](https://play.google.com/store/apps/details?id=baby.bot.app)

**Available now on Google Play for everyone.**

---

## Why this is different

There are a lot of AI apps on the Play Store. Most are chat wrappers. BabyBot isn't.

| Capability | ChatGPT app | Gemini | Claude app | Bixby / Assistant | **BabyBot** |
|---|:-:|:-:|:-:|:-:|:-:|
| Cloud LLM chat | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Run on local LLM (Ollama)** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Voice-driven hands-free** | partial | ✅ | partial | ✅ | ✅ |
| **Reads PDF / Word / Excel / PowerPoint** | ❌ | partial | ❌ | ❌ | ✅ |
| **Persistent visual memory of your world** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Hybrid web image identification** | ❌ | ❌ | ❌ | ❌ | ✅ |

The combination is the point. Lots of apps do one or two cells. BabyBot does the whole table.

---

## What it feels like to use

A morning with BabyBot, roughly:

- *"What's on my calendar today and the weather?"* — answered in one breath
- *"Let's go home"* — turn-by-turn navigation starts in Google Maps
- *"Text my wife that I'll be late"* — Messages opens with the contact and text ready, you tap send
- *"Call my brother"* — dialer opens with the right number, you tap call
- *"Email this summary to my work address"* — sent from your own email account, no app switching
- *"What's in this PDF?"* (attached) — summary in plain English, key points pulled out
- *"What component is this?"* (photo attached) — identifies the part, finds the spec sheet, summarizes
- *"Is this my dog?"* (photo attached) — checks against your saved photos, knows
- *"Set a timer for 25 minutes"* — done
- *"Remember this as my motorbike"* — saved. Next month when you photograph it from a different angle, BabyBot knows it's yours.

---

## The headline capabilities

### 📱 Gets things done on your phone

BabyBot prepares real actions and leaves the final tap to you:

- **Calls** — finds the contact (and sorts out duplicate numbers), opens the dialer ready to call
- **SMS** — opens Messages with the recipient and your text already written
- **Navigation** — starts turn-by-turn Google Maps navigation to wherever you ask
- **Location & weather** — where you are, current conditions and forecast anywhere, maps rendered inline (photos with GPS get a map too)
- **Alarms, timers, volume, battery, camera, contacts, opening any app** — from a voice command

Nothing gets sent or dialled behind your back. BabyBot does the work, and you confirm.

### ✉️ Your email, handled

Connect your own email accounts and BabyBot can send, read and answer mail for you.

- **Multiple accounts** — e.g. work and personal, each switched on or off on its own. Run both, or either.
- **Sending over SMTP** — quick setup or manual (STARTTLS / SSL/TLS), straight from your own account. Plain text or rich HTML, with attachments. No BabyBot server in between.
- **Reading over IMAP (optional)** — BabyBot can read and reply to incoming mail, using the same login.
- **Auto-reply — off by default.** Set it per account:
  - *Off* — manual only
  - *Allowlist only* — replies only to senders you've approved
  - *Anyone (business mode)* — answers incoming mail for you, e.g. a workshop inbox
- **Every new sender needs your one-tap approval first.**
- **Your rules** — give it instructions (*"Open Mon–Sat 9–18. Never quote prices. Sign as Robert."*) and optional branded HTML templates where the AI only fills in the variables.

Email is fully optional and can be turned off in Settings.

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

Save photos of the things and people that matter to you — your pet, your car, your kids, the components in your workshop, your friends' faces. BabyBot builds a local visual memory unique to you. Show it a new photo later — days, months, years — and it knows whether that's your dog Max on the sofa, or just *a* dog.

No training required. No cloud round-trips. No accounts. Add a photo, it's instantly recognizable. Add 20 photos across different angles and lighting, it gets robust.

### 📄 Documents and files — made on your phone

Attach a PDF, a Word doc, an Excel spreadsheet, a PowerPoint deck, or source code in 80+ formats — BabyBot reads it natively.

> *"What does this contract say about termination?"* — reads the PDF
>
> *"Find the highest Q3 number in this spreadsheet."* — reads the Excel
>
> *"Summarize this slide deck."* — reads the PowerPoint

And it works in the other direction too. **Files are created directly on your device** — not in anyone's cloud:

- **Create** `.txt .md .py .html .json .csv .pdf .docx .xlsx` and more
- **Edit** — surgical text replacements or full rewrites; DOCX/XLSX edits keep the original formatting
- **Convert** between PDF, DOCX, XLSX, PPTX and text
- **PDF tools** — extract, delete, rotate or render pages; pull embedded images out as PNGs
- Everything lands in **`Downloads/BabyBot/`** and comes back as a tappable file chip in chat

**BabyBot Editor** — a built-in editor with syntax highlighting and line numbers. It registers with Android's *Open with* menu, so you can open, edit and save files with BabyBot from anywhere on your phone.

**Select → Ask BabyBot** — highlight any part of a file in the editor — a few lines of code, a paragraph of a document, a clause in a contract — and tap *Ask BabyBot*. The selection drops into chat as a reference chip (e.g. `meow.py:11-13`), and you can ask BabyBot to explain it, summarise it, fix it or rewrite it.

### 🎨 Images and video

- **Image generation** — square, wide or portrait, through Gemini, OpenAI or OpenRouter in the cloud, or your own rig (ComfyUI, Stable Diffusion WebUI, NVIDIA NIM)
- **Video generation** — 4–12 second AI clips via Veo (Gemini), Sora (OpenAI-compatible) or ComfyUI, with in-app playback and thumbnail cards. Takes 1–3 minutes.
- Any chat model can trigger image or video generation — the media engine handles the rest

> ✅ **Cloud generation through OpenAI and Gemini is tested and works very well** — the recommended route.
>
> ⚠️ **Beta:** image and video generation on **local** backends (ComfyUI, Stable Diffusion WebUI) hasn't been fully tested yet. Expect rough edges — and please report what you find in the Discord.

### 🔍 Fully agentic — does real work

- **Plans and executes** — multi-step tasks get broken into ordered steps, each tracked as it runs, so long jobs don't fall apart halfway
- **Multi-stage web research** — query planning, parallel sub-questions, content extraction, host-reputation scoring, ranking, deduplication. Quick (3 sources), normal (6) or deep (12), with specialised modes for code, docs, GitHub and Stack Overflow
- **Real photos, not AI drawings** — "show me X" returns actual web images alongside the answer
- **Recall** — re-read an earlier search without re-running it, even after a restart
- **Baby Assist (planner / worker mode)** — a frontier model writes the plan, a cheaper or local model carries it out. Frontier-quality decisions at local-token cost.
- **Continue button** — long agentic tasks resume across sessions, app restarts, even days later
- **Persistent context** — your conversations save locally, encrypted at rest

### 🔌 Bring your own LLM

Six backends. Configure them all. Switch between them mid-conversation.

- **Anthropic** — Claude family
- **OpenAI** — GPT family and o-series
- **Google Gemini** — Pro and Flash
- **OpenRouter** — dozens of models through one API
- **Ollama** — point at any local server (LAN or DDNS), use any model
- **Custom** — any OpenAI-compatible endpoint

First launch walks you through **setting up a model**, so you're chatting within a minute.

---

## ⚠️ Honest limits

Things people assume BabyBot can do, but it can't:

- ❌ **Type into WhatsApp, Instagram, Telegram, etc.** — it can open the app, but not type into it
- ❌ **Auto-send calls or SMS** — it pre-fills, you tap send
- ❌ **See your screen** — it only sees images you attach
- ❌ **Read your gallery or files outside `Downloads/BabyBot/`** — except attachments you send it
- ❌ **Run terminal commands** — no shell on Android
- ❌ **Pay for things or take banking actions**

---

## 🥚 Easter eggs

This README covers the headline capabilities. **There's more.** Some tools and behaviours are intentionally undocumented because finding them is part of the fun.

Just ask BabyBot for things. If a capability is there, it'll use it. If it isn't, BabyBot will tell you honestly.

---

## Privacy

- **BabyBot is free and supported by ads** (Google AdMob). Ads are served by Google, which may use your device's advertising ID — see Google's privacy policy for details.
- **No analytics from BabyBot itself.** No BabyBot server, no crash reporter of our own.
- **Your files stay on your device.** Everything BabyBot creates is saved to `Downloads/BabyBot/` on your phone — never uploaded to a BabyBot cloud.
- **Cloud LLM calls** go directly to whichever provider you configured. There is no BabyBot server in the middle. What you send to a cloud provider (OpenAI, Google, Anthropic, OpenRouter…) is handled under *their* privacy policy and is outside BabyBot's control — use Ollama if you want to keep it local.
- **Local mode** with Ollama + web search off keeps your conversations on your own network (ad requests still go to Google).
- **Email** goes directly between your phone and your own mail server. Off unless you add an account.
- **Location, contacts, photos** are opt-in per feature. Default off.
- **API keys and email passwords** are stored encrypted on-device.
- **Google ML Kit** includes anonymous SDK telemetry as part of Google's bundled libraries. Planned to be replaced with raw TFLite in a future version.

<!-- Add links here once hosted: Privacy policy · Terms of Service -->

---

## Roadmap

**Coming next:**
- WorkManager-backed background ML downloads
- Per-component sub-image matching via on-device object detection

**On the horizon:**
- **Android Auto support** — voice-driven AI agent in your car. BYO LLM. Privacy-first alternative to Google Assistant in the dashboard.
- MobileCLIP integration for better semantic image matching
- Improved local-model tool-call reliability across the Ollama model zoo

**Long-term:**
- iOS port (no committed timeline)
- Multi-modal voice (speak with feeling, not just transcribe)

---

## Get BabyBot

1. **Install from Google Play** → [play.google.com/store/apps/details?id=baby.bot.app](https://play.google.com/store/apps/details?id=baby.bot.app)
2. **Set up a model** — point at your Ollama server or paste an API key
3. **Say hi in the Discord** → [discord.gg/qaSrVm2cAg](https://discord.gg/qaSrVm2cAg)

**Feedback especially welcome on:**
- Real-world image identification — products, places, plants, components
- Email auto-reply in real inboxes
- Local Ollama setup pain points
- Battery / performance with the embedding tier active
- Tool-call weirdness across different local models
- Document-reading edge cases (heavily formatted PDFs, multi-sheet Excels)

Feedback channels: the **Discord** (fastest), a Play Store review, or open an issue on this repo.

---

## About

BabyBot is built by [Robert Nilges / PixCats](https://github.com/PixCats)

Other projects:
- **[SteamWidget](https://github.com/PixCats/SteamWidget)** — Steam stats widget for Windows
- **MyLuck** — also on the Play Store

---

## License

**BabyBot is closed-source software.** This repository is the public face of the project — documentation, issue tracking, release notes. The application source code is not published.

Compiled APKs are distributed exclusively through Google Play. Sideloaded redistribution of the APK is not authorized. Reverse engineering, decompilation, and disassembly are prohibited except where expressly permitted by applicable law.

Copyright © 2025–2026 Robert Nilges / PixCats. All rights reserved.

---

<div align="center">

**Built in Android Studio, with too much coffee** ☕

</div>
