# 07 — Procedural Montage 导演规则

**构建内容:** 用户输入装配、制作、变装、调查、训练、烹饪等以"状态推进"为核心逻辑的场景时，Skill 自动识别为 Procedural Montage，按专用导演规则生成故事板：每格推进新状态、展示 Before/During/After、包含 Interruption Beat、结尾产生 Consequence。该模式也可以作为嵌入 Pattern 用于其他主模式中的局部片段。

**类型:** AFK

**被哪些阻塞:** 01, 02, 03, 04, 05, 06

**状态:** completed

- [ ] 新增 references/procedural-montage.md，包含：
  - 定义：State-transition-driven Storyboard，与 Action Choreography 的本质区别
  - 11 个 Subtype（ASSEMBLY、REPAIR、CRAFT、COOKING、PREPARATION、TRANSFORMATION、ACTIVATION、EXPERIMENT、CONSTRUCTION、MAINTENANCE、WORKFLOW）
  - 适用性判断器（6 问）
  - 典型叙事结构模板（HOOK → PARTS REVEAL → COMMIT → EARLY PROGRESS → ACCELERATION → INTERRUPTION → RECOVERY → FINAL STEP → ACTIVATION → CONSEQUENCE）
  - 导演规则：Show Progress Not Repetition、Before/During/After Readability、Tool/Part Recognizability、Macro Inserts、Wide Reset 时机、Interruptions Create Drama、Final State Must Be Cinematically Meaningful
  - 嵌入 Pattern 规则：当 Primary Mode 为其他类型时，Procedural Montage 可作为 Secondary Pattern
  - 不适用边界：纯情感停顿、纯空间探索、对话权力变化、连续动作表演
- [ ] Mode Router 新增 PROCEDURAL_MONTAGE 作为一级 Sequence Logic
- [ ] 定义 Procedural Montage 的默认配置：Continuity 使用 FULL（PROP_STATE 强制）、Annotation 推荐 CLEAN_EXTERNAL_METADATA 或 SIMPLE、Rhythm Curve 必须规划
- [ ] 更新 storyboard-core.md 的 Mode Router 和 Cinematic Function（新增 PROCESS_PROGRESS、STATE_CHANGE）
- [ ] 更新 SKILL.md 工作流中的模式识别和 reference 加载步骤
- [ ] 确保 Procedural Montage 规则不污染 Action、Dialogue、Horror 模式

## 完成总结报告

- [ ] 若本 issue 涉及接口、参数、响应字段、校验规则或默认行为变化，完成后已在当前项目约定的 reports 目录生成对应 summary 报告。
- [ ] summary 报告已包含新增/修改接口、输入参数变更、输出字段变更、人工验证建议、技术验证结果、风险与注意事项。
- [ ] 已在本 issue 的 `## 评论` 中追加 summary 报告路径和生成时间。
- [ ] 若本 issue 无接口或可观测行为变化，已在 `## 评论` 中说明无需 summary 报告的原因。

## 评论

2026-08-23：已完成。新增 `references/procedural-montage.md`，Core/SKILL 已接入一级 Mode、11 个 Subtype、六问判断器、过程结构和 Secondary Pattern；未执行图像回归。无需单独 summary report。
