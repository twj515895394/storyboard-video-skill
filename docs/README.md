# Storyboard Video Skill — Design Docs

本目录用于记录 `storyboard-video-skill` 从当前 Storyboard Prompt / Previs Skill 演进为 **Universal Storyboard Director Skill** 的设计思路、关键决策、Golden Cases 与后续开发依据。

当前阶段的原则是：

> **先冻结设计和测试基线，再分阶段修改 Skill；边开发、边生成、边回归，不一次性大改。**

---

## 文档索引

### 1. [UNIVERSAL_STORYBOARD_DESIGN.md](./UNIVERSAL_STORYBOARD_DESIGN.md)

主设计文档。

包含：

- 当前 Skill 的能力判断与结构性缺口
- Universal Storyboard Director 产品定位
- Project → Sequence → Scene → Shot → Panel 层级模型
- Storyboard Mode Router
- Story Engine
- Reference Role System
- Continuity State / Scene Geography
- Character Motion DNA
- Action Choreography
- Shot Design Model
- Annotation Profiles
- Storyboard Sheet 结构
- Prompt Compiler
- Validator
- 推荐目录结构
- 分阶段开发计划
- Golden Case / 回归测试策略
- 关键设计决策记录

后续涉及架构级变化时，应优先更新该文档。

### 2. [examples/ACTION_PREVIS_WHOKING_VS_DAO_WANG.md](./examples/ACTION_PREVIS_WHOKING_VS_DAO_WANG.md)

`ACTION_PREVIS` Golden Case。

文档完整保留用户收集到的 Whoking vs Dao Wang 20 格 anime-wuxia Storyboard Prompt，并记录：

- 为什么这个 Prompt 值得研究
- Project Card / Continuity Header 设计
- Character Motion DNA
- Cause → Effect 动作逻辑
- Active Wide Reset
- One Clear Action Idea Per Panel
- Final Action Silhouette
- 原 Prompt 红框 / 蓝箭头规则与新系统规范的冲突
- `ANNOTATION_LEGACY_BLUE` 兼容方案
- 后续回归测试指标

该案例不应直接复制进 `SKILL.md`，而应作为设计素材和 Action Previs 回归基线。

### 3. [tests/golden-cases/README.md](../tests/golden-cases/README.md)

V0 Golden Case 回归协议和首批三组冻结案例：

- `ACTION_PREVIS`：Whoking vs Dao Wang
- `DIALOGUE`：室内对白 / 权力反转
- `HORROR_SUSPENSE`：走廊 / 看不见的威胁

这些文件冻结输入、生产目标、硬约束和人工评分口径；真实图像模型输出需要在实际调用 Skill 后另行采集。当前包含 Action、Dialogue、Horror 与 V2 `PROCEDURAL_MONTAGE / ASSEMBLY` 的 Teleport Device Assembly。

---

## 开发方式

建议后续每个阶段遵循：

```text
设计文档确定规则
    ↓
只修改一个核心模块
    ↓
运行 Golden Cases
    ↓
实际生成 Storyboard 图片
    ↓
观察模型遵循率和视觉问题
    ↓
修订 reference / rule
    ↓
回归 Action + Dialogue/Horror
    ↓
再进入下一阶段
```

避免：

```text
一次性创建十几个 references
→ 大幅改写 SKILL.md
→ 最后才测试
```

因为 Storyboard Skill 的质量最终取决于图像模型实际执行效果，而不是 Markdown 结构本身有多完整。

---

## 当前状态

当前仓库仍保留 fork 时的原始运行逻辑，已完成 Phase 0 的三组 V0 Golden Case 输入和回归协议冻结。

Phase 0 新增了 `tests/golden-cases/` 基线资产；Phase 1 已新增核心规则 reference，随后完成了 Reference Role System、Continuity State、Scene Geography、Annotation Profiles 和 Action Previs 规则层。当前仍**尚未修改：**

- `references/storyboard-formats.md`
- `agents/openai.yaml`

Action Previs 规则已落地，并已完成 Whoking Golden Case 的三 Variant 第一轮生成对照；Dialogue/Horror 回归也已完成，运行结果分别见 `tests/golden-cases/runs/GC-ACTION-001/2026-08-23/`、`tests/golden-cases/runs/GC-DIALOGUE-001/2026-08-23/` 和 `tests/golden-cases/runs/GC-HORROR-001/2026-08-23/`。下一阶段进入 Validator。
