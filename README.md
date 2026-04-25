# Voice in the Wild

An open-source resource hub for builders working on **voice AI that holds up outside the lab** - noisy rooms, bad mics, slurred speech, overlapping speakers, hostile environments.

Originally curated for the [Big Berlin Hack 2026](https://luma.com/bigberlinhack) "Voice Interface in the Wild" track, but useful for anyone shipping voice apps to the real world.

## What's in here

| File | What it covers |
|---|---|
| [`TOOLKIT.md`](./TOOLKIT.md) | Categorized list of open-source repos: audio understanding, speech quality, ASR/VAD, DSP, augmentation, agent platforms |
| [`quality-dashboard/README.md`](./quality-dashboard/README.md) | How to wire up a "quality dashboard" (DNSMOS + WER + VAD-miss + LUFS) in under an hour |
| [`examples/README.md`](./examples/README.md) | Six concrete project ideas with the right tools picked out for each |
| [`DATASETS.md`](./DATASETS.md) | Noise, RIR, and stress-test datasets you can break your pipeline with |
| [`CONTRIBUTING.md`](./CONTRIBUTING.md) | How to add a tool, fix a broken link, or suggest a new use case |

## The challenge (from the track brief)

> Voice-driven apps work flawlessly in the lab but fall apart the moment they meet the real world. Background chatter, low-quality microphones, slurred speech, and unpredictable environments kill the user experience. Most teams discover these failures only after shipping.

**Build a voice-driven application that survives real-world harsh audio conditions.** Pick your own adventure - agent, game, hardware, robot - and prove your app holds up when the environment fights back.

## If you only install one thing

[**VERSA**](https://github.com/wavlab-speech/versa) - a unified evaluation CLI that wraps DNSMOS, UTMOS, PLCMOS, PESQ, STOI, VISQOL, WER, and speaker similarity behind a single command. Gets you 80% of the way to a quality dashboard.

## Recommended starter stack

A "quality dashboard" you can wire up in under an hour. Each row maps to a different failure mode you'll want to measure.

| Signal | Tool | Notes |
|---|---|---|
| Non-intrusive MOS | [DNSMOS](https://github.com/microsoft/DNS-Challenge/tree/master/DNSMOS) | De-facto ref-free quality metric (SIG/BAK/OVRL) |
| MOS decomposition | [NISQA](https://github.com/gabrielmittag/NISQA) | Noise / coloration / discontinuity / loudness |
| ASR degradation | [faster-whisper](https://github.com/SYSTRAN/faster-whisper) + [jiwer](https://github.com/jitsi/jiwer) | WER vs. clean reference |
| VAD miss-rate | [silero-vad](https://github.com/snakers4/silero-vad) | Tiny, pip-installable, torch-hub callable |
| Loudness / usability | [pyloudnorm](https://github.com/csteinmetz1/pyloudnorm) | LUFS + true peak (ITU-R BS.1770) |
| Unified runner | [VERSA](https://github.com/wavlab-speech/versa) | Single CLI for all of the above + more |

See [`quality-dashboard/README.md`](./quality-dashboard/README.md) for the wiring guide.

## How to use this repo

- **Browsing for ideas?** Start with [`examples/`](./examples/).
- **Need a specific tool?** Jump to [`TOOLKIT.md`](./TOOLKIT.md) and Ctrl-F.
- **Building a benchmark?** Read [`quality-dashboard/`](./quality-dashboard/).
- **Need to break your pipeline on purpose?** See [`DATASETS.md`](./DATASETS.md) and the augmentation section of `TOOLKIT.md`.

## Contributing

PRs welcome. See [`CONTRIBUTING.md`](./CONTRIBUTING.md). Broken links, missing tools, and better one-liners are all fair game.

## License

MIT - see [`LICENSE`](./LICENSE). Curated descriptions and structure are MIT-licensed; linked tools retain their own licenses (check before shipping).
