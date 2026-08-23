# V0/V2 Golden Cases

这组案例用于冻结 `storyboard-video-skill` 在进入 Universal Storyboard Director 改造前的行为对照标准。

## 基线边界

- 本目录冻结的是输入、生产目标、结构硬约束和人工评分口径，不伪造图像模型输出。
- 当前仓库没有独立 Skill 执行器，因此首次输出采集需要人工调用 Skill；采集结果应与案例文件分开保存，并注明模型、日期和 Prompt 版本。
- Phase 0 不引入脚本 Validator、JSON Schema、图像相似度比较或运行时依赖。
- 后续修改 `SKILL.md` 或 references 后，按版本从案例库中选择 2–3 个案例回归；V2 至少包含 Procedural Montage 和一个既有模式边界案例。本轮 Ticket 01–08 不执行图像回归。

## 案例清单

| Case ID | 模式 | 目标 | 面板 | 文件 |
|---|---|---|---:|---|
| `GC-ACTION-001` | `ACTION_PREVIS` | Whoking vs Dao Wang 高密度动作预演 | 20 / 5×4 | [action-whoking-vs-dao-wang.md](./action-whoking-vs-dao-wang.md) |
| `GC-DIALOGUE-001` | `DIALOGUE` | 室内对白中的权力反转、视线和反应 | 8 / 4×2 | [dialogue-power-shift.md](./dialogue-power-shift.md) |
| `GC-HORROR-001` | `HORROR_SUSPENSE` | 不可见威胁、停顿和延迟揭示 | 8 / 4×2 | [horror-corridor.md](./horror-corridor.md) |
| `GC-PROCEDURAL-001` | `PROCEDURAL_MONTAGE / ASSEMBLY` | Teleport Device 状态推进与过程装配 | 24 / 4×6 | [procedural-teleport-assembly.md](./procedural-teleport-assembly.md) |

## 手工回归流程

1. 读取一个案例文件中的 `Frozen Input`，保持文字、面板数、画幅和模式不变。
2. 使用当前版本 Skill 生成完整输出；不要额外补充会改变测试目标的故事设定。
3. 按案例中的 `Hard Constraints` 做通过/失败判断。
4. 按统一评分表记录 0–2 分：`0` 未满足，`1` 部分满足，`2` 清晰满足。
5. 记录 `Prompt / Skill 版本`、执行日期、使用的模型或调用方式，以及输出文件路径。
6. 修改后与上一版本对照：硬约束不得从通过变为失败；评分下降需要记录原因。

## 统一评分维度

| 维度 | 关注点 |
|---|---|
| Story Clarity | 不看原文时，故事节拍和结局是否仍可读 |
| Shot Necessity | 每格是否有独立的电影功能，是否存在可压缩重复格 |
| Shot Variety | 景别、角度和运动是否服务功能，而非随机堆叠 |
| Continuity | 方向、位置、道具、状态和空间锚点是否连续 |
| Mode Fidelity | 是否遵守当前模式的特有规则，没有套用 Action 规则 |
| Character / Subject Fidelity | 角色身份、动作语言或表演关系是否稳定 |
| Prompt Consistency | 画幅、风格、面板数、注释和负面约束是否互相矛盾 |
| Final Panel Strength | 结尾是否完成结果、揭示或有效悬念 |

## 结果记录格式

首次采集真实输出时，建议在对应案例目录下保存：

```text
tests/golden-cases/runs/<case-id>/<version>/
├── output.md
├── score.md
└── metadata.md
```

Phase 0 不创建这些运行结果目录；它们只在实际调用 Skill 后产生，避免把未执行结果误当成基线。
