# V2 Spec: Procedural Montage、Soft Validator 与通用能力增强

> Status: ready-for-agent
> 日期: 2026-08-23
> 来源: Supplement 01 + Supplement 02 + V1 交接文档 + 对话确认

---

## 问题陈述 (Problem Statement)

当前 Universal Storyboard Director Skill 已经能处理 Action Choreography（动作编排）、Dialogue Performance（对白表演）和 Horror Suspense（恐怖悬疑）三类故事板。但当用户输入一个以"状态推进"为核心逻辑的序列（例如装配设备、烹饪、变装准备、调查拼图、训练蒙太奇等），Skill 缺乏对应的识别、规划和校验能力：

1. Mode Router 不认识 Procedural Montage，无法正确分类。
2. Continuity System 只追踪角色、空间和基础道具身份，缺乏 Prop State（道具状态）和 Process Dependency（过程依赖）追踪。
3. 没有 Single-Moment Rule，导致单格可能描述多个连续动作（V1 Dialogue P07 已暴露此问题）。
4. Style 系统不区分"最终渲染风格"和"故事板绘制风格"，容易导致故事板 Panel 被画成彩色成稿。
5. Annotation System 缺少 CLEAN_EXTERNAL_METADATA Profile，无法将生产信息全部放在 Panel 画面外部。
6. 缺少 Rhythm Curve（节奏曲线）概念，每格节奏独立标注，无法设计整体波形。
7. Validator 策略已确认但尚未落地为独立规则文件。
8. 缺少 Procedural Montage 的 Golden Case（Teleport Device Assembly）。

## 解决方案 (Solution)

在不重构 V1 已有架构的前提下，渐进式扩展：

1. Mode Router 新增 `PROCEDURAL_MONTAGE` 作为一级 Sequence Logic，支持 11 个 Subtype。
2. Continuity System 新增 `PROP_STATE_CONTINUITY` 和 `PROCESS_CONTINUITY`，支持 State Before / State After。
3. 新增通用 Single-Moment Rule，适用于所有模式。
4. Style 系统分离为 `FINAL_VISUAL_STYLE` 和 `STORYBOARD_DRAWING_STYLE`，Prompt Compiler 明确隔离二者。
5. Annotation System 新增 `CLEAN_EXTERNAL_METADATA` Profile。
6. 新增 Rhythm Curve 和 Escalation Curve 概念，作为 Director Plan 的规划工具。
7. 落地 Soft Validator 策略为独立规则文件，统一各 reference 中的严重级别。
8. 新增 Golden Case B（Teleport Device Assembly），建立 10 项验收指标。
9. V2 全部完成后，用 4 个 Golden Case 统一回归校验。

## 用户故事 (User Stories)

1. 作为故事板创作者，我希望输入一个装配/制作/准备类场景时，Skill 能自动识别为 Procedural Montage 而不是 Action，以便生成的故事板按状态推进而非动作对抗来组织。

2. 作为故事板创作者，我希望每个 Panel 只描述一个可提取的视觉瞬间，以便生成的图像不会出现同一人物的多个时间状态（ghost poses / onion-skin）。

3. 作为故事板创作者，我希望在 Procedural Montage 中，关键道具的状态能被连续追踪（例如"电池已安装"后不会在后续格中消失），以便故事板的因果逻辑可读。

4. 作为故事板创作者，我希望能指定"最终视频是赛博朋克风格，但故事板本身是灰色素描"，以便 Panel 不会被画成彩色成稿。

5. 作为故事板创作者，我希望 Panel 内部完全干净（无箭头、标签、字幕），所有生产信息放在画面外部，以便获得 CLEAN_EXTERNAL_METADATA 风格的故事板。

6. 作为故事板创作者，我希望 Skill 在规划阶段就设计整体节奏曲线（Hook → Acceleration → Interruption → Peak → Drop → Final Spike），而不是每格独立标注快慢，以便故事板整体节奏更有设计感。

7. 作为故事板创作者，我希望 Validator 只在明显错误时阻断交付（BLOCKER），专业质量建议以 WARNING/NOTE 呈现，以便减少不必要的返工。

