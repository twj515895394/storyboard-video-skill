# Storyboard Core Reference

这是 Universal Storyboard Director 的核心规则层。所有故事板任务先使用本文件完成生产目标解析、模式路由、电影功能分配和输出校验，再按需要读取镜头语言与题材格式参考。严重级别和 Validation Notes 统一由 `references/storyboard-validator.md` 定义。

## 1. Production Target

先把用户请求整理成最小生产目标：

```text
aspect_ratio:
panel_count:
layout:
medium:
color_rules:
sheet_or_separate_frames:
duration: optional
language:
output_mode: FULL | PLAN_ONLY | SHEET_PROMPT | FRAME_PROMPTS | ANIMATIC
```

规则：

- 用户明确指定的值优先。
- 未指定时默认 `16:9`、单张 Storyboard Sheet、12 Panel、粗糙电影预演风格。
- `ACTION_PREVIS` 默认 16 Panel；已明确高密度动作或引用 Whoking 案例时使用 20 Panel / 5×4。
- 不把 Panel 数量自动等同于 Shot 数量；一个 Shot 可以由多个 Panel 展示起始、过程和结果。
- 用户没有要求时间轴时，不擅自添加精确时间戳；只有 `ANIMATIC` 或图生视频需求才增加时长建议。

## 2. Storyboard Mode Router

始终选出一个 `PRIMARY_MODE`。只有在第二种模式确实改变导演约束时，才增加一个 `SECONDARY_MODE`。

### 2.1 可识别模式

| Mode | 识别信号 | 核心关注 |
|---|---|---|
| `NARRATIVE` | 普通故事、场景扩写、剧情段落 | Story Beat、情绪弧线、视觉转折、最终画面 |
| `DIALOGUE` | 对话、谈判、争执、关系变化 | Eyeline、Reaction、Power Shift、Silence |
| `ACTION_PREVIS` | 战斗、追逐、体育、武打、复杂身体动作 | Geography、Cause → Effect、Motion DNA、Active Wide Reset |
| `HORROR_SUSPENSE` | 恐怖、悬疑、未知威胁、调查、惊吓 | Negative Space、Offscreen Pressure、Delay、Reveal Timing |
| `COMEDY` | 喜剧节奏、误会、反转、包袱 | Setup、Misdirection、Hold、Reaction、Payoff |
| `COMMERCIAL` | 产品、品牌、功能展示、广告 | Product Readability、Benefit、Hero Shot |
| `MV_PERFORMANCE` | MV、舞蹈、表演、音乐节拍、歌词 | Rhythm、Motif、Performance、Texture |
| `SHORT_FORM` | 短视频、Reels、TikTok、强 Hook、循环 | Hook、Pattern Interrupt、Compression、Loop |
| `CINEMATIC_KEYFRAME` | 单张关键帧、海报感、概念图、世界观画面 | Iconic Composition、Lighting、Scale、Emotion |
| `ANIMATIC` | 预演、镜头时长、运镜、转场、声音提示 | Duration、Motion、Transition、Coverage |
| `PROCEDURAL_MONTAGE` | 装配、修理、制作、准备、变装、烹饪、训练等状态推进 | Process Progress、State Change、Cause → Effect、Prop State |

### 2.2 路由顺序

1. 用户明确指定模式时直接采用。
2. 用户同时给出多个强信号时，选择最影响镜头决策的模式作为 `PRIMARY_MODE`。
3. 只有第二种模式提供额外且不冲突的约束时，才记录 `SECONDARY_MODE`。
4. 模式仍然不清晰时使用 `NARRATIVE`，不要为了显得专业而强行分类。
5. 混合模式发生冲突时，`PRIMARY_MODE` 的规则优先；`SECONDARY_MODE` 只补充，不覆盖主模式。
6. 对状态推进序列执行 `references/procedural-montage.md` 的六问判断；满足多数问题时才路由为 `PROCEDURAL_MONTAGE`，否则保留更合适的主模式并可嵌入该 Pattern。

### 2.3 Phase 1 的模式边界

- `NARRATIVE` 是通用默认路径。
- `ACTION_PREVIS`、`DIALOGUE`、`HORROR_SUSPENSE` 必须应用本文件中的模式保护规则。
- 其他模式可以被识别并使用 `storyboard-formats.md` 的已有节奏模板；本阶段不提前创建各自的深度 reference 模块。
- “每格必须有明显身体动作”永远不是全局规则。

## 3. Story Engine

正式拆 Panel 前，先提取一份短而可操作的 Story Engine：

```text
protagonist:
desire:
obstacle:
stakes:
emotional_arc:
setting:
key_prop_or_motif:
dominant_motion:
visual_contrast:
opening_image:
final_image:
```

只保留会改变镜头决策的字段。缺失信息可以留空或根据文本做低风险推断，不为填表制造设定。

## 4. Cinematic Function

全局规则是：

> Every panel must create a new cinematic function.

每个 Panel 至少承担一个功能：

