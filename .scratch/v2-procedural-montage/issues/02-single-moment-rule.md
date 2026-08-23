# 02 — Single-Moment Rule

**构建内容:** 任何模式下，每个 Panel 只描述一个可提取的视觉瞬间。用户输入的故事板不再出现同一人物的多个时间状态（ghost poses、onion-skin、一格内描述"她抓起电池、转身、插入、拉杆"等连续动作）。这同时解决 V1 Dialogue P07 的子画面拆分问题。

**类型:** AFK

**被哪些阻塞:** None — 可以立即开始

**状态:** completed

- [ ] 在 storyboard-core.md 新增 Single-Moment Rule 章节，定义标准表述和禁止列表
- [ ] 明确该规则适用于所有模式（Action、Dialogue、Horror、Procedural Montage 等）
- [ ] 在 Director Plan 阶段加入单瞬间检查：每格描述中如果包含多个连续动作，提示拆分
- [ ] 更新 SKILL.md 工作流中的 Panel 设计步骤
- [ ] Validator 中将"单格多时间状态"标记为 WARNING（不是 BLOCKER，因为偶尔的双动作可读时可接受）

## 完成总结报告

- [ ] 若本 issue 涉及接口、参数、响应字段、校验规则或默认行为变化，完成后已在当前项目约定的 reports 目录生成对应 summary 报告。
- [ ] summary 报告已包含新增/修改接口、输入参数变更、输出字段变更、人工验证建议、技术验证结果、风险与注意事项。
- [ ] 已在本 issue 的 `## 评论` 中追加 summary 报告路径和生成时间。
- [ ] 若本 issue 无接口或可观测行为变化，已在 `## 评论` 中说明无需 summary 报告的原因。

## 评论

2026-08-23：已完成。Single-Moment Rule 已写入 Core、SKILL 工作流与统一 Validator，适用于所有模式；多时间状态标记为 WARNING，不执行图像回归。无需单独 summary report。
