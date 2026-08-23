# Storyboard Validator

这是 Universal Storyboard Director 的统一校验口径。各领域 reference 可以定义要检查的事实，但严重级别、输出格式和阻断边界以本文件为准。

## 1. Validator Scope

Validator 是返回前的推理检查，不是图像生成器、执行器或 JSON Schema。它检查当前 Director Plan 和四段输出契约是否可解释；没有运行 Golden Case 时，不得声称已经完成模型实测。

校验顺序：

1. 先检查用户明确要求和基本输出契约。
2. 再检查模式边界、关键角色/道具/场景和不可调和矛盾。
3. 最后检查 Continuity、Geography、Reference、Annotation、Motion DNA、Action 等专业质量。
4. 先修复所有 `BLOCKER`，再交付；`WARNING` 和 `NOTE` 记录为可见风险或建议，不因审美偏好阻断交付。

## 2. Severity Levels

### `BLOCKER`

仅用于以下明确硬问题：

- 用户明确要求的画幅、Panel 数、布局、媒介或色彩规则完全不符。
- `PRIMARY_MODE` 明显错误，或模式规则与用户硬约束直接冲突。
- 用户指定的关键角色、道具、场景或必需事实缺失。
- 发生不可解释且影响后续理解的连续性矛盾，例如人物瞬移、核心道具消失、身体/伤势无因果回滚、空间出口互相冲突。
- Prompt 或输出内部存在无法同时成立的矛盾，例如同时要求黑白画面与全彩成片，且没有把颜色限定为注释层。
- 必需输出缺失，例如没有 Panel Table、整合 Prompt（用户未选择 `PLAN_ONLY` 时）或 Validation Notes。
- 用户明确禁止的内容被直接加入，且不是可通过说明或局部调整消解的冲突。

以下情况不能单独升级为 `BLOCKER`：镜头类型重复、动作因果不够强、Wide Reset 不够理想、Motion DNA 过于概括、结尾不够有冲击力、Panel 子画面拆分、没有覆盖所有动作阶段，或某格没有明显身体动作。

### `WARNING`

用于不会使交付失效、但值得返工或在下一轮修正的专业风险：

- 某字段或空间锚点缺失，但当前没有直接矛盾。
- 相邻 Panel 功能相近、镜头变化过多、动作阶段重复或升级变量不足。
- 单格包含多个连续时间状态，或 Procedural Montage 的状态推进/过程依赖不够清楚。
- Rhythm / Escalation Curve 缺少整体意图，或 Interruption Beat 没有改变后续决策。
- 连续 Close-up/Insert 使 Geography 变弱，但仍可用 Wide Reset 修复。
- Motion DNA 只剩空泛词、Signature Move 重复、恢复逻辑偏弱，或角色动作区分度不足但未违反明确禁用项。
- Reference 的 ID、职责、Target 或优先级不完整；同职责冲突尚可通过用户说明或补充优先级解决。
- Annotation Profile 与模式/用户意图不理想，或技术文字过长，但没有发生语义互相覆盖。
- 用户要求高度身份一致却未提供可用 Identity Reference。
- Dialogue 的 Eyeline、Reaction、Power Shift 或道具状态需要补充；Horror 的威胁来源或 Reveal Timing 需要补充。

### `NOTE`

用于可选的专业建议、取舍说明和有意保留的风险，不表示失败：

- 建议增加镜头变化、动作阶段、Wide Reset、Continuity Cue 或更具体的动作词。
- 记录某个模式选择了静止、Hold、Silence、Negative Space、False Calm 或 Delayed Reveal。
- 说明没有参考图、使用推断 ID、采用默认 Profile 或没有执行 Golden Case。
- 记录不影响硬约束的局部可读性优化，例如单格内容偏密、结尾可以更有余波。

### `PASS`

用于说明一项检查已满足，或当前没有发现对应风险。`PASS` 不等于真实图像模型验证通过。

## 3. Domain Alignment

| Domain | `BLOCKER` 仅保留 | `WARNING` / `NOTE` 示例 |
|---|---|---|
| Reference Role | 角色身份、场景事实或关键道具被错误职责覆盖；同一事实存在无法调和的冲突 | 缺 ID、职责过多、优先级未写清、无参考图但用户希望高度一致 |
| Continuity | 关键状态无因果反转、核心道具消失、人物/空间不可接续 | 字段缺失、状态 Cue 不完整、建议补 Wide Reset |
| Geography | 瞬移、无理由越轴导致关系不可解释、出口/目标矛盾 | 地理变弱、锚点不足、Reset 不够积极 |
| Annotation | Profile 互相声明、颜色语义冲突、注释颜色改变用户要求的画面媒介 | Profile 不匹配、颜色过多、文字过长 |
| Motion DNA | 违反用户明确禁用动作，或与已确定身体状态直接矛盾 | 动作同质化、形容词过多、Signature Move 重复、恢复偏弱 |
| Action | 关键动作结果/对象缺失而无法理解，或触发 Continuity/Geography 的硬矛盾 | Cause → Effect 弱、阶段重复、升级不足、结尾普通 |
| Style / Procedural Montage | 最终风格与故事板绘制媒介直接冲突且无法分层；关键过程状态缺失导致序列不可理解 | 风格隔离表达不清、曲线意图不足、过程状态或中断节拍需要补充 |

“某个领域有一条 `BLOCKER`”不代表它自动阻断所有任务；先确认该领域已被当前模式和用户请求启用。`ACTION_PREVIS` 的规则不得用于判断 Dialogue、Horror 或 Comedy 的静止与停顿。

## 4. Mode-Safe Stillness

以下内容在有清晰 Cinematic Function、状态变化、信息变化或情绪意图时是有效输出，不得仅因“没有明显身体动作”而报错：

- `DIALOGUE`：Silence、Hold、Listener Reaction、Eyeline、Power Shift、呼吸或道具停顿。
- `HORROR_SUSPENSE`：静止、等待、Negative Space、Offscreen Pressure、False Calm、Delayed Reveal、受限可见性和未解决结尾。
- 其他模式：只要该格承担 `RHYTHM`、`EMOTION`、`INFORMATION`、`REVEAL` 或其他明确 Cinematic Function。

如果上述静止格没有可解释功能，可记为 `WARNING`；只有它同时造成用户明确要求缺失或连续性硬矛盾时，才升级为 `BLOCKER`。

## 5. Validation Notes Template

默认在输出第四段的 `Validation Notes` 中使用摘要，不暴露完整内部状态 YAML：

```text
Validation Notes
- PASS: [已满足的硬约束或领域检查]
- WARNING: [可修复的专业风险；注明 Panel / Domain]
- NOTE: [有意选择、默认值、未执行的实测或可选建议]
- BLOCKER: [仅列出明确硬问题；若存在，修复后再交付]
Status: READY | NEEDS_REVISION
Evidence: DIRECTOR_PLAN_CHECK | GOLDEN_CASE_RUN:[id/date]
```

规则：

- 没有对应发现时可以省略该级别，但 `Status` 和 `Evidence` 必须保留。
- `Status: NEEDS_REVISION` 仅表示仍有 `BLOCKER`，不是对 WARNING 的惩罚。
- `Evidence: DIRECTOR_PLAN_CHECK` 表示规则层推理检查；只有实际运行并记录 Golden Case 后才填写其 ID 和日期。
- 示例：“`PASS`：录音笔位置连续；`WARNING`：P07 后空间锚点变弱，建议 P08 使用 Wide Reset；`NOTE`：Horror 的静止格承担 False Calm。”
