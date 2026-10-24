# Dolphin-ScoobyAudio (unofficial)

Unofficial Windows build of [Dolphin Emulator](https://dolphin-emu.org/) `2609a` with a one-file DSP HLE change. Not an official Dolphin release and not affiliated with the Dolphin project.

Simple version: `Dolphin-ScoobyAudio.exe` stops the crackling audio (steady ticks in the music) you get in *Scooby-Doo! Night of 100 Frights* on stock Dolphin with HLE.

## What this fixes

On stock Dolphin DSP HLE, *Scooby-Doo! Night of 100 Frights* (GameCube, `GIHE78`) can have ticks or crackle in the background music, often on a steady beat (about every 0.875 seconds in outdoor areas like Mystic Playground).

The game streams level music as ADPCM in a small ring buffer in ARAM. When the voice loops, HLE was reloading predictor/scale from the voice block’s `adpcm_loop_info`. For stream voices the game does not keep that field updated (it stays 0), while the real header in ARAM is not. Every loop wrap decoded the first block wrong → a click in the music.

This build, on stream loop, reads predictor/scale from the ADPCM block header at the loop address instead. Non-stream loops and PCM are unchanged.

## Credit

The bug and fix were nailed down in [Scooby-Doo Refrightened PR #11](https://github.com/josiahg2502-glitch/Scooby-Doo-Refrightened/pull/11) by [Loneranger419](https://github.com/Loneranger419) (native recomp runtime). I ported the same idea to Dolphin’s `AXVoice.h` for my own GameCube copy. Thanks to Loneranger for the write-up and before/after captures.

## Download

[Releases](https://github.com/Easy301/dolphin-scooby-hle-audio/releases) — `Dolphin-ScoobyAudio.exe` only (built from the patch in this repo at the matching release tag). Drop it next to your existing Dolphin `2609a` `Dolphin-x64` folder (same Qt DLLs and `Sys` as stock). Your profile stays in `%AppData%\Dolphin Emulator`. Keep `Dolphin.exe` for other games; use `Dolphin-ScoobyAudio.exe` for Scooby (or anywhere you want this HLE path).

- DSP: leave HLE (default). LLE is not required for this fix.
- ROMs / BIOS: not included. Use only media you own and your own backups.

## Build from source

See [BUILD.md](BUILD.md). Base tag: `2609a` on [dolphin-emu/dolphin](https://github.com/dolphin-emu/dolphin). Patch: [patches/dolphin-2609a-ax-adpcm-stream-loop.patch](patches/dolphin-2609a-ax-adpcm-stream-loop.patch).

Rename the built `Dolphin.exe` to `Dolphin-ScoobyAudio.exe` so it is clearly an unofficial distribution (Dolphin’s build system also suggests giving redistributed builds a distinct name).

## Legal / Dolphin etiquette

- License: Derivative of Dolphin under GNU GPL v2 or later. Full text: [COPYING](COPYING). This repo’s patch and build notes are the corresponding source for the released binary (GPL §3). Rebuild from `2609a` + patch if you want to verify the exe.
- Unofficial: Do not present this as dolphin-emu.org or “official” Dolphin. The Dolphin name and logo belong to the Dolphin project.
- Upstream: If Dolphin merges an equivalent fix, use official builds and retire this fork.

---

[Gregory's project hub](https://github.com/Easy301/gregory-project-hub)
