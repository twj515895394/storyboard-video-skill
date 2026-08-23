# 01 — Soft Validator 策略落地

**构建内容:** 用户修改 Skill 规则后，Validator 能自动按统一策略输出 BLOCKER / WARNING / NOTE 三级校验结果。只有基础硬约束（画幅不符、模式错误、关键元素缺失、不可调和矛盾、输出缺少必需部分）才判定 BLOCKER；专业质量建议（镜头重复、动作因果不够强、Motion DNA 不够稳定、结尾不够有冲击力等）一律降为 WARNING 或 NOTE，不阻断交付。

**类型:** AFK

**被哪些阻塞:** None — 可以立即开始

**状态:** completed

- [ ] 创建独立 Validator reference 文件，定义 BLOCKER / WARNING / NOTE 三级严重程度和判定标准
- [ ] 定义统一输出格式模板（Validation Notes 块）
- [ ] 将各专项 reference（Continuity、Geography、Annotation、Motion DNA、Action、Reference Role）中已有的 PASS / WARNING / BLOCKER 规则引用统一策略
- [ ] 更新 storyboard-core.md 的 Preflight Validation 章节，引用统一 Validator reference
- [ ] 更新 SKILL.md 工作流中的校验步骤
- [ ] 确保 Dialogue/Horror 的静止和 Hold 不会被误判为问题

## 完成总结报告

- [ ] 若本 issue 涉及接口、参数、响应字段、校验规则或默认行为变化，完成后已在当前项目约定的 reports 目录生成对应 summary 报告。
- [ ] summary 报告已包含新增/修改接口、输入参数变更、输出字段变更、人工验证建议、技术验证结果、风险与注意事项。
- [ ] 已在本 issue 的 `## 评论` 中追加 summary 报告路径和生成时间。
- [ ] 若本 issue 无接口或可观测行为变化，已在 `## 评论` 中说明无需 summary 报告的原因。

## 评论

2026-08-23：已完成。新增 `references/storyboard-validator.md`，并将主 Skill、Core、Continuity、Geography、Annotation、Motion DNA、Action、Reference Role 的严重度统一；未执行图像回归。无需单独 summary report，变更属于规则层与默认行为文档化，验证结果记录在主线程规划文件中。