```text
GEOGRAPHY   建立或重建空间关系
ACTION      推进动作或身体行为
REACTION    展示角色或环境反应
INFORMATION 传递关键事实、道具或线索
EMOTION     推进情绪或关系
RHYTHM      控制速度、停顿或呼吸
TRANSITION  完成视觉或叙事转接
IMPACT      强调接触、撞击或结果
REVEAL      揭示新信息
PAYOFF      兑现前面的铺垫
PROCESS_PROGRESS 让过程完成度前进
STATE_CHANGE 让对象或环境进入可见新状态
```

重复景别或静止画面只有在功能、信息或情绪发生变化时才保留。Validator 应优先问“这一格为什么存在”，而不是检查镜头类型数量。

## 5. Director Plan（内部模型）

Director Plan 是内部导演设计，不要求用户看到 YAML，也不要求强制 JSON 输出。最小结构如下：

```text
production_target:
primary_mode:
secondary_mode: optional
story_engine:
reference_map: when references exist; use reference-image-system.md
scene_geography: when spatial continuity matters
continuity_state: when state can change between panels
panel_plan:
  - panel:
    function:
    story_beat:
    visual_action:
    shot_language:
    subject_motion:
    camera_movement:
    state_before: when prop/process continuity is active
    state_after: when prop/process continuity is active
    transition:
    continuity_in:
    continuity_out:
output_mode:
rhythm_curve: before panel allocation when sequence rhythm matters
escalation_curve: before panel allocation when pressure or stakes change
```

Shot 与 Panel 不强制一对一：简单任务可以一 Shot 一 Panel；复杂运镜或动作可用多个 Panel 展示一个 Shot 的起始、过程和结果。

有参考图时，必须先用 `references/reference-image-system.md` 把每张图绑定到明确的 Role 和 Target，再把结果传给 Panel 设计和 Prompt Compiler。没有参考图时不创建 Reference Map。

## 6. Mode Guardrails

### `NARRATIVE`

要求场景目标、障碍、视觉转折、情绪弧线和最终画面可读。镜头变化必须服务叙事，不用术语替代故事。

### `ACTION_PREVIS`

要求加载 `references/character-motion-dna.md` 和 `references/action-choreography.md`，先建立 Geography，再设计 Initiation → Approach → Evade/Block → Contact/Miss → Redirection → Recovery → Counter → Escalation。允许持续动作，但仍要保留结果和空间重建；禁止重复 stare-down、随机 Hero Pose 和无因果动作。

### `DIALOGUE`

要求先建立关系和空间，再用 Speaker、Listener Reaction、Gesture/Prop Insert、Power Shift 和 Silence 推进。保持 Eyeline 与屏幕方向；摄影机运动应少而有意。沉默可以是有效的 `RHYTHM` 或 `EMOTION` Panel。

### `HORROR_SUSPENSE`

要求 Ordinary Space → Wrong Detail → Isolated Reaction → Offscreen Pressure → False Calm → Delayed Reveal → Unresolved Image。静止、等待、负空间和受限可见性可以是主要功能；不要提前完整展示威胁。

### `PROCEDURAL_MONTAGE`

加载 `references/procedural-montage.md` 与 `references/continuity-system.md`。先规划状态推进和曲线，再分配 Panel；每格遵守 Single-Moment Rule，必须能解释一个新的过程状态、因果结果或中断后的决策。

### 其他模式

读取现有 `storyboard-formats.md` 中对应的节奏模板，仍然遵守“每格需要电影功能”和连续性规则。没有专用 reference 时，不凭空发明复杂模式语法。

## 7. Continuity and Geography Activation

当任务包含动作、追逐、多人 Blocking、关键道具、复杂空间或多格状态变化时，加载：

- `references/scene-geography.md`：建立空间锚点、Camera Axis、运动路径和 Active Wide Reset。
- `references/continuity-system.md`：建立分级状态、Panel In/Out 和状态传播。

按复杂度选择状态级别：

```text
FULL    Action / chase / complex blocking / changing props or body state
LIGHT   Dialogue / Horror / Narrative multi-panel scene
MINIMAL Single cinematic keyframe or isolated composition
```

`PROCEDURAL_MONTAGE` 强制使用 `FULL`，并启用 `PROP_STATE_CONTINUITY` 与 `PROCESS_CONTINUITY`。

`continuity_in` 必须能解释本格从哪里来，`continuity_out` 必须能解释下一格从哪里接。不要让单张 `CINEMATIC_KEYFRAME` 被完整状态表绑架。

## 8. Annotation Profile Activation

当用户要求注释、任务需要动作/摄影机/构图方向，或输出是专业 Storyboard Sheet 时，加载 `references/annotation-system.md`。

Profile 选择遵循：用户明确指定 > 旧 Prompt 兼容要求 > 模式/复杂度推断 > 默认值。默认值为：普通任务 `ANNOTATION_SIMPLE`，Action/Dance/复杂 Blocking 使用 `ANNOTATION_PRO`，Cinematic Keyframe 使用 `ANNOTATION_CLEAN`，`ANNOTATION_LEGACY_BLUE` 只在明确兼容时使用。用户要求画面内零注释时使用 `CLEAN_EXTERNAL_METADATA`。

