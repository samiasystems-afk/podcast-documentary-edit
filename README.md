# podcast-documentary-edit

A Claude Code skill for editing long-form video podcasts in the AI Cross Fields house style: an opening built from real lines, one-time name cards, drawn explanations, music direction, a fast pace with punch-in zooms and branded B-roll cards, and checks on the delivered file.

Worked examples on [AI Cross Fields](https://www.youtube.com/@aicrossfields): Episode 15 with McKay Salisbury (the current fast-pace pipeline) and [the Lena Komarova episode](https://youtu.be/81fguxFhLPA).

## Install

```bash
git clone https://github.com/samiasystems-afk/podcast-documentary-edit ~/.claude/skills/podcast-documentary-edit
```

Restart Claude Code.

## Use

Have ready: the separate Riverside tracks (one camera per person), the guest's exact name and role, the episode number, and the footage location. Open Claude Code in your editing folder and say:

> Use the podcast-documentary-edit skill to edit this episode.

## What's inside

- `SKILL.md`: the editing decisions and delivery checks.
- `references/episode-runbook.md`: start here for a new episode. What to ask for, the 10-step pipeline, Mac setup, and bugs already fixed.
- `references/`: brand, editing direction, drawn explanations, interview and packaging, the McKay (Ep. 15) and Lena worked examples, local ffmpeg finishing notes, and the fast-pace rules (`fast-pace-seven-habits.md`).
- `assets/`: the AI Cross Fields channel logo and banner (for this show only), and the Anton font with its SIL Open Font License.

The AI Cross Fields logo and banner belong to the show. Don't use them for other channels.
