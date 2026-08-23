# Phase 0 V0 Golden Baseline Implementation Plan

> **For agentic workers:** Execute this plan inline, task by task, and keep the checkbox/status state synchronized with the planning files.

**Goal:** 为当前 Storyboard Skill 冻结动作、对白、恐怖三类 V0 Golden Case 的输入和可验证验收标准，形成后续改造的对照基线。

**Architecture:** 采用纯 Markdown 基线资产。每个 Golden Case 单独描述冻结输入、生产目标、不可退化的结构指标和人工评分项；统一协议负责规定回归顺序与判定口径。本轮不修改 `SKILL.md`、现有 references 或运行入口。

**Tech Stack:** Markdown、Git、`rg`、`git diff --check`；不引入运行时依赖或自动化测试框架。

---

## Phases

### Phase 1: Requirements & Discovery

- [x] 阅读现有 `SKILL.md`、references 与设计文档
- [x] 确认 V1 范围、默认输出、Continuity、Golden Case 范围
- [x] 记录当前项目无独立执行器、测试脚本和已采集模型输出
- **Status:** complete

### Phase 2: Golden Case Structure

- [x] 创建 `tests/golden-cases/README.md`，定义执行流程、评分维度和回归规则
- [x] 创建动作案例，固定 Whoking vs Dao Wang 的 16:9、20 面板 Action Previs 目标
- [x] 创建对白案例，固定室内两人权力反转和对白/反应连续性目标
- [x] 创建恐怖案例，固定走廊中不可见威胁和静止镜头有效性的目标
- **Status:** complete

### Phase 3: Documentation Alignment

- [x] 在 `docs/README.md` 标记 Phase 0 基线已建立，并列出基线资产
- [x] 在 `docs/UNIVERSAL_STORYBOARD_DESIGN.md` 更新 Phase 0、测试策略和下一步说明
- **Status:** complete

### Phase 4: Verification

- [x] 检查三个案例都包含输入、生产目标、结构约束和评分口径
- [x] 检查文档链接、Markdown 格式和工作区差异
- [x] 明确未执行真实图像模型生成，避免把规范误报为模型实测结果
- **Status:** complete

### Phase 5: Delivery

- [x] 更新 `findings.md` 和 `progress.md`
- [x] 汇总新增文件、验证结果和下一步 Phase 1 入口
- **Status:** complete

## Decisions Made

| Decision | Rationale |
|----------|-----------|
| V1 先做 Core Architecture、Mode Router、Narrative 与 Action Previs | 控制范围，避免一次性实现十个模式 |
| Phase 0 只新增 Golden Case 与回归协议 | 先冻结对照标准，再修改运行逻辑 |
| 三个案例为 Action、Dialogue、Horror | 覆盖运动连续性、表演反应和静止/未知压力三种核心风险 |
| 不伪造模型输出 | 当前仓库没有独立执行器或已保存输出，规范与实测必须分开 |
| 默认输出仍以 Brief、Panel Table、Prompt、Validation 为核心 | 与已确认的 V1 输出契约一致 |

### Phase 6: Core Architecture / Mode Router

- [x] 创建 `references/storyboard-core.md`，定义生产目标、模式路由、电影功能、最小 Director Plan 和内部校验
- [x] 更新 `SKILL.md`，接入核心参考、Primary/Secondary Mode、模式边界和新的默认输出契约
- [x] 保留现有 `shot-language.md` 与 `storyboard-formats.md`，避免 Phase 1 重复抽取题材知识
- **Status:** complete

### Phase 7: Phase 1 Verification

- [x] 检查主 Skill 是否明确加载核心参考并保留现有四段输出结构
- [x] 检查 Action 规则没有成为全局规则，Dialogue/Horror 的静止与停顿仍被允许
- [x] 运行文档结构、空白和差异检查，并记录未执行真实模型生成
- **Status:** complete

### Phase 8: Reference Role System

- [x] 创建 `references/reference-image-system.md`，定义 Reference Role、Reference Map、控制边界和冲突优先级
- [x] 更新 `storyboard-core.md`，让 Director Plan 和 Preflight Validation 使用角色化参考图规则
- [x] 更新 `SKILL.md`，在有参考图时按角色加载和编译 Reference Map，无参考图时保持静默
- [x] 同步 README 与设计文档中的 reference 目录和 Phase 状态
- **Status:** complete

### Phase 9: Phase 2 Verification

- [x] 检查角色枚举、优先级、冲突处理和禁止控制项完整
- [x] 检查无参考图路径不会要求用户补充虚构的 Reference Map
- [x] 检查主 Skill、核心 reference、README 和设计文档引用一致
- [x] 运行 Markdown、空白和差异检查，并记录未执行真实图像模型生成
- **Status:** complete

### Phase 10: Continuity State + Scene Geography

- [x] 创建 `references/scene-geography.md`，定义空间锚点、Camera Axis、运动路径和 Active Wide Reset 触发条件
- [x] 创建 `references/continuity-system.md`，定义 Continuity State、Panel In/Out、状态传播和模式启用范围
- [x] 更新 `storyboard-core.md` 和 `SKILL.md`，接入连续性/地理 reference 与分级状态策略
- [x] 同步 README 与设计文档中的 references 和阶段状态
- **Status:** complete

### Phase 11: Phase 3 Verification

