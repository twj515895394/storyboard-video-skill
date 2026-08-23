# Progress Log

## Session: 2026-08-23

### Phase 1: Requirements & Discovery

- **Status:** complete
- **Started:** 2026-08-23
- Actions taken:
  - 阅读当前 Skill、references、Universal 设计文档和 Action Golden Case。
  - 与用户确认 V1 范围、输出契约、Continuity、Annotation 和三个 Golden Case。
- Files created/modified:
  - `.planning/current`
  - `.planning/tasks/phase0-v0-baseline/task_plan.md`
  - `.planning/tasks/phase0-v0-baseline/findings.md`
  - `.planning/tasks/phase0-v0-baseline/progress.md`

### Phase 2: Golden Case Structure

- **Status:** complete
- Actions taken:
  - 创建统一回归协议和三个冻结案例。
- Files created/modified:
  - `tests/golden-cases/README.md`
  - `tests/golden-cases/action-whoking-vs-dao-wang.md`
  - `tests/golden-cases/dialogue-power-shift.md`
  - `tests/golden-cases/horror-corridor.md`

### Phase 3: Documentation Alignment

- **Status:** complete
- Actions taken:
  - 更新 `docs/README.md` 的当前状态和基线索引。
  - 更新 Universal 设计文档的 Phase 0、Golden Case 和下一步说明。
- Files created/modified:
  - `docs/README.md`
  - `docs/UNIVERSAL_STORYBOARD_DESIGN.md`

### Phase 4: Verification

- **Status:** complete
- Actions taken:
  - 检查三份案例均包含 Case Card、Frozen Input、Hard Constraints、Human Review Prompts 和 Minimum Regression Score。
  - 检查新增 Markdown 无尾随空白，文档差异可正常通过 `git diff --check`。
  - 确认 `SKILL.md`、现有 references 和 `agents/openai.yaml` 没有被修改。
  - 确认真实图像模型输出未执行，未将规范误报为实测结果。
- Files created/modified:
  - 无新增文件；完成只读验证。

### Phase 5: Delivery

- **Status:** complete
- Actions taken:
  - 更新任务规划、发现记录和进度记录。
  - 完成 Phase 0 基线资产交付说明。
- Files created/modified:
  - `.planning/tasks/phase0-v0-baseline/task_plan.md`
  - `.planning/tasks/phase0-v0-baseline/findings.md`
  - `.planning/tasks/phase0-v0-baseline/progress.md`

## Test Results

| Test | Input | Expected | Actual | Status |
|------|-------|----------|--------|--------|
| 需求确认 | 用户确认全部基线决策 | V1 范围与 Phase 0 资产明确 | 已确认 | PASS |
| 当前实现勘测 | 仓库文件与设计文档 | 确认无执行器/测试脚本 | 已确认 | PASS |
| Golden Case 结构 | 三份案例文件 | 必须章节完整 | 三份均完整 | PASS |
| 文档格式 | 新增/修改 Markdown | 无尾随空白、差异可检查 | `git diff --check` 通过 | PASS |
| 运行逻辑保护 | `SKILL.md`、references、`agents/openai.yaml` | 不应被 Phase 0 修改 | 无差异 | PASS |
| 模型实测 | 当前仓库能力 | 不伪造输出 | 未执行，已明确记录 | NOT RUN |
| Core wiring | `SKILL.md` + `references/storyboard-core.md` | 主 Skill 必须先加载核心参考 | 断言通过 | PASS |
| Mode guardrails | Action/Dialogue/Horror 规则 | Action 不得污染其他模式 | 断言通过 | PASS |
| Static format | 主 Skill 与核心参考 | 无尾随空白、行数可维护 | 检查通过 | PASS |

## Error Log

| Timestamp | Error | Attempt | Resolution |
|-----------|-------|---------|------------|
| 2026-08-23 | 初次批量补丁与初始化模板上下文不匹配 | 1 | 读取当前模板后改用完整文件替换补丁 |
| 2026-08-23 | 静态断言使用了与实际句子不同的词序 | 1 | 按实际契约文本重跑断言，确认模式保护存在 |
| 2026-08-23 | 连续性静态断言误用了表格列顺序 | 1 | 按实际 `State Activation` 表格顺序重跑断言，确认三档状态存在 |
| 2026-08-23 | Action 文档状态断言使用了过窄的完整短语 | 1 | 按实际文案拆分为 Whoking 与 reference 路径断言，确认文档状态一致 |

## 5-Question Reboot Check

| Question | Answer |
|----------|--------|
| Where am I? | Phase 7：Phase 1 静态验证已完成 |
| Where am I going? | Phase 2：Reference Role System |
| What's the goal? | 在不破坏 V0 基线的前提下演进 Universal Storyboard Skill |
| What have I learned? | 当前项目仍是纯 Markdown Prompt Skill，核心规则已拆出到 `storyboard-core.md` |
| What have I done? | 完成 Core Architecture、Mode Router 和模式隔离规则 |

### Phase 6: Core Architecture / Mode Router

