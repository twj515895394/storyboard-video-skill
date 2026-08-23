# 04 — CLEAN_EXTERNAL_METADATA Annotation Profile

**构建内容:** 用户可以选择一种 Annotation Profile，使 Panel 画面内完全干净（无箭头、标签、字幕、摄影机注释、技术标记、可读设备文字），所有生产信息（Panel 编号、Header Chip、BEAT、CAMERA、ACTION、RHYTHM、ESCALATION、STATE、STYLE）放在 Panel 画面外部。

**类型:** AFK

**被哪些阻塞:** None — 可以立即开始

**状态:** completed

- [ ] 在 annotation-system.md 新增 CLEAN_EXTERNAL_METADATA Profile，定义 Panel 内禁止项和 Panel 外允许项
- [ ] 明确该 Profile 与现有 CLEAN 的区别：CLEAN 是"极简注释"，CLEAN_EXTERNAL_METADATA 是"画面内零注释 + 画面外完整 metadata"
- [ ] 定义默认启用场景：用户明确要求时；或 Procedural Montage + 用户要求 Clean 时推荐
- [ ] 更新 SKILL.md Annotation 选择逻辑
- [ ] Validator 中将"CLEAN_EXTERNAL_METADATA 模式下 Panel 内出现箭头或标签"标记为 WARNING

## 完成总结报告

- [ ] 若本 issue 涉及接口、参数、响应字段、校验规则或默认行为变化，完成后已在当前项目约定的 reports 目录生成对应 summary 报告。
- [ ] summary 报告已包含新增/修改接口、输入参数变更、输出字段变更、人工验证建议、技术验证结果、风险与注意事项。
- [ ] 已在本 issue 的 `## 评论` 中追加 summary 报告路径和生成时间。
- [ ] 若本 issue 无接口或可观测行为变化，已在 `## 评论` 中说明无需 summary 报告的原因。

## 评论

2026-08-23：已完成。Annotation System 新增 `CLEAN_EXTERNAL_METADATA`，定义 Panel 内禁止项、Panel 外 metadata 和与 `ANNOTATION_CLEAN` 的边界；未执行图像回归。无需单独 summary report。
