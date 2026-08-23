# 06 — Continuity 扩展: PROP_STATE + PROCESS

**构建内容:** Procedural Montage 中关键道具的状态能被连续追踪（例如"电池已安装"后不会在后续格中消失），且步骤之间的过程依赖关系得到校验（例如"核心未安装"时不允许出现"密封管道"）。

**类型:** AFK

**被哪些阻塞:** None — 可以立即开始

**状态:** completed

- [ ] 在 continuity-system.md 新增 PROP_STATE_CONTINUITY Domain：追踪道具"当前处于什么阶段"，保证状态单向推进（不可逆状态不能回退）
- [ ] 新增 PROCESS_CONTINUITY Domain：追踪步骤间依赖关系（A 未完成时不允许出现依赖 A 的 C）
- [ ] 新增 State Before / State After 字段定义：每个关键 Panel 内部记录操作前后状态，由 Director Plan 阶段自动推导
- [ ] 定义 PROP_STATE 启用条件：PROCEDURAL_MONTAGE 强制启用，其他模式有道具状态需求时可选启用
- [ ] Validator 中将"道具状态无原因回退"标记为 WARNING，将"关键道具凭空消失"标记为 BLOCKER
- [ ] 更新 storyboard-core.md 和 SKILL.md

## 完成总结报告

- [ ] 若本 issue 涉及接口、参数、响应字段、校验规则或默认行为变化，完成后已在当前项目约定的 reports 目录生成对应 summary 报告。
- [ ] summary 报告已包含新增/修改接口、输入参数变更、输出字段变更、人工验证建议、技术验证结果、风险与注意事项。
- [ ] 已在本 issue 的 `## 评论` 中追加 summary 报告路径和生成时间。
- [ ] 若本 issue 无接口或可观测行为变化，已在 `## 评论` 中说明无需 summary 报告的原因。

## 评论

2026-08-23：已完成。Continuity System 新增 `PROP_STATE_CONTINUITY`、`PROCESS_CONTINUITY`、State Before/After，并将 Procedural Montage 设为强制 FULL；未执行图像回归。无需单独 summary report。
