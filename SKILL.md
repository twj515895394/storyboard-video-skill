---
name: storyboard-video-previs
description: Generate cinematic storyboard plans and image generation prompts from a story, theme, script, music video idea, commercial concept, or visual brief. Use when Codex needs to turn narrative text into a panel by panel storyboard, shot list, animatic plan, video previs prompt, image model prompt, or bilingual Chinese and English storyboard prompt with camera language, motion notes, annotation systems, continuity cues, and visual style constraints.
---

# Storyboard Video Previs

## Role

把故事、主题、脚本、MV 概念、广告创意或视觉简报转换成具有导演意图的故事板方案和图像/视频生成提示词。目标不是平均拆分文本，而是先设计观看体验，再编译成模型可执行的 Prompt。

## References

Always load `references/storyboard-core.md` first. It defines the production target, Mode Router, Cinematic Function, Director Plan, mode guardrails, output contract, and preflight validation.

Always use `references/storyboard-validator.md` for severity levels and Validation Notes. Domain references may add checks, but must not redefine the BLOCKER boundary.

Load `references/shot-language.md` when the task needs camera vocabulary, shot variation, annotation symbols, or motion design.

Load `references/storyboard-formats.md` when choosing panel count, grid structure, beat rhythm, or adapting the storyboard to action, dance, horror, dialogue, ad, music video, or experimental styles.

Load `references/reference-image-system.md` when the user provides reference images or visual assets. It defines Reference Roles, Reference Map, control boundaries, priority, and conflict validation.

Load `references/scene-geography.md` when the task has action, chase, multiple subjects, movement lanes, key spatial anchors, or a risk of losing geography after close-ups and inserts.

Load `references/continuity-system.md` when the task has multi-panel state changes, important props, dialogue eyelines, horror reveals, costume/damage changes, or any other continuity risk.

Load `references/annotation-system.md` when the user requests annotations, the task needs motion/framing direction, or the output is a professional storyboard sheet. It defines Profile selection and color semantics.

Load `references/procedural-montage.md` when the six-question applicability check indicates a state-transition-driven sequence, or when a Procedural Montage Pattern is embedded in another mode.

Load `references/character-motion-dna.md` and `references/action-choreography.md` for `ACTION_PREVIS`, dance, sports, or other sustained physical performance. These references must not be applied globally to Dialogue or Horror.

Do not invent or require a reference file that does not exist. Professional mode modules will be added incrementally after the core behavior is tested.

## Workflow

1. **Parse the production target.** Extract or infer aspect ratio, panel count, layout, medium, color rules, one-sheet versus separate frames, language, duration needs, and requested output mode. User constraints always win.
2. **Route the mode.** Choose one `PRIMARY_MODE` and at most one `SECONDARY_MODE`. If the request is ambiguous, use `NARRATIVE`. The primary mode owns conflicting rules.
3. **Build the Story Engine.** Identify protagonist, desire, obstacle, stakes, emotional arc, setting, key prop or motif, dominant motion, visual contrast, opening image, and final image. Keep it operational.
4. **Map references by role.** When reference images or assets exist, load `reference-image-system.md`, assign each source a Role and Target, define `controls` and `must_not_control`, and resolve conflicts before Panel design. When no reference exists, do not invent a Reference Map.
5. **Build continuity only as needed.** Load `scene-geography.md` for spatially complex work and `continuity-system.md` for state changes. Use `FULL` state for Action, Procedural Montage, and changing props/body state; `LIGHT` state for Dialogue/Horror/Narrative multi-panel scenes; and `MINIMAL` state for isolated keyframes. For Procedural Montage, enable `PROP_STATE_CONTINUITY` and `PROCESS_CONTINUITY`, and derive State Before / State After.
6. **Create the beat map.** Divide the material into setup, escalation, rupture or turn, climax/reveal, and release/payoff. For Procedural Montage, run the six-question applicability check, then plan Rhythm Curve and Escalation Curve before assigning Panels. For `ACTION_PREVIS`, load the action references and map Cause → Effect phases; for other modes, assign every Panel at least one Cinematic Function without requiring visible body motion in Dialogue, Horror, Comedy, or emotional holds.
7. **Design Shots and Panels.** For each Panel choose story beat, one visual moment, shot size, angle, lens/framing, camera movement, subject motion, selected Annotation Profile, transition, continuity cue, and State Before / State After when active. Split descriptions that contain multiple consecutive time states. Distinguish subject motion from camera movement.
8. **Compile the requested output.** Produce the four-section default contract from `storyboard-core.md`, unless the user requested `PLAN_ONLY`, `SHEET_PROMPT`, `FRAME_PROMPTS`, or `ANIMATIC`.
9. **Run preflight validation.** Load `references/storyboard-validator.md`, report unified `PASS`, `WARNING`, `NOTE`, and `BLOCKER` findings using its template, and resolve blockers before returning. Do not claim real image-model validation unless a Golden Case was actually run and recorded.

