# GC-ACTION-001 实测元数据

- **Case:** Whoking vs Dao Wang
- **Date:** 2026-08-23
- **Execution:** 内置图像生成工具
- **Intent:** 生成同一 Action Previs 案例的三种注释/导演规则对照
- **Input source:** `docs/examples/ACTION_PREVIS_WHOKING_VS_DAO_WANG.md` 第 2 节及 `tests/golden-cases/action-whoking-vs-dao-wang.md`
- **Target:** 16:9、20 Panel、5×4、粗糙黑白动作故事板、20 格可读、Whoking/Dao Wang 动作 DNA 可区分

## Variants

| Variant | Prompt profile | 目的 | 输出 |
|---|---|---|---|
| A | Original / Legacy semantics | 原始 Prompt 对照组；红框=构图，蓝箭头混合表达动作/力线/摄影机 | [variant-a-original-legacy.png](./variant-a-original-legacy.png) |
| B | Universal Director + `ANNOTATION_LEGACY_BLUE` | 测试加入 Project Card、Continuity Header、Motion DNA 和动作阶段后，Legacy 注释是否仍可兼容 | [variant-b-universal-legacy.png](./variant-b-universal-legacy.png) |
| C | Universal Director + `ANNOTATION_PRO` | 测试红=主体运动、蓝=摄影机、绿=构图的语义分离方案 | [variant-c-universal-pro.png](./variant-c-universal-pro.png) |

## Prompt Set Summary

- 三个 Variant 统一使用 20 Panel / 5×4、16:9、黑白粗糙 Storyboard Sheet、Whoking vs Dao Wang、破败庭院和最终喉线停手。
- A 保留原始 Legacy 语义。
- B 增加 Project Card、Continuity Header、Camera Axis、动作阶段和 Active Wide Reset，但保留 Legacy 语义。
- C 增加同样的导演结构，并使用 `ANNOTATION_PRO` 的分离颜色语义。

## Limitations

- 本次是单次三 Variant 人工对照，不是统计学评测。
- 图像中的标题、字幕和微型文字只作为可读性趋势观察，不作为文字识别准确率结论。
- 尚未进行 Image-to-Video 实测。