8. 作为故事板创作者，我希望 Procedural Montage 不仅适用于"装配"，还能用于变装、调查、训练、烹饪、空间改造等任何状态推进式序列，以便 Skill 真正具备通用能力。

9. 作为故事板创作者，我希望 Procedural Montage 可以作为其他模式的嵌入片段使用（例如动作电影中的"穿装备"段落），而不是只能作为整段的主模式。

10. 作为故事板创作者，我希望 Teleport Device Assembly 成为第四个 Golden Case，与 Action、Dialogue、Horror 并列回归，以便每次修改都不会让某一类能力退化。

11. 作为故事板创作者，我希望 Procedural Montage 中存在"中断节拍"（Interruption Beat），例如意外火花、工具滑落、警报响起，以便过程不会变成枯燥的教程。

12. 作为故事板创作者，我希望 Procedural Montage 的结尾不是简单的"完成"，而是"完成 → 结果 → 新问题/悬念"，以便故事板具有叙事张力。

13. 作为故事板创作者，我希望在需要展示最终渲染风格时，可以在故事板顶部添加 Style Swatch Strip（风格色板条），而不是让 Panel 本身变成彩色，以便风格参考和故事板绘制分离。

14. 作为故事板创作者，我希望 V2 完成后，Action、Dialogue、Horror、Procedural Montage 四个 Golden Case 一起回归，以便确认所有模式互不污染。

## 实现决策 (Implementation Decisions)

### 架构决策

- **渐进扩展，不重构 V1**：所有新增能力以新增 reference 文件、扩展现有 reference 字段和更新 SKILL.md 工作流的方式接入，不重写已有模块。
- **PROCEDURAL_MONTAGE 作为一级 Mode**：在 Mode Router 中与 ACTION_PREVIS、NARRATIVE、HORROR 等并列，不作为 Action 的子类型。
- **Subtype 不改变核心规则加载**：ASSEMBLY、REPAIR、COOKING 等 Subtype 只影响 Prompt 中的领域词汇和示例，不各自创建独立 reference 文件。
- **PROP_STATE_CONTINUITY 作为 Continuity System 的新增 Domain**：与现有 CHARACTER、SPATIAL、COSTUME 等并列，不替换现有 Domain。
- **State Before / State After 作为 Panel 内部 metadata**：不要求用户手动填写，由 Director Plan 阶段自动推导。
- **Single-Moment Rule 作为通用规则**：写入 storyboard-core.md，适用于所有模式，不限于 Procedural Montage。
- **Style 双层分离**：在 storyboard-core.md 中定义 `FINAL_VISUAL_STYLE` 和 `STORYBOARD_DRAWING_STYLE`，Prompt Compiler 步骤明确声明"Final style does not apply to panel rendering"。
- **CLEAN_EXTERNAL_METADATA 作为第五个 Annotation Profile**：与 SIMPLE、PRO、CLEAN、LEGACY_BLUE 并列。
- **Rhythm Curve 作为 Director Plan 的规划文本**：不引入复杂数据结构，以自然语言描述整体波形。
- **Soft Validator 统一策略**：创建独立 reference 文件，定义 BLOCKER / WARNING / NOTE 三级，各专项 reference 中的校验规则引用统一策略。
- **Style Swatch Strip 作为 Output Profile 的可选能力**：默认关闭，用户要求或 Commercial/Sci-fi/MV 模式下可启用。

### 模块变更

- **新增 `references/procedural-montage.md`**：Procedural Montage 导演规则、Subtype 列表、适用性判断器、叙事结构模板、导演规则（Show Progress Not Repetition、Before/During/After、Interruption、Final Consequence 等）。
- **新增 `references/storyboard-validator.md`**：统一 Validator 策略、严重级别定义、输出格式模板。
- **扩展 `references/continuity-system.md`**：新增 PROP_STATE_CONTINUITY、PROCESS_CONTINUITY Domain，新增 State Before / State After 字段。
- **扩展 `references/storyboard-core.md`**：新增 Single-Moment Rule、Style 双层定义、Rhythm Curve 概念、PROCEDURAL_MONTAGE 路由规则。
- **扩展 `references/annotation-system.md`**：新增 CLEAN_EXTERNAL_METADATA Profile。
- **更新 `SKILL.md`**：工作流步骤接入新增能力。
- **新增 `tests/golden-cases/procedural-teleport-assembly.md`**：Golden Case B 定义。

