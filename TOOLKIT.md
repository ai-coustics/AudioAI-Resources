# Audio Intelligence Toolkit

A curated set of open-source repos for building voice-driven apps. Pick what you need, ignore the rest. Every entry is described in one line so you can scan and decide in seconds.

## Contents

- [Audio understanding](#audio-understanding) — extract information from the signal
- [Speech quality & intelligibility metrics](#speech-quality--intelligibility-metrics)
- [ASR / WER / VAD](#asr--wer--vad)
- [DSP & features](#dsp--features)
- [Noise, reverb, augmentation](#noise-reverb-augmentation--break-your-own-pipeline)
- [End-to-end toolkits](#end-to-end-toolkits)
- [Voice agent platforms](#voice-agent-platforms)
- [Speech enhancement / cleanup](#speech-enhancement--cleanup)
- [Beyond the usual metrics](#beyond-the-usual-metrics)
- [Further reading](#further-reading)

---

## Audio understanding

Turning raw audio into **structured information**: what's happening, who's speaking, how they feel, what sounds are present.

### Sound event & scene classification — "what is happening?"

- [PANNs / CNN14](https://github.com/qiuqiangkong/audioset_tagging_cnn) — pre-trained on AudioSet, 527 classes. De-facto baseline.
- [YAMNet](https://tfhub.dev/google/yamnet/1) — Google, 521 AudioSet classes, available on TF Hub / ONNX.
- [AST (Audio Spectrogram Transformer)](https://github.com/YuanGongND/ast) — transformer-based, HuggingFace-loadable.
- [PaSST](https://github.com/kkoutini/PaSST) — efficient patch-out AudioSet classifier.
- [BEATs](https://github.com/microsoft/unilm/tree/master/beats) — SOTA AudioSet classification and representations (Microsoft).
- [CLAP / LAION-CLAP](https://github.com/LAION-AI/CLAP) — zero-shot classification via text prompts ("is this a baby crying?").

### Emotion & paralinguistics — "how does the speaker feel?"

- [audeering wav2vec2](https://huggingface.co/audeering/wav2vec2-large-robust-12-ft-emotion-msp-dim) — continuous arousal / dominance / valence.
- [SpeechBrain emotion-recognition](https://huggingface.co/speechbrain/emotion-recognition-wav2vec2-IEMOCAP) — categorical: neutral / happy / sad / angry.
- [HuBERT-SUPERB-ER](https://huggingface.co/superb/hubert-large-superb-er) — SUPERB emotion benchmark fine-tune.
- [openSMILE](https://github.com/audeering/opensmile) — classic feature extractor (GeMAPS, ComParE). Strong baseline for any paralinguistic classifier.
- [parselmouth](https://github.com/YannickJadoul/Parselmouth) — Praat from Python: pitch, formants, jitter, shimmer, intensity.

### Speaker identity & attributes

- [SpeechBrain ECAPA-TDNN](https://huggingface.co/speechbrain/spkrec-ecapa-voxceleb) — SOTA speaker verification.
- [Resemblyzer](https://github.com/resemble-ai/Resemblyzer) — lightweight speaker embeddings, great for similarity / clustering.
- [pyannote/embedding](https://huggingface.co/pyannote/embedding) — speaker embeddings from the pyannote family.
- [audeering wav2vec2-age-gender](https://huggingface.co/audeering/wav2vec2-large-robust-6-ft-age-gender) — age regression + gender classification.
- [TitaNet (via NeMo)](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/nemo/models/titanet_large) — production-grade speaker recognition.

### Language, accent, wake-word

- [Whisper language detection](https://github.com/openai/whisper) — 99 languages, built in.
- [VoxLingua107 ECAPA](https://huggingface.co/speechbrain/lang-id-voxlingua107-ecapa) — 107-language identification.
- [openWakeWord](https://github.com/dscripka/openWakeWord) — custom wake-word models trainable in minutes.
- [Picovoice Porcupine](https://github.com/Picovoice/porcupine) — low-power wake-word engine.

### Audio captioning, QA, and audio-LLMs (frontier)

- [Qwen2-Audio](https://github.com/QwenLM/Qwen2-Audio) — chat-style audio understanding. Ask natural-language questions *about* a clip.
- [SALMONN](https://github.com/bytedance/SALMONN) — audio perception combined with LLM reasoning.
- [Pengi](https://github.com/microsoft/Pengi) — audio captioning and QA (Microsoft).

### Music & non-speech analysis

- [essentia](https://github.com/MTG/essentia) — BPM, key, genre, instrument, mood descriptors.
- [madmom](https://github.com/CPJKU/madmom) — beat, onset, tempo, chord, drum transcription.
- [demucs](https://github.com/facebookresearch/demucs) — source separation (isolate voice from music / noise before downstream analysis).

---

## Speech quality & intelligibility metrics

### Reference-based (classics)

- [pesq](https://github.com/ludlows/PESQ) — PESQ (ITU-T P.862), the most-cited speech quality metric.
- [pystoi](https://github.com/mpariente/pystoi) — STOI / ESTOI for intelligibility.
- [pysepm](https://github.com/schmiph2/pysepm) — one-stop shop: SNR, segSNR, fwSegSNR, LLR, WSS, cepstrum distance, Csig / Cbak / Covl.
- [SRMRpy](https://github.com/jfsantos/SRMRpy) — non-intrusive modulation-spectrum ratio, good for dereverb.
- [speechmetrics](https://github.com/aliutkus/speechmetrics) — unified API wrapping PESQ, STOI, BSSEval, MOSNet, SRMR, SI-SDR.
- [torchmetrics (audio)](https://lightning.ai/docs/torchmetrics/stable/audio/) — SI-SDR, PESQ, STOI, SDR, NISQA, DNSMOS. GPU-friendly, drops into PyTorch pipelines.

### Reference-free / non-intrusive — the interesting ones for "in the wild"

- [DNSMOS](https://github.com/microsoft/DNS-Challenge/tree/master/DNSMOS) — Microsoft's P.808 / P.835 (SIG / BAK / OVRL). De-facto non-intrusive MOS.
- [NISQA](https://github.com/gabrielmittag/NISQA) — predicts overall MOS plus noise, coloration, discontinuity, loudness.
- [UTMOSv2](https://github.com/sarulab-speech/UTMOSv2) — SSL-ensemble MOS predictor; VoiceMOS Challenge baseline.
- [TorchAudio-SQUIM](https://pytorch.org/audio/main/tutorials/squim_tutorial.html) — Meta's reference-free estimators for PESQ / STOI / SI-SDR. Ships with torchaudio.
- [Distill-MOS](https://github.com/microsoft/Distill-MOS) — lightweight distilled MOS model, fast on CPU.
- [SCOREQ](https://arxiv.org/abs/2410.06675) — contrastive-regression speech quality (2024).
- [PLCMOS](https://github.com/microsoft/PLC-Challenge) — packet-loss-concealment MOS; ~0.97 Pearson correlation to human ratings.
- [AECMOS](https://arxiv.org/abs/2110.03010) — echo-impairment-specific MOS.
- [WhiSQA](https://arxiv.org/abs/2406.08374) — quality prediction on Whisper encoder features.

### Unified runner

- [VERSA](https://github.com/wavlab-speech/versa) — wraps DNSMOS, UTMOS, PLCMOS, PESQ, STOI, VISQOL, WER, speaker similarity in one CLI. **Start here.**

---

## ASR / WER / VAD

- [jiwer](https://github.com/jitsi/jiwer) — WER, CER, MER, WIL. The standard.
- [faster-whisper](https://github.com/SYSTRAN/faster-whisper) — drop-in ASR for measuring transcription degradation vs. clean.
- [whisperX](https://github.com/m-bain/whisperX) — Whisper + word-level timestamps + diarization.
- [silero-vad](https://github.com/snakers4/silero-vad) — tiny, fast VAD, torch-hub callable.
- [pyannote-audio](https://github.com/pyannote/pyannote-audio) — VAD, diarization, overlap detection. Heavier but powerful.
- [py-webrtcvad](https://github.com/wiseman/py-webrtcvad) — classic, tiny VAD baseline.
- [NoRefER](https://arxiv.org/abs/2306.12153) — referenceless WER proxy via semi-supervised LM fine-tuning.

---

## DSP & features

Build your own metric here.

- [librosa](https://github.com/librosa/librosa) — spectrograms, MFCC, chroma, onset, pitch. Start here for any custom metric.
- [torchaudio](https://pytorch.org/audio/) — tensor-native, GPU-friendly equivalent.
- [essentia](https://github.com/MTG/essentia) — heavy-duty feature extractor: loudness (EBU R128), dynamic complexity, spectral descriptors.
- [pyloudnorm](https://github.com/csteinmetz1/pyloudnorm) — ITU-R BS.1770 LUFS + true peak. Important for any "is the audio usable" check.
- [pyroomacoustics](https://github.com/LCAV/pyroomacoustics) — simulate rooms and reverb; compute RT60, DRR.
- [auraloss](https://github.com/csteinmetz1/auraloss) — differentiable perceptual losses (multi-res STFT, Mel, SI-SDR). Usable as quality proxies.

---

## Noise, reverb, augmentation — break your own pipeline

- [audiomentations](https://github.com/iver56/audiomentations) — easy CPU augmentation API (noise, reverb, gain, pitch shift, codec artefacts).
- [torch-audiomentations](https://github.com/asteroid-team/torch-audiomentations) — GPU version, batch-friendly.
- [pedalboard](https://github.com/spotify/pedalboard) — Spotify's audio effects library. Realistic distortion, compression, EQ.

For datasets to inject noise from, see [`DATASETS.md`](./DATASETS.md).

---

## End-to-end toolkits

- [SpeechBrain](https://github.com/speechbrain/speechbrain) — enhancement, separation, ASR, speaker ID, all in one.
- [ESPnet](https://github.com/espnet/espnet) — research-oriented, Kaldi-style recipes.
- [Asteroid](https://github.com/asteroid-team/asteroid) — source separation / enhancement recipes.
- [NVIDIA NeMo](https://github.com/NVIDIA/NeMo) — NVIDIA's ASR / TTS / VAD / speaker toolkit.

---

## Voice agent platforms

Open-source / self-hostable:

- [Pipecat](https://github.com/pipecat-ai/pipecat) — low-code, pipeline-based voice agent framework.
- [LiveKit Agents](https://github.com/livekit/agents) — WebRTC-native agent framework.
- [n8n](https://n8n.io) — open-source workflow automation you can wire to any model.

Hosted / commercial:

- [Vapi](https://vapi.ai) — hosted voice agent platform, no-code.
- [Retell](https://retellai.com) — hosted voice agents, telephony-first.
- [Synthflow](https://synthflow.ai) — no-code voice agent builder.
- [Bland](https://bland.ai) — telephony-first voice AI.
- [Lindy](https://lindy.ai) — workflow-first AI agents.

Speech-to-Speech / Realtime models:

- [OpenAI Realtime API](https://platform.openai.com/docs/guides/realtime) — low-latency S2S.
- [Gemini Live](https://ai.google.dev/gemini-api/docs/live) — Google's multimodal S2S.

---

## Speech enhancement / cleanup

Front-end audio cleanup before STT / agent / model.

- [DeepFilterNet](https://github.com/Rikorose/DeepFilterNet) — fast, real-time noise suppression. Rust + PyTorch.
- [RNNoise](https://github.com/xiph/rnnoise) — classic recurrent denoiser, tiny, runs everywhere.
- [voicefixer](https://github.com/haoheliu/voicefixer) — restoration model for severely degraded speech.
- [Resemble Enhance](https://github.com/resemble-ai/resemble-enhance) — denoise + super-resolution.
- [Demucs (htdemucs)](https://github.com/facebookresearch/demucs) — source separation, can isolate voice.
- [ai-coustics](https://ai-coustics.com) — commercial real-time speech enhancement SDK (track partner; mention for completeness).

---

## Beyond the usual metrics

Opportunity territory — invent something here.

- [CLAP / LAION-CLAP](https://github.com/LAION-AI/CLAP) — audio–text embeddings. Build semantic "is this still understandable?" checks, or classify acoustic scenes zero-shot.
- Whisper encoder / [BEATs](https://github.com/microsoft/unilm/tree/master/beats) / [AudioMAE](https://github.com/facebookresearch/AudioMAE) — pretrained encoders as general-purpose audio embeddings for custom quality probes.
- [descript-audio-codec](https://github.com/descriptinc/descript-audio-codec) — neural codec whose reconstruction error can serve as a quality signal.
- [voicefixer](https://github.com/haoheliu/voicefixer) — reference for "how much could this be cleaned up?" — delta between input and fixed output = degradation proxy.

### Three framings to push past out-of-the-box metrics

1. **Custom dashboard** — ship with DNSMOS + NISQA + WER-under-noise + VAD-miss-rate, then add *one* novel dimension specific to your app.
2. **Chaos injector** — define what "success under chaos" means for your specific use case, then break the pipeline on purpose with the augmentation tools above.
3. **Semantic degradation** — measure meaning loss with embedding distances (CLAP, Whisper features), not just acoustic fidelity.

---

## Further reading

- [VoiceMOS Challenge 2026](https://voicemos-challenge-2026.github.io/) — current state of MOS prediction.
- [URGENT Challenge 2026](https://urgent-challenge.github.io/urgent2026/) — universal speech enhancement benchmark.
- [DNS Challenge](https://github.com/microsoft/DNS-Challenge) — canonical denoising benchmark + dataset.
- [SUPERB](https://superbbenchmark.org/leaderboard) — generalist speech representation benchmark.
