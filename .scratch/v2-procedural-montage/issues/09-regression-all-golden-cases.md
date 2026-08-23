# 09 — 四 Golden Case 统一回归校验

**构建内容:** V2 所有规则落地后，从 Action（Whoking）、Dialogue（厨房录音笔）、Horror（无人走廊）、Procedural Montage（Teleport Assembly）Golden Case 库中选择 2–3 个代表性案例生成图像，按统一评分表评分，确认新增能力不污染既有模式且核心指标不退化。输出回归报告。

**类型:** HITL

**被哪些阻塞:** 01, 02, 03, 04, 05, 06, 07, 08

**状态:** ready-for-agent

- [ ] 选择 2–3 个冻结 Golden Case 输入生成图像并评分，至少包含 Procedural Montage 和一个既有模式边界案例
- [ ] 核心指标（Story Clarity、Continuity、Mode Fidelity、Final Panel Strength）不低于 2 分
- [ ] 确认新增 Validator / Single-Moment / Style / Continuity 规则未污染所选既有模式
- [ ] 确认 Procedural Montage 规则未污染所选 Action、Dialogue 或 Horror 边界案例
- [ ] 如发现退化，记录具体问题并决定是否修复
- [ ] 输出回归报告到 tests/golden-cases/runs/ 对应目录
- [ ] 更新设计文档的阶段状态

## 完成总结报告

- [ ] 若本 issue 涉及接口、参数、响应字段、校验规则或默认行为变化，完成后已在当前项目约定的 reports 目录生成对应 summary 报告。
- [ ] summary 报告已包含新增/修改接口、输入参数变更、输出字段变更、人工验证建议、技术验证结果、风险与注意事项。
- [ ] 已在本 issue 的 `## 评论` 中追加 summary 报告路径和生成时间。
- [ ] 若本 issue 无接口或可观测行为变化，已在 `## 评论` 中说明无需 summary 报告的原因。

## 评论
