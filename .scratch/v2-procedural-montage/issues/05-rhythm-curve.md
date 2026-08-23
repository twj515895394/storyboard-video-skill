# 05 — Rhythm Curve + Escalation Curve

**构建内容:** Skill 在 Director Plan 阶段先设计整体节奏曲线和情绪强度曲线，再把具体 Panel 分配进曲线的各个阶段，而不是每格独立标注"快/慢"。用户获得的故事板具有设计过的整体节奏波形。

**类型:** AFK

**被哪些阻塞:** None — 可以立即开始

**状态:** completed

- [ ] 在 storyboard-core.md 新增 Rhythm Curve 概念，定义典型波形阶段（SETUP → ACCELERATION → INTERRUPTION → RE-ACCELERATION → PEAK → DROP → FINAL_SPIKE）
- [ ] 区分 RHYTHM（剪辑和动作速度）与 ESCALATION（剧情压力/情绪强度），二者可以不同步（例如 pause + drop 为再冲高留空间）
- [ ] Director Plan 步骤在 Panel 分配前先规划整体曲线
- [ ] Validator 不要求"每格越来越快"，而是检查"是否存在有意图的整体曲线"（WARNING 级别）
- [ ] 更新 SKILL.md 工作流

## 完成总结报告

- [ ] 若本 issue 涉及接口、参数、响应字段、校验规则或默认行为变化，完成后已在当前项目约定的 reports 目录生成对应 summary 报告。
- [ ] summary 报告已包含新增/修改接口、输入参数变更、输出字段变更、人工验证建议、技术验证结果、风险与注意事项。
- [ ] 已在本 issue 的 `## 评论` 中追加 summary 报告路径和生成时间。
- [ ] 若本 issue 无接口或可观测行为变化，已在 `## 评论` 中说明无需 summary 报告的原因。

## 评论

2026-08-23：已完成。Core 已加入 `RHYTHM_CURVE`、`ESCALATION_CURVE` 与七阶段波形，要求先规划曲线再分配 Panel；Validator 只做 WARNING 级整体意图检查。未执行图像回归。无需单独 summary report。