Annotation Profile 只控制注释层，不改变 storyboard 绘画的媒介、色彩和渲染风格；黑白 storyboard 仍保持黑白，彩色只用于注释时必须明确写出。

## 9. Single-Moment Rule、Style Layers 与曲线

每个 Panel 只描述一个可提取的视觉瞬间。禁止在同格中把“抓起、转身、插入、拉杆”等连续时间状态串成动作链，也禁止同一人物或道具出现 ghost pose / onion-skin。若一个描述有多个连续动作，拆成多个 Panel；在 Director Plan 阶段逐格检查。该规则适用于 Action、Dialogue、Horror、Procedural Montage 及其他模式。可读的双动作仍记录为 `WARNING`，不是自动 `BLOCKER`。

Style 分成两层：`FINAL_VISUAL_STYLE` 描述最终视频/成片风格，`STORYBOARD_DRAWING_STYLE` 描述 Panel 的绘制媒介和线稿风格。旧输入只有一个 `style` 时，为兼容性将其同时作为两层；用户分别指定时必须分离。Prompt Compiler 必须写明：`Final-video style reference does not apply to storyboard panel rendering`。

`STYLE_SWATCH_STRIP` 为可选 Output Profile，默认关闭；用户明确要求，或 Commercial、Sci-fi、MV、Animation 任务需要展示成片参考时才开启。它只放 2–3 个最终风格色板缩略图，不改变 Panel 绘制层。

`RHYTHM_CURVE` 描述剪辑/动作速度，`ESCALATION_CURVE` 描述压力/情绪强度；两者可以不同步。需要整体节奏的任务先规划 `SETUP → ACCELERATION → INTERRUPTION → RE-ACCELERATION → PEAK → DROP → FINAL_SPIKE`，再把 Panel 分配到阶段；Validator 检查是否有意图的整体波形，不要求每格越来越快。

## 10. Default Output Contract

除非用户指定其他 `OUTPUT_MODE`，完整输出固定为四段：

1. **Storyboard Creative Brief**：开头注明 Format、Mode、Panel 数量、布局、媒介、色彩规则和单张/分帧形式，再概括世界、情绪、主要运动、视觉母题、摄影机性格和结尾画面。
2. **Panel Table**：每行一个 Panel，列为 `Panel`、`Story beat`、`Visual action`、`Shot language`、`Motion and camera notes`、`Annotation notes`、`Continuity cue`。
3. **Consolidated Image Prompt**：将 Director Plan 编译为目标图像模型可执行的整合提示词。
4. **Negative Prompt and Validation Notes**：先列负面约束，再按 `references/storyboard-validator.md` 的模板列 `PASS`、`WARNING`、`NOTE` 或 `BLOCKER`，并注明 `Status` 与 `Evidence`。

用户明确要求 `PLAN_ONLY` 时，不生成整合图像 Prompt；用户明确要求图生视频或 `ANIMATIC` 时，才增加每格的运镜、主体运动、转场和大致时长。

## 11. Preflight Validation

返回前加载 `references/storyboard-validator.md` 执行内部自检。该文件是唯一严重级别来源：`BLOCKER` 只覆盖明确硬约束、模式错误、关键元素缺失、不可调和矛盾和必需输出缺失；专业质量建议降为 `WARNING` 或 `NOTE`。先修复所有 `BLOCKER`，再按模板输出 `Validation Notes`，不因审美偏好阻断交付。

参考图任务还必须执行 `Reference Validator`：确认每张图的 Role、Target、controls 和 must-not-control 可解释，Style Reference 没有覆盖身份、场景或道具事实；按统一 Validator 判断可修复的职责/优先级问题为 `WARNING`，不可调和的事实冲突才是 `BLOCKER`。

连续性任务还必须执行 `Geography Validator` 和 `Continuity Validator`：检查 Camera Axis、屏幕方向、空间锚点、Panel In/Out 及关键状态变化。完整规则见对应 references。

注释任务还必须执行 `Annotation Validator`：检查 Profile 是否唯一、颜色是否各司其职、Legacy 语义是否与默认 Profile 混用，以及注释颜色是否误改变画面媒介。Profile 不理想或颜色过多是 `WARNING`，语义互相冲突才是 `BLOCKER`。

`ACTION_PREVIS` 任务还必须执行 `Motion DNA Validator` 和 `Action Validator`：检查角色动作语言、Cause → Effect、动作阶段、空间后果和结尾结果。两者只在 Action 模式生效；动作质量不足通常为 `WARNING`/`NOTE`，只有触发关键缺失、明确禁用项或连续性/地理硬矛盾时才为 `BLOCKER`。

不要把“没有自动脚本验证”写成“已通过模型实测”。验证结果是当前 Director Plan 的推理检查，真实图像效果仍需 Golden Case 回归。
