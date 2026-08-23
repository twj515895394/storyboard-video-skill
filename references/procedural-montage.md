# Procedural Montage Reference

Procedural Montage 是以状态转移为核心的故事板序列：观众需要看懂对象从 Start State 如何经过一组有因果的步骤到达 End State。它不是 Action Choreography 的慢速版本；Action 关注对抗、空间和身体因果，Procedural Montage 关注过程依赖、状态推进和压缩时间。

## 1. Subtypes

`ASSEMBLY`、`REPAIR`、`CRAFT`、`COOKING`、`PREPARATION`、`TRANSFORMATION`、`ACTIVATION`、`EXPERIMENT`、`CONSTRUCTION`、`MAINTENANCE`、`WORKFLOW`。

Subtype 只提供领域词汇和例子，不改变核心规则加载。

## 2. 六问适用性判断器

逐项回答 Yes/No：

1. 是否存在明确 `Start State` / `End State`？
2. 中间状态是否可以被视觉化？
3. 步骤之间是否存在 `Cause → Effect`？
4. 过程本身是否构成观看价值？
5. 是否适合用多镜头压缩时间？
6. 是否需要观众理解进度、变化或完成度？

大多数为 Yes 时选择 `PROCEDURAL_MONTAGE`；只有 1–2 项满足时不强制选择。纯情感停顿、纯空间探索、对白权力变化和连续身体动作表演不应被误路由到此模式。

## 3. 导演结构

可用结构：`HOOK → PARTS REVEAL → COMMIT → EARLY PROGRESS → ACCELERATION → INTERRUPTION → RECOVERY → FINAL STEP → ACTIVATION → CONSEQUENCE`。短序列可以合并阶段，但不能删除状态因果。

- **Show Progress, Not Repetition**：每格至少推进一个新状态、关系或可见完成度。
- **Before / During / After**：关键步骤分别让前状态、转移中的关键动作和后状态可读；不把三个时间状态塞进同一 Panel。
- **Recognizable Tools and Parts**：工具、部件和接触关系必须能被看见并服务因果。
- **Macro Inserts / Wide Reset**：在关键接口、危险细节或空间关系不清时使用 Insert；连续近景后用 Wide Reset 恢复整体进度。
- **Interruption Beat**：加入火花、滑落、卡顿、警报或意外结果等中断；中断必须改变下一步决策。
- **Final Consequence**：结尾必须是完成/激活后的结果，再给出新问题、代价或悬念，而不是只写“完成”。

默认配置：`Continuity = FULL`，强制启用 `PROP_STATE_CONTINUITY` 与 `PROCESS_CONTINUITY`；必须先规划 Rhythm/Escalation Curve；Annotation 推荐 `CLEAN_EXTERNAL_METADATA` 或 `ANNOTATION_SIMPLE`。

## 4. 嵌入 Pattern

当其他模式是 `PRIMARY_MODE` 时，Procedural Montage 可作为局部 `SECONDARY_PATTERN`，只加载状态推进、过程压缩和中断规则，不改变主模式的总体导演约束。例如动作片中的“穿装备”片段仍由 `ACTION_PREVIS` 负责主体动作与空间，装配段落借用本 Pattern 负责部件状态。

## 5. Validator 关注点

- `PASS`：Start/End State、关键步骤和结果清楚，Panel 之间的状态可推导。
- `WARNING`：过程有重复、曲线没有明确意图、工具或部件辨识度不足、Interruption 对后续决策影响不清。
- `BLOCKER`：关键道具凭空消失、过程依赖无法成立、状态发生不可解释的硬回退，或主模式与用户明确要求冲突。

以上严重度沿用 `references/storyboard-validator.md`，不在本 reference 重新定义全局阻断规则。
