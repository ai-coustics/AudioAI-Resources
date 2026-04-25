# Datasets

For breaking your pipeline on purpose and stress-testing it under realistic conditions.

## Noise

- [DNS-Challenge](https://github.com/microsoft/DNS-Challenge) — huge curated noise + RIR dataset from Microsoft. The canonical pick.
- [WHAM!](http://wham.whisper.ai/) — noise mixtures designed for stress testing.
- [WHAMR!](http://wham.whisper.ai/) — WHAM! plus reverb. Stricter test conditions.
- [ESC-50](https://github.com/karolpiczak/ESC-50) — 2,000 environmental sound clips, 50 classes. Small, easy to work with.
- [FSD50K](https://zenodo.org/records/4060432) — 51,000 Freesound-sourced clips, much bigger than ESC-50.
- [Dawn Chorus](https://dawn-chorus.org/) — natural soundscapes. Good for realistic backgrounds.
- [AudioSet](https://research.google.com/audioset/) — Google's 2M+ clip ontology. Use with care; download via [audioset-downloader](https://github.com/marl/audiosetdl).

## Room impulse responses (reverb)

- [DNS-Challenge RIRs](https://github.com/microsoft/DNS-Challenge) — curated RIR set bundled with the noise data.
- [BUT Speech@FIT Reverb DB](https://speech.fit.vutbr.cz/software/but-speech-fit-reverb-database) — real-world RIRs from a variety of rooms.
- [OpenSLR RIRs](https://www.openslr.org/28/) — simulated and real RIRs, classic baseline.

## Speech (clean reference)

- [LibriSpeech](https://www.openslr.org/12) — 1000h read English speech.
- [Common Voice](https://commonvoice.mozilla.org/) — multilingual, accented, varied quality.
- [VCTK](https://datashare.ed.ac.uk/handle/10283/3443) — 110 English speakers, multiple accents.
- [VoxCeleb](https://www.robots.ox.ac.uk/~vgg/data/voxceleb/) — speaker recognition, lots of speakers, in-the-wild conditions.

## Degraded / accented / "in-the-wild" speech

- [SLURP](https://github.com/pswietojanski/slurp) — multi-domain spoken language understanding, real-world recordings.
- [L2-ARCTIC](https://psi.engr.tamu.edu/l2-arctic-corpus/) — non-native English speech, 24 speakers, 6 L1s.
- [TORGO](https://www.cs.toronto.edu/~complingweb/data/TORGO/torgo.html) — dysarthric speech (cerebral palsy / ALS). Important for accessibility testing.
- [UA-Speech](http://www.isle.illinois.edu/sst/data/UASpeech/) — dysarthric speech corpus.

## Tip: combining them

The standard recipe for stress-testing a voice pipeline:

1. Pick a clean speech corpus (LibriSpeech, Common Voice).
2. Mix in noise from DNS-Challenge / WHAM! at a target SNR (e.g., 0, 5, 10, 15, 20 dB).
3. Convolve with RIRs to add reverb.
4. Optionally apply codec / bandwidth reduction with `pedalboard` or `audiomentations`.
5. Run your pipeline. Measure WER vs. clean reference + non-intrusive MOS.

The [`quality-dashboard/`](./quality-dashboard/) folder shows how to wire this up.