- **Status:** complete
- **Started:** 2026-08-23
- Actions taken:
  - 复核现有 `SKILL.md` 与 references，确认需要新增核心规则层。
  - 确认本阶段不重复创建题材 references，不引入代码执行器。
  - 新增 `storyboard-core.md`，定义 Production Target、Mode Router、Cinematic Function、Director Plan、模式保护和 Preflight Validation。
  - 更新 `SKILL.md`，接入核心 reference 和新的九步工作流。
  - 同步 `README.md` 的文件结构说明。
- Files created/modified:
  - `references/storyboard-core.md`
  - `SKILL.md`
  - `README.md`

### Phase 7: Phase 1 Verification

- **Status:** complete
- Actions taken:
  - 核心 reference wiring、四段默认输出和关键模式保护断言通过。
  - `git diff --check` 和新增文件尾随空白检查通过。
  - 确认 `references/shot-language.md`、`references/storyboard-formats.md`、`agents/openai.yaml` 未修改。
  - 真实图像模型未执行，仍需后续 Golden Case 实测。
- Files created/modified:
  - 无新增文件；完成静态验证。

### Phase 8: Reference Role System

- **Status:** complete
- **Started:** 2026-08-23
- Actions taken:
  - 复核现有 `Reference Map` 占位字段和 Universal 设计文档中的参考图角色定义。
  - 确定本阶段采用角色控制边界、用户优先级和显式 WARNING 冲突策略。
  - 新增 `reference-image-system.md`，定义 13 种 Reference Role、控制边界、优先级、Prompt 编译格式和 Validator。
  - 更新核心 reference、主 Skill、README 和设计文档。
- Files created/modified:
  - `references/reference-image-system.md`
  - `references/storyboard-core.md`
  - `SKILL.md`
  - `README.md`
  - `docs/README.md`
  - `docs/UNIVERSAL_STORYBOARD_DESIGN.md`

### Phase 9: Phase 2 Verification

- **Status:** complete
- Actions taken:
  - 角色词汇、控制边界、优先级和冲突校验断言通过。
  - 无参考图不创建 Reference Map 的行为约束断言通过。
  - README、核心 reference、主 Skill 和设计文档引用一致。
  - `git diff --check` 与尾随空白检查通过；真实图像模型未执行。
- Files created/modified:
  - 无新增文件；完成静态验证。

### Phase 10: Continuity State + Scene Geography

- **Status:** complete
- **Started:** 2026-08-23
- Actions taken:
  - 复核设计文档中的 Scene Geography、Character State 和 Continuity In/Out 约定。
  - 确定采用完整/轻量/不强制三档状态策略，避免所有任务都被完整状态表绑架。
  - 新增 `scene-geography.md`，定义空间锚点、Camera Axis、运动路径、越轴和 Active Wide Reset。
  - 新增 `continuity-system.md`，定义 State Activation、State Domains、Panel In/Out、状态传播和按模式规则。
  - 更新核心 reference、主 Skill、README 和设计文档。
- Files created/modified:
  - `references/scene-geography.md`
  - `references/continuity-system.md`
  - `references/storyboard-core.md`
  - `SKILL.md`
  - `README.md`
  - `docs/README.md`
  - `docs/UNIVERSAL_STORYBOARD_DESIGN.md`

### Phase 11: Phase 3 Verification

- **Status:** complete
- Actions taken:
  - Scene Geography、Continuity State、模式启用范围和冲突等级断言通过。
  - `git diff --check` 与尾随空白检查通过。
  - 确认真实图像模型未执行，后续仍需 Golden Case 实测。
- Files created/modified:
  - 无新增文件；完成静态验证。

### Phase 12: Annotation Profiles

- **Status:** complete
- **Started:** 2026-08-23
- Actions taken:
  - 复核现有 `shot-language.md` 的颜色语义和设计文档中的四种 Profile。
  - 确定由 `annotation-system.md` 负责 Profile 选择和冲突隔离，`shot-language.md` 继续负责通用镜头词汇。
  - 新增 `annotation-system.md`，定义 SIMPLE、PRO、CLEAN、LEGACY_BLUE 的选择、颜色语义、Prompt Legend 和 Validator。
  - 更新核心 reference、主 Skill、README、`shot-language.md` 和设计文档。
- Files created/modified:
  - `references/annotation-system.md`
  - `references/storyboard-core.md`
  - `SKILL.md`
  - `README.md`
  - `references/shot-language.md`
  - `docs/README.md`
  - `docs/UNIVERSAL_STORYBOARD_DESIGN.md`

### Phase 13: Phase 4 Verification

- **Status:** complete
- Actions taken:
  - 四种 Profile、默认选择、颜色语义、Legacy 隔离和黑白媒介保护断言通过。
  - 确认 `shot-language.md` 仅提供 PRO 基础颜色词汇，Profile 选择由新 reference 负责。
  - `git diff --check` 与尾随空白检查通过；真实图像模型未执行。
- Files created/modified:
  - 无新增文件；完成静态验证。