- [x] 检查 Action 完整状态、Dialogue/Horror 轻量状态、Keyframe 无强制状态的边界
- [x] 检查 Camera Axis、屏幕方向、空间锚点和 Panel In/Out 规则完整
- [x] 检查连续性冲突的 PASS / WARNING / BLOCKER 口径
- [x] 运行 Markdown、空白和差异检查，并记录未执行真实图像模型生成
- **Status:** complete

### Phase 12: Annotation Profiles

- [x] 创建 `references/annotation-system.md`，定义 SIMPLE、PRO、CLEAN、LEGACY_BLUE 的选择、颜色语义和冲突规则
- [x] 更新 `storyboard-core.md` 和 `SKILL.md`，接入按模式/用户要求选择 Annotation Profile 的流程
- [x] 同步 README 与设计文档中的 annotation reference 和阶段状态
- **Status:** complete

### Phase 13: Phase 4 Verification

- [x] 检查四种 Profile 的默认选择、颜色语义和 Legacy 隔离
- [x] 检查黑白画面规则与彩色注释规则没有混淆
- [x] 检查 `shot-language.md` 与新 Profile reference 的职责边界
- [x] 运行 Markdown、空白和差异检查，并记录未执行真实图像模型生成
- **Status:** complete

### Phase 14: Action Previs + Motion DNA

- [x] 创建 `references/character-motion-dna.md`，定义角色动作 DNA、状态字段、采样规则和禁止动作
- [x] 创建 `references/action-choreography.md`，定义 Cause → Effect、动作阶段、可读性、Active Wide Reset 和结尾结果
- [x] 更新 `storyboard-core.md` 和 `SKILL.md`，让深度动作规则只在 `ACTION_PREVIS` 生效
- [x] 将 Whoking Golden Case 接入 Action 规则与回归说明
- [x] 同步 README 与设计文档中的 references 和阶段状态
- **Status:** complete

### Phase 15: Phase 5 Verification

- [x] 检查 Motion DNA 与 Identity Reference、Continuity State 的职责边界
- [x] 检查动作阶段、因果链、空间重建和最终结果规则完整
- [x] 检查 Action 规则没有进入 Dialogue/Horror 全局路径
- [x] 运行 Markdown、空白和差异检查，并记录未执行真实图像模型生成
- **Status:** complete

### Phase 16: Whoking Generation Comparison

- [x] 生成 Original / Legacy 对照图
- [x] 生成 Universal + `ANNOTATION_LEGACY_BLUE` 对照图
- [x] 生成 Universal + `ANNOTATION_PRO` 对照图
- [x] 保存三张图、元数据和人工评分到 Golden Case runs 目录
- **Status:** complete

### Phase 17: Phase 6 Verification

- [x] 比较 Story Clarity、Continuity、Motion DNA、Action Readability 和 Ending
- [x] 记录 Legacy 兼容层与 PRO 默认推荐的差异
- [x] 记录单次生成样本的局限，不把一次对照当作统计结论
- **Status:** complete

### Phase 18: Dialogue/Horror Regression

- [x] 生成 `GC-DIALOGUE-001` 的 `ANNOTATION_SIMPLE` 回归图
- [x] 生成 `GC-HORROR-001` 的 `ANNOTATION_CLEAN` 回归图
- [x] 保存两张图、元数据和人工评分到各自 Golden Case runs 目录
- **Status:** complete

### Phase 19: Phase 7 Verification

- [x] 检查 Dialogue 的 Silence、Eyeline、Reaction、Power Shift 和道具连续性
- [x] 检查 Horror 的静止、Negative Space、False Calm、Delayed Reveal 和未解决结尾
- [x] 确认 Action 规则没有污染 Dialogue/Horror
- [x] 记录 Dialogue 子画面问题，明确归入后续 Prompt Compiler/Validator
- **Status:** complete

### Phase 20: Soft Validator Policy

- [ ] 创建 `references/storyboard-validator.md`，定义基础硬校验、专业软校验、严重级别和输出格式
- [ ] 更新 `storyboard-core.md` 与 `SKILL.md`，接入统一软校验策略
- [ ] 将现有 Reference、Continuity、Geography、Annotation、Motion DNA、Action 规则与统一严重级别对齐
- [ ] 创建 10 条代表性 Validator 校验矩阵，覆盖正常、边界和异常流程
- **Status:** in_progress

### Phase 21: Phase 8 Verification

- [ ] 检查硬校验只覆盖基础契约和明确用户要求
- [ ] 检查专业质量问题默认不会阻断返工
- [ ] 检查 Dialogue/Horror 的静止和 Hold 不会被误判
- [ ] 运行 Markdown、空白、引用和差异检查，并记录未执行自动脚本验证
- **Status:** pending

## Errors Encountered

| Error | Attempt | Resolution |
|-------|---------|------------|
| 初次批量补丁与初始化模板上下文不匹配 | 1 | 读取当前文件后改用删除后重新创建的补丁方式 |
| 静态断言使用了与实际句子不同的词序 | 1 | 按实际契约文本重跑断言，确认模式保护存在 |
| 连续性静态断言误用了表格列顺序 | 1 | 按实际 `State Activation` 表格顺序重跑断言，确认三档状态存在 |
| Action 文档状态断言使用了过窄的完整短语 | 1 | 按实际文案拆分为 Whoking 与 reference 路径断言，确认文档状态一致 |
