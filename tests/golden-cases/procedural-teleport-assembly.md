# Golden Case B — Teleport Device Assembly

## Case Card

```text
Case ID: GC-PROCEDURAL-001
Name: TELEPORT_DEVICE_ASSEMBLY
Primary Mode: PROCEDURAL_MONTAGE
Subtype: ASSEMBLY
Status: frozen input; image regression not executed in this ticket
```

## Frozen Input

```text
Create a 16:9 image. Project: TELEPORT DEVICE ASSEMBLY | tense sci-fi assembly | fast-paced multiple cuts | Goal: 15-second 24-panel animatic of the character assembling a broken teleport beacon. A calm scientist rapidly rebuilds a scattered teleport device before its unstable core collapses. Her goggles reveal the broken device; fast cuts track her hands snapping every part into place until the portal flickers alive. Location: cluttered sci-fi workshop floor, scattered base, loose cables, glass cylinder, antenna, clamps, battery cell, bolts and smoke residue. Character: woman scientist with platinum bob, round dark goggles, white coat, blue tie and shoulder strap; precise, urgent, controlled movement. Start: separate broken parts reflected in goggles. End: assembled beacon hums open with unstable portal pull. Final video may use retro anime sci-fi, cream/red graphic space, cyan energy, clean cel edges and subtle film grain; storyboard panels must be colorless rough light-graphite sketch only, with no shading, color fills or rendered lighting. Use 24 panels in a 4x6 grid. Put BEAT, CAMERA, ACTION, RHYTHM, ESCALATION, STATE and STYLE outside the panel artwork. Keep every panel to one extractable shot and one clear pose; no ghost poses, onion-skin bodies, arrows, labels, captions, subtitles, logos, watermarks, readable device text or technical marks inside panels.
```

## Production Target

- Format: `16:9`, 24 panels, `4×6`, one storyboard sheet, 15-second animatic planning.
- `FINAL_VISUAL_STYLE`: retro anime sci-fi glow, cream/red graphic space, cyan energy, clean cel edges, subtle film grain.
- `STORYBOARD_DRAWING_STYLE`: colorless rough graphite-gray sketch, no shading, color fills or rendered lighting.
- Annotation: `CLEAN_EXTERNAL_METADATA`; optional three-swatch `STYLE_SWATCH_STRIP` above the sheet for final style only.
- Continuity: `FULL`, `PROP_STATE_CONTINUITY`, `PROCESS_CONTINUITY`, State Before / State After.

## Hard Constraints / Human Review Prompts

Score each item 0–2 (`0` not met, `1` partial, `2` clear). Items 1–10 are the Golden Case acceptance set:

| # | Indicator | Review prompt |
|---:|---|---|
| 1 | Type recognition | Is the sequence routed as Procedural Montage / Assembly rather than martial Action? |
| 2 | State progression | Does the beacon move from broken parts to unstable portal without unexplained state rollback? |
| 3 | Cause → Effect | Can each key assembly step be read as causing the next state? |
| 4 | Single moment | Does every Panel show one extractable moment, with no ghost poses or chained time states? |
| 5 | Camera variety | Do inserts, wides, overheads, reactions and final wides serve information rather than random variety? |
| 6 | Rhythm Curve | Is there a readable setup, acceleration, interruption, peak, drop and final spike? |
| 7 | Prop consistency | Do the core, cable, lens, clamp, bolt, ring, antenna, battery, glass and lever persist after their state changes? |
| 8 | Style separation | Are final-video style references kept out of the graphite storyboard panels? |
| 9 | Annotation placement | Are production metadata and swatches outside the artwork, with no arrows, labels or readable device text inside? |
| 10 | Final consequence | Does the ending show activation, result and a new unstable pull/problem rather than merely “completed”? |

## Minimum Regression Score

Minimum `2` on indicators 1, 2, 3, 4, 7, 8, 9 and 10; no unresolved `BLOCKER`. Indicators 5 and 6 may score `1` with a documented `WARNING`. This file defines the test target only; no image generation or Golden Case scoring is performed here.
