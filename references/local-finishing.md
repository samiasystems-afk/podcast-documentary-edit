# Demonstrated Mac finishing lessons

Read only for local FFmpeg implementation. Discover current binaries/capabilities first. From Episode 15 the build is Remotion plus ffmpeg on Yousif's Mac; its setup and gotchas are in [episode-runbook.md](episode-runbook.md). The notes below are from Lena.

- Check which ffmpeg build you have: some fast Homebrew builds lack drawtext. Use a build with drawtext for typography segments. `h264_videotoolbox` needed actual Mac hardware access; respect current execution permissions.
- Encoded section changes caused filter reinitialisation and reset zoompan clocks, dropping frames. Demonstrated correction: input `-reinit_filter 0`, video prefix `setpts=N/(24*TB)` before zoompan, output `-fps_mode cfr`. **24fps is source-specific.** Validate a boundary test and full final frame count. Do not blindly suppress reinitialisation if geometry genuinely changes.
- Continuous audio rebuilt from mapped source pieces avoided mono/stereo gain differences. Lena's mono speech used unity stereo centre, tiny3ms anti-click fades and limiter latency compensation. Inspect boundary words, not just arithmetic.
- Two-pass loudness normalization delivered−16.01LUFS stereo/−3.76dBTP. −16LUFS and <=−1.5dBTP are useful spoken-word starting targets, not universal dynamics requirements.
- Transcription from Episode 15: `whisper-cli` base.en with `-dtw base.en -nfa -ojf`. Plain word offsets ran up to 0.6 s late.
- Local Faster Whisper small.en used16kHz mono PCM through Python wave/numpy after compressed-media decoding hit an av compatibility issue. No footage upload or metered transcription needed.
- Waveform correlation between aligned premix and decoded AAC establishes encode timing despite gain changes. It does not prove original lip sync or whole-mix subjective quality.
- Sparse white titles on charcoal can trigger blackdetect; deliberate graphic holds trigger freezedetect. Inspect every flag before repair. Do not weaken thresholds to hide failures.

Lena's episode-specific scripts live in Yousif's workspace. These contain hardcoded filenames, episode times and assets; adapt rather than executing against new footage unchanged.

- Lena v5 recovery: a genuine owner-accessible Riverside signed export link downloaded locally after the browser reported a client-side block. Preserve the same access scope and expiry; never weaken browser protections or bypass authentication.
- Keep headroom in intermediate audio: floating-point PCM or safe pre-gain before filters avoids clipping a near-full-scale source before final normalization. V5 used0.5 pre-gain before65Hz high-pass and float premix.
- Validate audio timing against a reference with the same intended filtering. Untreated low frequencies can depress waveform correlation without indicating a timing error. Retain raw diagnostics; do not weaken passing thresholds.
- A concat remux can add SPS/PPS headers to H264 keyframes without changing decoded pixels. Use exact decoded-frame hashes to test approved picture preservation; compare the final coded stream against the reviewed assembled picture separately.