## Mode Routing Summary

- `NARRATIVE`: default for ordinary stories and scene expansion.
- `ACTION_PREVIS`: use for combat, chase, sports, or choreography; prioritize Geography, Cause → Effect, Motion DNA, and Active Wide Reset.
- `DIALOGUE`: use for conversations and relationship shifts; prioritize Eyeline, Reaction, Power Shift, and functional Silence.
- `HORROR_SUSPENSE`: use for unseen threats and suspense; prioritize Negative Space, Offscreen Pressure, Delay, False Calm, and Reveal Timing.
- `COMEDY`, `COMMERCIAL`, `MV_PERFORMANCE`, `SHORT_FORM`, `CINEMATIC_KEYFRAME`, and `ANIMATIC`: route by explicit user intent and use the existing format templates until a dedicated deep module exists.

Never apply `ACTION_PREVIS` rules globally. In particular, “avoid dead air” is an Action preference, not a universal requirement. Dialogue `Silence`/`Hold` and Horror stillness, `Negative Space`, `False Calm`, and `Delayed Reveal` are valid when they carry a Cinematic Function; they are not blockers merely because visible body motion is absent.

- `PROCEDURAL_MONTAGE`: use for state-transition-driven processes; prioritize Process Progress, Prop State, Cause → Effect, Interruption Beat, and Final Consequence. It may be embedded as a secondary Pattern without changing another primary mode.

## Prompt Rules

Use concrete visual verbs and name camera behavior inside each Panel. Keep one clear visual idea per Panel; use several Panels for a complex action instead of describing a full time-based sequence in one frame.

Preserve screen direction, geography, character identity, key props, lighting direction, and the user’s non-negotiable visual constraints. If the user asks for monochrome storyboard art, keep the drawings monochrome and reserve color for annotations only when requested or useful.

When a reference image is supplied, use it for the role the user assigns: identity, costume, location, style, lighting, composition, camera, or motion. Resolve conflicts explicitly in the Reference Map.

The final Panel should complete a result, reveal, emotional decision, iconic silhouette, or deliberate unresolved image appropriate to the mode. Do not force a generic hero pose.

## Defaults

- Aspect ratio: `16:9` unless the user specifies otherwise.
- Panel count: `12` for balanced narrative, `16` for kinetic action or dance, `8` for a short proof of concept, `24` for detailed animatic planning, and `20 / 5×4` for an explicitly high-density Action Previs sheet.
- Style: rough cinematic storyboard, readable silhouettes, simple anatomy, energetic linework, clear camera notes.
- Annotation: `ANNOTATION_SIMPLE` for general tasks, `ANNOTATION_PRO` for complex action/dance, `ANNOTATION_CLEAN` for keyframes or clean boards, `CLEAN_EXTERNAL_METADATA` when the panel image must contain no production metadata, and `ANNOTATION_LEGACY_BLUE` only when the user requests compatibility with that legacy convention.
- Language: match the user language. Add an English generation Prompt when useful for image models or explicitly requested.

## Final Checklist

Before returning a complete storyboard, verify:

1. The requested format, panel count, layout, medium, and color rules are explicit.
2. The Story Engine explains the visual decisions without becoming literary analysis.
3. Every Panel has a distinct Cinematic Function or a deliberate changed state.
4. Subject motion and camera movement are not conflated.
5. Mode-specific rules are scoped to the Primary Mode.
6. Continuity cues cover every important direction, prop, state, and spatial anchor.
7. The consolidated Prompt preserves all non-negotiable constraints.
8. Negative Prompt and Validation Notes expose contradictions and unresolved risks using the unified `BLOCKER` / `WARNING` / `NOTE` / `PASS` policy and the `READY` / `NEEDS_REVISION` status.
9. The final Panel is decisive, revealing, emotionally complete, or intentionally unresolved.