### 适用性判断器

Skill 在 Mode Router 阶段使用 6 问判断是否适用 Procedural Montage：

1. 是否存在明确 Start State / End State？
2. 中间状态是否可视觉化？
3. 步骤之间是否存在 Cause → Effect？
4. 过程本身是否构成观看价值？
5. 是否适合通过多镜头压缩时间？
6. 是否需要观众理解进度/变化/完成度？

大多数为 Yes 时选择 Procedural Montage；仅 1-2 项满足时不强制使用。

### Procedural Montage 可作为嵌入 Pattern

当 Primary Mode 为其他类型（如 ACTION_PREVIS、HORROR）时，Procedural Montage 可以作为 Secondary Pattern 嵌入局部片段，使用其状态推进和过程压缩规则，但不改变整体模式。

## 测试决策 (Testing Decisions)

- **唯一测试缝隙：Golden Case 生成回归**。每次修改 SKILL.md 或 references 后，用冻结的 Golden Case 输入生成图像，按统一评分表对照，检查核心指标不退化。
- **V2 全部完成后统一校验**：不在每个 Ticket 完成时单独回归；所有规则落地后，从 Golden Case 库中选择 2–3 个案例回归，至少包含 Procedural Montage 和一个既有模式边界案例，不强制一次性运行全部案例。
- **Golden Case B 验收指标**：类型识别、状态推进、Cause → Effect、Panel 单瞬间、Camera Variety、Rhythm Curve、Prop Consistency、Style Separation、Annotation Placement、Ending Consequence，共 10 项。
- **回归标准**：核心指标（Story Clarity、Continuity、Mode Fidelity、Final Panel Strength）不低于 2 分；非核心指标 WARNING 不阻断。

## 超出范围 (Out of Scope)

- **Storyboard IR（中间表示层）**：设计文档中提出的长期方向，不在 V2 实现。
- **多轴 metadata 全面替换 Mode Router**：SEQUENCE_LOGIC、ENERGY_PROFILE、OUTPUT_PROFILE 等多轴描述作为长期方向记录，V2 仅实现 PROCEDURAL_MONTAGE 一级 Mode 和 Rhythm Curve。
- **自动化脚本 Validator**：V2 Validator 仍为 Markdown 规则，不引入可执行脚本。
- **更多 Golden Case（C ~ H）**：Dialogue Performance、Comedy Timing、Product Demo、MV Performance 等留待后续版本。
- **代码执行器 / JSON Schema**：项目继续保持纯 Markdown Skill 形态。
- **ENTITY_STATE_CONTINUITY 通用化**：Supplement 02 提出的 BODY_STATE、INFORMATION_STATE、ENVIRONMENT_STATE 等扩展留待后续版本；V2 先落地 PROP_STATE 和 PROCESS。
- **Animatic 作为 Output Profile 独立拆分**：记录为设计方向，V2 不改动现有 OUTPUT_MODE。

## 补充说明 (Further Notes)

- Supplement 01（1734 行）提供了 Procedural Montage 的完整定义、导演规则、Teleport Golden Case 原始 Prompt、验收指标和非破坏式接入建议。
- Supplement 02（782 行）将 Procedural Montage 从"装配模板"扩展为通用 Sequence Pattern，列举了 10+ 适用场景，明确了不适用边界，并提出 ENTITY_STATE 长期方向。
- V1 Dialogue P07 的子画面拆分问题将被 Single-Moment Rule 解决。
- V1 已确认的 Soft Validator 策略（BLOCKER 只覆盖基础硬约束，其余降级为 WARNING/NOTE）将在本轮正式落地。
- Procedural Montage 与 Action Choreography 共享 Shot Language、Reference Role、Continuity Core、Prompt Compiler、Validator、Style System、Output Profiles，差异在于 Sequence-specific 模块（Process Steps / Prop State / Before-After 等）。
