# GC-ACTION-001：Whoking vs Dao Wang

## Case Card

- **Mode:** `ACTION_PREVIS`
- **Production target:** 16:9 单张 Storyboard Sheet，20 Panel，5×4 网格，粗糙但清晰的黑白动作预演，彩色注释可按 Profile 使用
- **Source of truth:** [ACTION_PREVIS_WHOKING_VS_DAO_WANG.md](../../docs/examples/ACTION_PREVIS_WHOKING_VS_DAO_WANG.md) 第 2 节的原始 Prompt
- **Rule references:** [character-motion-dna.md](../../references/character-motion-dna.md) 与 [action-choreography.md](../../references/action-choreography.md)
- **Primary risk:** 高密度动作中的 Combat Geography、Cause → Effect、Motion DNA 和连续性
- **Expected annotation:** 默认 `ANNOTATION_PRO`；如测试兼容性，另行记录 `ANNOTATION_LEGACY_BLUE`

## Frozen Input

使用上述 Source of truth 中保存的完整原始 Prompt，保持以下条件不变：

- Whoking：敏捷、弹簧式、低重心、突然变向、空中重定向、顽皮假动作
- Dao Wang：沉静、精确、盘绕式闪避、窄线反击、节省动作、威胁感
- 场景：破败庭院、断柱、尘土、连续交战
- 结构：20 个连续动作格，从发起、接近、闪避、接触、重定向、升级到最终制胜结果
- 结尾：Dao Wang 的蛇形手停在 Whoking 喉部，Whoking 仍处于运动和失衡中，形成强剪影

## Hard Constraints

- [ ] 必须生成 20 个 Panel，布局为 5×4。
- [ ] 必须有 Project Card 和 Continuity Header。
- [ ] 必须明确区分 `subject_motion` 与 `camera_movement`。
- [ ] 初始左右关系、主要运动方向和关键庭院锚点必须可读。
- [ ] Whoking 与 Dao Wang 的 Motion DNA 必须持续可区分。
- [ ] 攻击、闪避、格挡和反击必须能从前后格找到因果关系，不得只是随机姿势。
- [ ] Close-up 或 Insert 密集后，必须有承担空间重建作用的 Wide 或 Active Wide Reset。
- [ ] 不得出现重复 stare-down、重复静态 Hero Pose 或无功能的空白暂停。
- [ ] 最终 Panel 必须表达动作结果，而不是普通站立英雄姿势。
- [ ] 使用 `ANNOTATION_PRO` 时，红色只表示主体/物体运动，蓝色只表示摄影机运动，绿色表示构图/重构图。

## Human Review Prompts

- 观众能否在不读原文的情况下判断两人的空间关系？
- 每次 Counter 是否回应上一格的动作？
- P09 类似的 Active Wide Reset 是否真正恢复地理关系，同时保持动作活性？
- 镜头变化是否服务动作功能，而不是为了凑齐镜头类型？
- 最后一格是否同时完成胜负倾向、动作结果和剪影记忆点？

## Minimum Regression Score

`Continuity`、`Mode Fidelity`、`Character / Subject Fidelity`、`Final Panel Strength` 四项不得低于 2；其余统一评分维度不得低于 1。
