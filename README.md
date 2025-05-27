# CMPM-147-Final-Project

## Gibberish Voice Synth

> A tiny web tool that turns any line of text into a fast‑talking “babble” clip ‑ completely offline and perfectly repeatable.

---

\## 📖 Overview
This project accepts a **text line** and a **seed** (typed or randomised), then synthesises a short string of beeps whose pitch, speed, and timbre are derived from that seed.
The same **text + seed** input will *always* recreate the exact same audio, making the tool ideal for placeholder dialogue, NPC voices, or light‑hearted role‑play.

\## 🧩 Components

| Part         | Description                                                                                                                                     |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Web UI       | Single‑page app (HTML + CSS + JS). Controls: text box, seed box / 🎲 random button, **Generate**, **Play / Stop**, **Download WAV** (optional). |
| Seed → RNG   | The seed string is hashed with **xxhash32** to a 32‑bit integer, then fed to a lightweight PRNG (e.g. `mulberry32`).                            |
| Audio Engine | **Web Audio API** with an `OfflineAudioContext` for fully deterministic rendering.                                                              |
| Base Sample  | One 50–100 ms “beep” encoded inline (Base‑64). Each character in the text re‑uses this sample with different playback rate / gain.              |
| Export       | A tiny `encodeWav()` helper wraps the rendered buffer into a 44.1 kHz / 16‑bit mono WAV for download.                                           |

\## ⚙️ Process

1. **Seed hashing** → 32‑bit ID.
2. Derive a *voice DNA* (base pitch, speed, FX flags).
3. For every character in the string, schedule the base beep with calculated pitch, duration, and gap.
4. Render offline, apply rare “mutations” (reverse, bit‑crush, stereo swap), then expose the clip for playback & download.

\## 🎯 Purpose
*Give every throw‑away character a voice without opening a DAW.*
Designers can iterate on scripts while guaranteeing that the final build reproduces the same sounds.
Exploring seeds is half the fun: some yield chipmunk chatter, others a gravelly robot—and a lucky few trigger weird mutations for easter‑egg variety.

\## 📋 Requirements / MVP

* Deterministic output for identical **text + seed**.
* Runs entirely in the browser—no servers, no external APIs.
* Minimal UI (text, seed, generate, play).
* Optional: **Download WAV** and **Play / Stop** toggle.

