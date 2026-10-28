# Episode runbook: from raw Riverside tracks to a publishing kit

This is the pipeline that delivered Episode 15 (McKay Salisbury, October 2026). Reuse the scripts in the previous episode's `work/` folder and change the times and words; do not write a new pipeline per episode. Worked example: [mckay-handoff.md](mckay-handoff.md).

## Ask at the start

1. The **separate Riverside tracks**: one camera per person, the "raw-video-cfr" files. The combined split-screen export alone is not enough.
2. The guest's exact name and role. Verify both online before they go on a name card or title.
3. The episode number.
4. Where the footage is. Ep. 15 was in `/Volumes/Extreme SSD/Cold emails/`.

## Project layout

One folder per episode on the SSD, e.g. `/Volumes/Extreme SSD/Cold emails/<guest>_episode_edit/`:

- `work/`: scripts (`edl.py`, `edl_full.py`, `edl_reels.py`, `build_audio_full.py`, `build_audio_reels.py`, `mix.sh`, `render_full.sh`, `qa_stalls.py`).
- `renders/`: full episode, teaser, `reels/`, `thumbnails/`. Version every output (`_v2`, `_v3`).
- `notes/`: paper edit, cue sheet, source map, QA, `PUBLISHING_KIT.txt`.
- `tmp/`: `TMPDIR` for renders.

The Remotion engine stays on the internal disk at `~/Movies/aicf-engine/remotion` (compositions: Teaser, Full, Reel, Thumbnail). See the machine notes below for why.

## Pipeline

1. **Transcribe locally.** `whisper-cli` with `~/Documents/Video editing/work/models/ggml-base.en.bin` and **`-dtw base.en -nfa -ojf`** for word times. Plain word offsets were up to 0.6 s late, which breaks word-anchored cards and captions.
2. **Check sync** of the separate tracks against the combined export by audio cross-correlation. Ep. 15 was 0.0 s; do not assume the next one is.
3. **Scan the sources** for black, frozen or silent gaps (`blackdetect`, `freezedetect`, `silencedetect`). Ep. 15 had the guest's camera frozen for 6 s; it was covered with the host's same-moment camera. Never cover a gap with footage from a different moment.
4. **Speaker detection** from the two isolated mics: RMS per 0.1 s. Ep. 15 had about 40 dB separation, so the louder mic is the speaker.
5. **Teaser** (`edl.py` + `Teaser.tsx`). About 65 s of the guest's real lines in story order: tension → context → insight → limit → turn → unresolved question. Guest full-frame, hero words in clean wall space, genuine same-moment host reaction cutaways, logo sting. Re-transcribe the assembled teaser audio and check every join reads as the guest said it.
6. **Full episode** (`edl_full.py` + `Full.tsx`). Teaser → logo sting → interview → end card. Pauses over 0.45 s cut to 0.22 s; camera follows the speaker; punch-ins alternate 1.00/1.12; chapter stings; word-anchored cards (list, flow, compare, statement, chat) with a live face tile; term chips; one name card each; subscribe animation; end card. A card arrives 0.3 s before its first spoken item, never on a title alone. Full rules: [fast-pace-seven-habits.md](fast-pace-seven-habits.md).
7. **Audio** (`build_audio_full.py` + `mix.sh`). Level-match the two mics. Episode −16 LUFS stereo, reels −14 LUFS, true peak ≤ −1.5 dBTP. Music: "Signal to Noise" by Scott Buckley (CC-BY). The credit goes in every description.
8. **Reels** (`edl_reels.py` + `Reel.tsx` + `build_audio_reels.py`). 1080×1920, one continuous moment each (no stitched lines), word-by-word amber captions. Check any unclear word on the isolated mic before it becomes a caption.
9. **Thumbnails and publishing kit.** Three thumbnail options with a recommendation. `PUBLISHING_KIT.txt`: titles, descriptions, chapters (matching the stings), tags, hashtags, music credit. Title format: **"<hook> | <Guest Name> | Ep. N"**, plain text, always with the episode number.
10. **QA on the delivered files.** Frame count, duration, black/freeze, loudness, audio sync start/middle/end, `qa_stalls.py` (frozen output video where the source has motion), and frames at every render-chunk join. Then open frames and watch; a passing script is not a viewing.

Nothing is published or sent to the guest without Yousif's approval.

## Machine notes (Yousif's Mac)

- **Documents and Desktop sync to iCloud.** Node and Remotion hang there. Keep the Remotion engine at `~/Movies/aicf-engine/remotion`.
- **The SSD is ExFAT**: no symlinks, so no `node_modules` on it. Project files and renders go on the SSD; code runs from the internal disk.
- **The SSD disconnected 4 times during Ep. 15.** Before a long render: plug it straight into the Mac (no hub), check the cable, run Disk Utility First Aid. Keep a copy of `work/` somewhere other than the SSD.
- **The internal disk has about 5 GB free.** Set `TMPDIR=<project>/tmp/` on the SSD before rendering.
- Remotion reads video through a local server: `python3 -m http.server 8765 --bind 127.0.0.1 --directory "/Volumes/Extreme SSD/Cold emails"`.
- **Make short-GOP intermediates first**: `ffmpeg -c:v libx264 -crf 14 -g 24 -keyint_min 24 -bf 0`. Riverside files have a keyframe only every 5 s, which makes Remotion seeking slow and unreliable.
- Render in chunks with `work/render_full.sh` under `caffeinate` (the Mac slept once mid-render). The full render takes about 60–70 min.
- This ffmpeg has no `drawtext`. All graphics are Remotion.

## Bugs already fixed (do not reintroduce)

- The face tile's `<Sequence from>` must be `seg.start - cardStart`, not `- absFrame`. The wrong offset shows the face from the wrong moment.
- Join render chunks **in numeric frame order**. A text sort put chunk 8000 last.
