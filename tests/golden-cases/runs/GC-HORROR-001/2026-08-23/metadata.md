# GC-HORROR-001 实测元数据

- **Case:** 没有人的脚步声
- **Date:** 2026-08-23
- **Execution:** 内置图像生成工具
- **Profile:** `HORROR_SUSPENSE` + `ANNOTATION_CLEAN`
- **Target:** 16:9、8 Panel、4×2、黑白粗糙恐怖分镜、大块负空间
- **Input source:** `tests/golden-cases/horror-corridor.md`
- **Output:** [horror-clean.png](./horror-clean.png)

## Regression Focus

- 保留 Ordinary Space、Wrong Detail、Reaction、False Calm、Delayed Reveal 和 Unresolved Image。
- 前 7 格不完整展示实体威胁。
- 允许静止/等待成为有效节奏，不使用 Action 的连续加速规则。
- 保持护士站、走廊、电梯和脚印方向的空间连续性。

## Limitations

- 本次是单次生成，不是统计学评测。
- 图中构图说明文字只作为结构趋势观察，不作为文字准确率结论。