### Phase 14: Action Previs + Motion DNA

- **Status:** complete
- **Started:** 2026-08-23
- Actions taken:
  - 复核 Whoking Golden Case、Motion DNA、Cause → Effect 和 Active Wide Reset 设计。
  - 确定 Motion DNA 负责角色动作语言，Continuity 负责跨 Panel 状态，Choreography 负责因果阶段。
  - 新增 `character-motion-dna.md`，定义动作 DNA 字段、提取优先级、Panel 采样和角色动作校验。
  - 新增 `action-choreography.md`，定义 Action State Graph、Cause → Effect、Action Panel Contract、升级和结尾规则。
  - 更新核心 reference、主 Skill、README 和设计文档。
- Files created/modified:
  - `references/character-motion-dna.md`
  - `references/action-choreography.md`
  - `references/storyboard-core.md`
  - `SKILL.md`
  - `README.md`
  - `docs/README.md`
  - `docs/UNIVERSAL_STORYBOARD_DESIGN.md`

### Phase 15: Phase 5 Verification

- **Status:** complete
- Actions taken:
  - Motion DNA、Action State Graph、Cause → Effect、Active Wide Reset 和 Action Validator 断言通过。
  - 确认 Action references 明确声明不得用于 Dialogue/Horror 全局路径。
  - `git diff --check` 与尾随空白检查通过；真实图像模型未执行。
- Files created/modified:
  - 无新增文件；完成静态验证。

### Phase 16: Whoking Generation Comparison

- **Status:** complete
- **Started:** 2026-08-23
- Actions taken:
  - 使用内置图像生成工具生成三组 16:9、20 Panel / 5×4 对照图。
  - 生成 Original/Legacy、Universal + `ANNOTATION_LEGACY_BLUE`、Universal + `ANNOTATION_PRO` 三个 Variant。
  - 将生成结果复制到 Golden Case 运行目录。
  - 完成一次人工视觉评分和差异观察。
- Files created/modified:
  - `tests/golden-cases/runs/GC-ACTION-001/2026-08-23/variant-a-original-legacy.png`
  - `tests/golden-cases/runs/GC-ACTION-001/2026-08-23/variant-b-universal-legacy.png`
  - `tests/golden-cases/runs/GC-ACTION-001/2026-08-23/variant-c-universal-pro.png`
  - `tests/golden-cases/runs/GC-ACTION-001/2026-08-23/metadata.md`
  - `tests/golden-cases/runs/GC-ACTION-001/2026-08-23/score.md`

### Phase 17: Phase 6 Verification

- **Status:** complete
- Actions taken:
  - C Variant 的动作/摄影机/构图注释分离最清楚，暂定为 Action 推荐 Profile。
  - B Variant 证明 Legacy Profile 可保留为兼容层，但不应成为默认。
  - A Variant 作为原始对照保留；单次生成结果不作为统计结论。
- Files created/modified:
  - 无新增文件；完成人工对照记录。

### Phase 18: Dialogue/Horror Regression

- **Status:** complete
- **Started:** 2026-08-23
- Actions taken:
  - 使用内置图像生成工具生成 Dialogue / `ANNOTATION_SIMPLE` 和 Horror / `ANNOTATION_CLEAN` 两张回归图。
  - 将生成结果复制到两个 Golden Case 的运行目录。
  - 完成两组人工评分和非动作模式污染检查。
- Files created/modified:
  - `tests/golden-cases/runs/GC-DIALOGUE-001/2026-08-23/dialogue-simple.png`
  - `tests/golden-cases/runs/GC-DIALOGUE-001/2026-08-23/metadata.md`
  - `tests/golden-cases/runs/GC-DIALOGUE-001/2026-08-23/score.md`
  - `tests/golden-cases/runs/GC-HORROR-001/2026-08-23/horror-clean.png`
  - `tests/golden-cases/runs/GC-HORROR-001/2026-08-23/metadata.md`
  - `tests/golden-cases/runs/GC-HORROR-001/2026-08-23/score.md`

### Phase 19: Phase 7 Verification

- **Status:** complete
- Actions taken:
  - Dialogue 核心指标：Continuity、Mode Fidelity、Final Panel Strength = 2。
  - Horror 核心指标：Story Clarity、Continuity、Mode Fidelity、Final Panel Strength = 2。
  - 确认静止/等待没有被 Action Dead Air 规则删除或替换为连续动作。
  - 记录 Dialogue 第 7 格子画面拆分问题，暂不修改模式边界。
- Files created/modified:
  - 无新增文件；完成回归评分记录。

### Phase 20: Soft Validator Policy

- **Status:** in_progress
- **Started:** 2026-08-23
- Actions taken:
  - 根据用户确认，将 Validator 分为基础硬校验和专业软校验。
  - 确定专业规则继续保留，但默认输出 WARNING/NOTE，不因审美或质量不足阻断交付。
- Files created/modified:
  - 待创建 `references/storyboard-validator.md`
  - 待创建 `tests/validator-cases.md`
