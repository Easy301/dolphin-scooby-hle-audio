# Dolphin build — Scooby-Doo! Night of 100 Frights (music HLE fix)

**Private backup repo** for an unofficial Windows build of [Dolphin Emulator](https://dolphin-emu.org/) **2609a** with a one-file DSP HLE change. Not affiliated with the Dolphin project.

## What this fixes

On stock Dolphin **DSP HLE**, *Scooby-Doo! Night of 100 Frights* (GameCube, `GIHE78`) can have **ticks or crackle in the background music**, often on a steady beat (about every 0.875 seconds in outdoor areas like Mystic Playground).

The game streams level music as **ADPCM in a small ring buffer** in ARAM. When the voice loops, HLE was reloading predictor/scale from the voice block’s `adpcm_loop_info`. For stream voices the game **does not keep that field updated** (it stays 0), while the real header in ARAM is not. Every loop wrap decoded the first block wrong → a click in the music.

This build, on stream loop, reads predictor/scale from the **ADPCM block header at the loop address** instead. Non-stream loops and PCM are unchanged.

## Credit

The bug and fix were nailed down in **[Scooby-Doo Refrightened PR #11](https://github.com/josiahg2502-glitch/Scooby-Doo-Refrightened/pull/11)** by **[Loneranger419](https://github.com/Loneranger419)** (native recomp runtime). I ported the same idea to Dolphin’s `AXVoice.h` for my own GameCube copy. Thanks to Loneranger for the write-up and before/after captures.

## Download (private releases)

**[Releases](https://github.com/Easy301/dolphin-scooby-hle-audio/releases)** — `Dolphin-ScoobyAudio.exe` only. Drop it next to your existing **Dolphin 2609a** `Dolphin-x64` folder (same Qt DLLs and `Sys` as stock). Your profile stays in `%AppData%\Dolphin Emulator`. Keep `Dolphin.exe` for other games; use `Dolphin-ScoobyAudio.exe` for Scooby (or anywhere you want this HLE path).

- **DSP:** leave **HLE** (default). LLE is not required for this fix.
- **ROMs:** not included. You need your own dump (e.g. RVZ/ISO).

## Build from source

See [BUILD.md](BUILD.md). Base tag: **`2609a`** on [dolphin-emu/dolphin](https://github.com/dolphin-emu/dolphin). Patch: [patches/dolphin-2609a-ax-adpcm-stream-loop.patch](patches/dolphin-2609a-ax-adpcm-stream-loop.patch).

## Legal / Dolphin project rules

- **License:** This patch is part of a derivative of Dolphin and is under **GNU GPL v2 or later**, same as upstream. Full text: [COPYING](COPYING).
- **Source:** This repo has the patch and build notes. That is the corresponding source for the released binary (GPL §3). For a full tree, apply the patch to Dolphin `2609a` and build per BUILD.md.
- **Trademark:** “Dolphin Emulator” and the Dolphin logo are the Dolphin project’s. This repo is **not** an official Dolphin build. Do not imply endorsement.
- **BIOS / dumps:** Dolphin does not ship game media. You must own discs and use your own backups.
- **Upstream:** If Dolphin merges an equivalent fix, prefer official builds and drop this fork.

## Going public later

Repo is private for now. Before making it public: keep COPYING + patch + BUILD.md, keep release binaries tied to a tagged commit, and add a short note in the release that it is unofficial.

---

[Gregory's project hub](https://github.com/Easy301/gregory-project-hub)
