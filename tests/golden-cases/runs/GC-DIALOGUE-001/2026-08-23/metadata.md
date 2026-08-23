# GC-DIALOGUE-001 实测元数据

- **Case:** 厨房里的录音笔
- **Date:** 2026-08-23
- **Execution:** 内置图像生成工具
- **Profile:** `DIALOGUE` + `ANNOTATION_SIMPLE`
- **Target:** 16:9、8 Panel、4×2、粗糙黑白对白分镜、低频摄影机运动
- **Input source:** `tests/golden-cases/dialogue-power-shift.md`
- **Output:** [dialogue-simple.png](./dialogue-simple.png)

## Regression Focus

- 保持父女的 180° 轴线和基本左右关系。
- 录音笔在桌面中心持续可见并承担信息/关系功能。
- 保留 Speaker、Listener Reaction、Insert、Silence/Hold 和 Power Shift。
- 不把 Dialogue 误处理为连续动作或随机运镜。

## Limitations

- 本次是单次生成，不是统计学评测。
- 图中对白和技术说明文字只作为结构趋势观察，不作为文字准确率结论。
