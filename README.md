# Podcast → Obsidian Transcriber

A browser-based tool that transcribes podcast episodes and outputs [LLM Wiki](https://github.com/wanderloots-tutorials/vibe-coding/blob/main/wanderloots-llm-wiki-core-setup-v1.0.0.md)-compatible Raw Source notes for your Obsidian vault.

**Live app:** https://jaketverberg.github.io/podcast-transcriber/

---

## Features

- Paste an Apple Podcasts URL to pull episode metadata automatically
- Drag & drop (or upload) any local audio file
- Transcription runs entirely in your browser via [Whisper](https://openai.com/research/whisper) — no accounts, no API keys, no cost
- Outputs a `.md` file with LLM Wiki frontmatter, ready to drop into `Raw/Sources/`
- Timestamps included in the transcript

## How to use

### From an Apple Podcasts URL

1. Copy the episode URL from Apple Podcasts (e.g. `https://podcasts.apple.com/us/podcast/...`)
2. Paste it into the URL field and click **Fetch**
3. Review the episode details, then click **Transcribe**
4. Wait for the download + transcription to complete (see progress bar)
5. Click **Download .md** and save the file to `Raw/Sources/` in your vault

### From a local audio file

1. Drag & drop an audio file onto the upload zone (or click to browse)
2. Click **Transcribe**
3. Download the output `.md` file

Supported formats: MP3, M4A, WAV, OGG, FLAC, AAC

## Model quality

You can choose the Whisper model before transcribing:

| Model | Download size | Speed | Accuracy |
|-------|--------------|-------|----------|
| Tiny  | ~80 MB | Fastest | Lower |
| **Base** (default) | ~145 MB | Good | Good |
| Small | ~460 MB | Slower | Best |

The model is cached in your browser after the first download — subsequent uses load instantly.

## Output format

Each transcript is saved as a Raw Source note matching the LLM Wiki schema:

```markdown
---
Title: "Episode Title"
Author: "Author Name"
Reference: "https://podcasts.apple.com/..."
ContentType:
  - "audio-transcript"
Created: YYYY-MM-DD
Processed: false
tags:
  - "source"
---

# Episode Title

**Podcast:** Podcast Name
**Author:** Author Name
**Date:** YYYY-MM-DD
**Duration:** 45:23

## Transcript

[0:00] Transcript text with timestamps...
```

After saving to `Raw/Sources/`, register it with your wiki tooling:

```bash
python3 scripts/wiki_tool.py source-scan --update --accept-covered
```

## Privacy

- Audio is processed locally by Whisper running as WebAssembly in your browser
- Audio files are never uploaded to any server
- When a podcast host blocks direct browser access (CORS restriction), the app automatically routes the audio download through [corsproxy.io](https://corsproxy.io) — only the file download passes through the proxy, transcription still happens locally

## Deploying your own copy

1. Fork this repo
2. Go to **Settings → Pages** → Source: `main` branch, `/ (root)`
3. Your copy will be live at `https://YOUR_USERNAME.github.io/podcast-transcriber/`
