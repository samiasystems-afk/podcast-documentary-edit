# Worked example: Episode 15, McKay Salisbury (October 2026)

The first episode built at the fast pace in [fast-pace-seven-habits.md](fast-pace-seven-habits.md), through the pipeline in [episode-runbook.md](episode-runbook.md). Yousif approved the teaser. Publication and anything sent to the guest still need his approval.

## Delivered

All in `/Volumes/Extreme SSD/Cold emails/mckay_episode_edit/`:

- `renders/mckay_full_episode_v2.mp4`: full episode, 27:35 (teaser → logo sting → interview → end card).
- `renders/mckay_teaser_v2.mp4`: teaser alone, 1:10. Approved by Yousif.
- `renders/reels/mckay_reel_sheep.mp4` and `mckay_reel_time-back.mp4`: two vertical reels with word-by-word captions.
- `renders/thumbnails/thumb_option1–3.png`: option 1, "LEAVING AI?", recommended.
- `notes/PUBLISHING_KIT.txt`: titles, descriptions, chapters, tags, hashtags, music credit.

## What happened and what carries over

- The separate Riverside tracks were in sync with the combined export (0.0 s). Check every episode anyway.
- McKay's camera froze for 6 s. Covered with Yussef's same-moment camera, found by `freezedetect` on the source before editing.
- Whisper's plain word offsets ran up to 0.6 s late. DTW word timing (`-dtw base.en -nfa -ojf`) fixed card and caption anchoring.
- Two render bugs (face tile offset, chunk join order) and three machine problems (SSD disconnects, iCloud folders, 5 GB free disk) cost the most time. All are written up in the runbook.
- Title format chosen: **"<hook> | <Guest Name> | Ep. 15"**.

## Yousif's preferences so far

- Fast pace (Nick Saraev style), branded to AI Cross Fields.
- Teaser built only from real lines, in an order that never changes their meaning.
- Titles end with "| Guest Name | Ep. N".
- Nothing is published or sent to the guest without his approval.
