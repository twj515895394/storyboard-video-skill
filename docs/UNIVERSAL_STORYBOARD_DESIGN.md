# Universal Storyboard Director Skill 设计方案

> 状态：Design Baseline / 设计基线  
> 日期：2026-08-23  
> 基于：`storyboard-video-previs` fork 进行演进  
> 目标：将当前的 Storyboard Prompt / Previs Generator 演进为一个可持续扩展、可调试、可验证的 **Universal Storyboard Director Skill**。

---

## 1. 背景与问题定义

当前 Skill 已经具备一条正确的核心链路：

```text
故事/主题/脚本
  ↓
Story Engine
  ↓
Storyboard Rhythm
  ↓
Shot Language
  ↓
Continuity
  ↓
Panel / Shot Plan
  ↓
Consolidated Image Prompt
  ↓
Image-to-Video Extension（可选）
```

这意味着它并不是简单的“把故事平均拆成 N 格”，而是在尝试设计观看体验：节奏、镜头语言、动作、连续性和最终视觉输出。

这个方向是正确的，因此本项目不计划推倒重来，而是以现有 Skill 为 V0 骨架继续扩展。

但如果目标是“全能故事板 Skill”，当前版本仍然存在几个结构性缺口：

1. 以单张 Storyboard Sheet 为中心，缺乏 Project → Sequence → Scene → Shot 的多层级组织。
2. Continuity 目前只是检查项，还不是可传递、可更新的状态系统。
3. Reference Image 只有“参考角色一致性”的概念，缺少明确的职责分配和优先级。
4. Action / Choreography 缺少角色自己的 Motion DNA、攻防因果和动作语法。
5. Annotation 颜色语义不够稳定，不同提示词可能把红/蓝箭头解释成不同东西。
6. 当前默认强调“每格都有可见动作”，这一规则不适用于对白、恐怖、情绪戏等。
7. 缺少 Shot Necessity、Redundancy、Continuity 等自动验证层。
8. 不同题材已经有模板，但还没有形成真正的 Mode Router 和模块化知识加载机制。
9. 输出更像高级 Prompt Generator，而不是“先做导演设计，再编译为不同模型可执行 Prompt”的系统。
10. 缺乏系统化开发、回归测试和效果调试方案。

因此，本次设计的核心不是“增加更多术语”，而是把 Skill 从 Prompt 模板升级为一个轻量的 **Storyboard Director System**。

---

## 2. 产品定位

### 2.1 一句话定位

**Universal Storyboard Director**：

> 将故事、脚本、创意、角色、场景与参考图，转换为具有导演意图、空间连续性、动作逻辑和可视化注释的故事板方案，并进一步编译为图像模型 / 视频模型可执行的提示词。

### 2.2 它不只是做什么

不是：

```text
输入故事 → 平均拆成 12 段 → 每段配一个镜头术语
```

而是：

```text
理解叙事意图
→ 判断适合的 Storyboard Mode
→ 识别角色/场景/参考图职责
→ 建立空间与连续性约束
→ 设计节拍和镜头存在的理由
→ 设计动作/表演/节奏
→ 选择合适的 Storyboard 表现方式
→ 生成可读 Storyboard Plan
→ 编译为目标图像模型 Prompt
→ 校验冗余、矛盾、断轴、角色漂移风险
```

### 2.3 目标用户场景

支持但不限于：

- 短片 / 微电影
- 动画 / 3D 动画
- 武打 / 战斗 / 追逐
- 体育 / 舞蹈 / 表演
- 对话戏 / 情绪戏
- 恐怖 / 悬疑
- 喜剧
- 商业广告 / 产品广告
- MV / 实验影像
- 短视频 / Reels / TikTok 式强 Hook 视频
- Cinematic Keyframe / Trailer Board
- Animatic / 图生视频预演
- 已有角色资产、场景资产基础上的镜头规划

---

## 3. 总体设计原则

### P1. Story First，Camera Second

先回答：

- 这一场戏发生了什么？
- 角色想要什么？
- 阻力是什么？
- 情绪如何变化？
- 最后观众应该记住什么？

再决定镜头。

镜头术语不能反过来替代叙事设计。

### P2. Every Panel Needs a Cinematic Function

废弃把“每格必须有明显动作”作为全局规则。

新的全局规则：

> **Every panel must create a new cinematic function.**

每个 Panel / Shot 至少承担一个明确功能：

- `GEOGRAPHY`：建立或重建空间关系
- `ACTION`：推进动作
- `REACTION`：表现反应
- `INFORMATION`：传递信息
- `EMOTION`：推进情绪
- `RHYTHM`：控制节奏
- `TRANSITION`：完成视觉/叙事转接
- `IMPACT`：强调撞击或结果
- `REVEAL`：揭示新信息
- `PAYOFF`：兑现前面的铺垫

Action 模式可以要求持续动作；Dialogue / Horror 则允许“静止、等待、停顿”成为有功能的镜头。

### P3. Cause → Effect > Cool Pose

动作、表演和剪辑首先要求因果可读，而不是单格漂亮。

```text
原因 → 动作发起 → 对方反应 → 接触/错过 → 空间变化 → 下一动作
```

任何动作镜头都应该尽量能解释：

- 为什么这样动？
- 从哪里来？
- 到哪里去？
- 对下一格造成什么影响？

### P4. Continuity Is State, Not Reminder

连续性不能只是最后一句“注意保持一致”。

需要把关键连续性变成显式状态，在 Panel 之间传递。

### P5. Reference Images Must Have Roles

多张参考图不能一锅炖。

必须先声明每张图控制什么，不控制什么。

### P6. One Clear Visual Idea Per Panel

每格优先一个主要视觉信息。

复杂动作通过连续 Panel 表现，而不是把一整个 2 秒动作过程都塞进一张图。

### P7. Modular Knowledge, Thin SKILL.md

`SKILL.md` 负责：

- 判断
- 路由
- 编排
- 输出契约

专业知识放入 `references/`。

避免最终出现一个数万字、每次都全量加载的巨型 `SKILL.md`。

### P8. Plan First, Prompt Second

内部概念上区分两层：

1. **Director Plan**：故事板导演设计
2. **Prompt Compiler**：将 Director Plan 编译成 GPT Image、其他图像模型、视频模型可执行提示词

这样未来更换模型时，不需要重做整个导演逻辑。

---

## 4. 核心层级模型

建议引入如下层级：

```text
PROJECT
  └── SEQUENCE
        └── SCENE
              └── SHOT
                    └── PANEL / KEYFRAME
```

### 4.1 Project

描述整个项目级别不轻易变化的内容：

- Project ID
- 标题
- 类型 / 风格
- 画幅
- 视觉世界观
- 角色资产
- 场景资产
- Style Packet
- Color Script
- 全局 Reference Map
- 全局禁止项

### 4.2 Sequence

一段连续的叙事单元，例如：

```text
SEQ_001 放学
SEQ_002 山路回家
SEQ_003 回到土屋
SEQ_004 淘米做饭
SEQ_005 炒白菜
SEQ_006 独自吃饭
```

一个 Sequence 可以对应一张 Storyboard Sheet，也可以拆为多个 Sheet。

### 4.3 Scene

描述具体时空场景：

- Location
- Time
- Weather
- Lighting
- Characters Present
- Scene Goal
- Emotional State
- Geography

### 4.4 Shot

真正意义上的电影镜头单位：

- Shot ID
- Cinematic Function
- Beat
- Subject Action
- Shot Size
- Angle
- Camera Movement
- Lens / Framing
- Screen Direction
- Duration（可选）
- Transition
- Continuity In
- Continuity Out

### 4.5 Panel / Keyframe

Storyboard Sheet 中的视觉关键帧。

注意：

> Shot 与 Panel 不强制 1:1。

对于简单故事板，可以 1 Shot = 1 Panel；对于复杂运镜或动作，一个 Shot 可以用多个 Panel 展示起始 / 过程 / 结束。

---

## 5. Storyboard Mode Router

Skill 首先判断任务属于哪种导演模式，而不是所有场景共用一个模板。

建议 V1 支持：

| Mode | 主要目标 | 主要约束 |
|---|---|---|
| `NARRATIVE` | 剧情推进 | 叙事清晰、节奏、视觉转折 |
| `DIALOGUE` | 表演与人物关系 | Eyeline、Reaction、Power Shift |
| `ACTION_PREVIS` | 战斗/追逐/体育 | Geography、Cause-Effect、Motion DNA |
| `HORROR_SUSPENSE` | 压力与未知 | Negative Space、Delay、Reveal Timing |
| `COMEDY` | 预期与反转 | Setup、Hold、Misdirection、Payoff |
| `COMMERCIAL` | 产品信息与品牌记忆 | Product Readability、Benefit、Hero Shot |
| `MV_PERFORMANCE` | 节奏与视觉表现 | Rhythm、Motif、Performance、Texture |
| `SHORT_FORM` | 短视频留存 | Hook、Pattern Interrupt、Compression、Loop |
| `CINEMATIC_KEYFRAME` | 高质量电影关键画面 | Composition、Lighting、Iconic Moment |
| `ANIMATIC` | 视频制作预演 | Duration、Motion、Transition、Coverage |
| `PROCEDURAL_MONTAGE` | 状态推进式过程 | Process Progress、State Change、Prop State、Cause-Effect |

允许混合模式，例如：

```text
PRIMARY_MODE: ACTION_PREVIS
SECONDARY_MODE: HORROR_SUSPENSE
```

但必须有 Primary，避免规则冲突。

`PROCEDURAL_MONTAGE` 使用六问适用性判断器；也可以作为其他 Primary Mode 的局部 Secondary Pattern，不覆盖主模式。

---

## 6. Story Engine / Creative Brief

在正式拆镜前，先生成一个短而可操作的 Story Engine。

建议字段：

```yaml
story_engine:
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

原则：

- 不做长篇文学分析。
- 每个字段都必须能影响镜头设计。
- 如果某项不重要，可以省略，不要为了填表制造内容。

---

## 7. Reference Role System

这是全能版必须新增的核心能力。

### 7.1 Reference 类型

建议支持：

```text
IDENTITY_REFERENCE
COSTUME_REFERENCE
HAIR_REFERENCE
BODY_REFERENCE
PROP_REFERENCE
LOCATION_REFERENCE
ARCHITECTURE_REFERENCE
STYLE_REFERENCE
LIGHTING_REFERENCE
COLOR_REFERENCE
COMPOSITION_REFERENCE
CAMERA_REFERENCE
MOTION_REFERENCE
```

### 7.2 Reference Map 示例

```yaml
references:
  - id: REF_001
    role: IDENTITY_REFERENCE
    target: CHAR_001
    controls:
      - face identity
      - age
      - skin/fur
      - facial geometry
    must_not_control:
      - costume
      - storyboard rendering style

  - id: REF_002
    role: COSTUME_REFERENCE
    target: CHAR_001
    controls:
      - clothing structure
      - materials
      - accessories

  - id: REF_003
    role: LOCATION_REFERENCE
    target: LOC_001

  - id: REF_004
    role: STYLE_REFERENCE
    target: GLOBAL
```

### 7.3 Priority

必要时支持：

```text
IDENTITY > COSTUME > STORYBOARD STYLE
```

重点不是固定一个永远正确的优先级，而是：

> Skill 必须让模型知道冲突时谁说了算。

---

## 8. Continuity State System

### 8.1 为什么需要状态

AI Storyboard 常见失败：

- 人物突然左右互换
- 攻击方向反转
- 道具凭空消失
- 手持物换手
- 伤痕 / 衣服状态回滚
- 光线方向漂移
- 场景结构每格重建
- 角色从一个位置瞬移到另一个位置

因此 Continuity 需要从“提醒”升级为“状态”。

### 8.2 Scene Geography

每个重要 Scene 先建立简化空间图：

```text
COURTYARD WEST                     COURTYARD EAST

Broken Pillar A    Whoking  ←→  Dao Wang    Steps
                         Clash Zone

Camera Base Axis  ───────────────────────────
```

不要求真的输出 ASCII 图，但 Agent 内部需要先明确：

- 谁在左 / 右
- 面朝方向
- 主要运动方向
- 关键环境锚点
- Camera Axis

### 8.3 Character State

示例：

```yaml
CHAR_001:
  position: courtyard_west
  facing: screen_right
  pose_state: airborne
  travel_direction: left_to_right
  left_hand: extended
  right_hand: retracted
  prop_state: none
  costume_state: intact
  damage_state: none
```

### 8.4 Continuity In / Out

每个 Panel 最少维护：

```yaml
continuity_in:
  screen_direction:
  position:
  prop_state:
  body_state:

continuity_out:
  screen_direction:
  position:
  prop_state:
  body_state:
```

不一定全部展示给用户，但应该参与生成和 Validator。

---

## 9. Character Motion DNA

Action、Dance、Performance 等模式必须新增。

### 9.1 目的

同一个角色不仅“长得一样”，还应该“动得像同一个人”。

### 9.2 数据结构建议

```yaml
motion_dna:
  locomotion:
  stance:
  attack_language:
  defense_language:
  rhythm:
  weight:
  acceleration:
  center_of_gravity:
  signature_moves:
  forbidden_motion:
```

### 9.3 示例概念

灵活猴拳角色：

```text
low crouch
spring-loaded launch
sudden direction change
hand slap
scrambling footwork
aerial redirect
low-to-high rhythm
```

蛇形角色：

```text
precise tracking
coiling evasion
narrow-line counter
minimal wasted motion
exact hand shapes
upright discipline
```

这样 Shot 设计时不是随机生成“漂亮动作”，而是从角色自己的动作语言中采样。

当前已将 Motion DNA 规则落地到 `references/character-motion-dna.md`，但仍需通过 Whoking Golden Case 的真实生成对照验证模型是否能稳定遵循。

---

## 10. Action Choreography Engine

`ACTION_PREVIS` 不能只使用一般 Story Beat。

建议把动作拆为：

```text
INITIATION
→ APPROACH
→ EVADE / BLOCK
→ CONTACT / MISS
→ REDIRECTION
→ RECOVERY
→ COUNTER
→ ESCALATION
```

每一格或每几个 Panel 形成清晰的 Cause → Effect。

### 10.1 Action Readability Rules

1. 先保证空间可读，再追求夸张透视。
2. 复杂动作使用 Wide / Full Shot 保持肢体关系。
3. Insert 只用于关键触点，不用于替代空间关系。
4. Close-up 之后必要时安排 Active Wide Reset。
5. Reset 不是静止对峙，而是重新说明地理关系的同时继续动作。
6. 避免重复 stare-down / pose / anticipation。
7. 每一次反击必须能找到前一个动作原因。
8. 结束帧应该是动作结果或强悬念，而不是随机 Hero Pose。

当前已将 Action Choreography 规则落地到 `references/action-choreography.md`；它只作用于 `ACTION_PREVIS`，不改变 Dialogue/Horror 的静止与停顿规则。

Whoking Golden Case 已完成三组第一轮生成对照：Original/Legacy、Universal + Legacy、Universal + PRO。结果与当前设计假设一致：`ANNOTATION_PRO` 的主体/摄影机/构图语义分离最清楚，但仍需 Dialogue/Horror 回归验证跨模式边界。

Dialogue/Horror 回归已完成：Dialogue 保留 Silence/Hold、Eyeline、录音笔连续性和 Power Shift；Horror 保留静止、Negative Space、False Calm 和延迟揭示。当前没有发现 Action 规则污染非动作模式；Dialogue 的单格子画面拆分问题留给后续 Prompt Compiler/Validator 处理。

---

## 11. Shot Design Model

建议每个 Shot / Panel 统一使用以下字段设计：

```yaml
panel: 01
function: ACTION
story_beat:
visual_action:
shot_size:
angle:
camera_movement:
lens_or_framing:
subject_motion:
screen_direction:
annotation:
continuity_cue:
transition:
```

其中必须区分：

- `camera_movement`
- `subject_motion`

这是后续箭头语义稳定的基础。

---

## 12. Annotation System

### 12.1 当前问题

实际收集到的优秀 Storyboard Prompt 中，可能出现：

```text
Red frame boxes = camera framing
Blue arrows = motion + attack + body travel + force + camera movement
```

而现有 Skill Reference 中采用的是另一套：

```text
Red = body / object movement
Blue = camera movement
Green = framing / crop
Orange = lighting
Purple = emotion / music
```

两者不能在系统内部混用，否则 Agent 和图像模型会产生语义歧义。

### 12.2 默认标准：ANNOTATION_PRO

建议正式确定：

| 颜色 | 语义 |
|---|---|
| RED | Subject / Object Motion、Attack、Force、Travel |
| BLUE | Camera Movement |
| GREEN | Framing、Crop、Reframe、Composition |
| ORANGE | Lighting、VFX Direction |
| PURPLE | Rhythm、Emotion、Music Hit |
| BLACK | Labels、Lens、Action Notes |

### 12.3 Profiles

#### `ANNOTATION_PRO`

完整专业模式，适合动作、追车、舞蹈、复杂 Blocking。

#### `ANNOTATION_SIMPLE`

```text
RED = subject motion
BLUE = camera motion
BLACK = notes
```

普通 Storyboard 默认可优先使用。

#### `ANNOTATION_CLEAN`

不画箭头，只保留 Panel Number / Shot Label。

适合 Cinematic Keyframe、广告美术板、情绪板。

#### `ANNOTATION_LEGACY_BLUE`

兼容用户收集到的旧式 / 特定 Prompt：

```text
RED BOX = framing
BLUE ARROW = motion / attack / camera / force
```

此模式只在用户明确指定或复现现有模板时使用，**不作为系统默认规范**。

当前已将四种 Profile 落地到 `references/annotation-system.md`，并明确其与 `shot-language.md` 通用镜头词汇的职责边界。

---

## 13. Storyboard Sheet Design

### 13.1 Sheet 不只是 Grid

一张专业 Storyboard Sheet 可包含：

```text
PROJECT CARD / MASTHEAD
CONTINUITY HEADER
STYLE / MODE TAGS
PANEL GRID
SHOT LABEL
MICRO ACTION CAPTION
ANNOTATION LEGEND（需要时）
```

### 13.2 Project Card

推荐字段：

```text
TITLE LOCKUP
META LINE
PRIORITY LINE
MICRO BRIEF
```

但不要把顶部做成沉重的后台表格；对于图像生成，应更像设计过的 typographic masthead。

### 13.3 Continuity Header

```text
SEQUENCE ID
PART
MODE
STYLE PACKET
REFERENCE PRIORITY
ANNOTATION PROFILE
```

### 13.4 Panel Number

Panel Number 优先放在 Frame 外，避免被模型画进角色或场景中。

### 13.5 Caption

每格仅使用很短的：

- Shot Label
- Micro Action

不要塞长句，Story Sheet 的首要目标仍是“扫一眼能读懂”。

---

## 14. Storyboard Layout Strategy

保留当前 6 / 8 / 12 / 16 / 24 Panel 基础，但不要只按题材硬编码。

建议未来根据：

```text
sequence duration
beat density
action complexity
continuity requirement
output readability
```

共同决定 Panel Count。

默认建议：

- 6：概念 / 极短动作
- 8：短场景 / Proof of Concept
- 12：均衡叙事
- 16：复杂动作 / 舞蹈 / 追逐
- 20：高密度 Action Sheet，可支持 5×4
- 24：Animatic / Detailed Previs

需要注意：20 Panel 应正式加入，因为优秀 Action Previs 案例已经证明 5×4 对 15 秒高密度动作非常实用。

---

## 15. Mode-specific Directing Rules

### 15.1 NARRATIVE

重点：

- Scene Goal
- Visual Turn
- Emotional Arc
- Motif
- Final Image

### 15.2 DIALOGUE

重点：

- Eyeline
- 180-degree rule / 有意识越轴
- Speaker / Listener Reaction
- Gesture Insert
- Power Shift
- Silence / Hold

不要为了镜头丰富而不停移动摄影机。

### 15.3 ACTION_PREVIS

重点：

- Combat Geography
- Motion DNA
- Cause → Effect
- Attack / Counter
- Active Wide Reset
- Impact Insert
- Escalation

### 15.4 HORROR_SUSPENSE

重点：

- Negative Space
- Offscreen Pressure
- Restricted Visibility
- Delay
- False Calm
- Reveal Timing

静止可以是强功能镜头。

### 15.5 COMEDY

新增模板：

```text
SETUP
EXPECTATION
REINFORCEMENT
MISDIRECTION
HOLD
REACTION
PAYOFF
AFTERMATH / TAG
```

喜剧中的 Hold 不应被 Validator 错判为“Dead Air”。

### 15.6 COMMERCIAL

重点：

- Product recognizability
- Macro tactile detail
- Usage
- Benefit visualization
- Before / After
- Product Hero

### 15.7 MV_PERFORMANCE

重点：

- Performance identity
- Rhythm
- Motif mutation
- Texture
- Camera energy
- Lyric / Beat accent

### 15.8 SHORT_FORM

新增模板：

```text
0-2s HOOK
PATTERN INTERRUPT
IMMEDIATE VALUE / CONFLICT
RAPID ESCALATION
PAYOFF
LOOP / REWATCH TRIGGER
```

### 15.9 CINEMATIC_KEYFRAME

弱化 Panel 连续动作，强化：

- iconic composition
- lighting
- scale
- emotion
- production design

### 15.10 ANIMATIC

在 Storyboard 基础上增加：

- approximate duration
- camera move
- subject move
- transition
- optional dialogue / sound cue

---

## 16. Prompt Compiler

### 16.1 为什么需要 Compiler 概念

Director Plan 应尽量模型无关。

最终 Prompt 根据目标模型进行适配：

```text
Director Plan
   ├── GPT Image Storyboard Sheet Prompt
   ├── Individual Frame Prompt
   ├── Image-to-Video Prompt
   └── Future Model Adapter
```

### 16.2 Storyboard Sheet Prompt Recommended Sections

推荐结构：

```text
[PROJECT CARD]
[CONTINUITY HEADER]
[REFERENCE MAP]
[SUBJECTS]
[SCENE]
[MOTION DNA]                # relevant modes only
[STORYBOARD FORMAT]
[VISUAL LANGUAGE]
[MODE DNA]
[SHOT DESIGN RULES]
[PANEL BEAT MAP]
[CONTINUITY RULES]
[ANNOTATION RULES]
[NEGATIVE]
```

不是所有模式都必须输出所有 Section，应由 Router 选择。

---

## 17. Validator 设计

最终 Skill 必须增加 Validator，而不是生成完 Prompt 就结束。

### 17.1 Shot Necessity Validator

检查：

> WHY DOES THIS SHOT EXIST?

如果连续多个 Panel 的 Function、Action、Camera、Information 几乎相同，应压缩或重设计。

### 17.2 Redundancy Validator

发现：

- repeated stare-down
- repeated hero pose
- repeated same shot size
- repeated same information
- repeated static beat（Action 模式）

### 17.3 Continuity Validator

检查：

- screen direction
- facing
- position
- prop
- costume state
- damage state
- environment anchor
- light direction

### 17.4 Geography Validator

尤其 Action：

- 观众能否知道 A/B 在哪里？
- Close-up 太多后是否失去空间？
- 是否需要 Active Wide Reset？

### 17.5 Reference Validator

- 每个关键角色是否有正确 Reference Role？
- 是否让 Style Reference 覆盖了 Identity？
- 是否存在 Reference 职责冲突？

### 17.6 Prompt Contradiction Validator

例如：

```text
rough storyboard
vs
highly polished final illustration
```

```text
monochrome drawing
vs
full color cinematic render
```

必须在发送给图像模型前消除冲突。

### 17.7 Mode Validator

Action 的“无 Dead Air”不能错误应用到 Horror / Dialogue / Comedy。

规则必须经过 Mode Scope。

---

## 18. 推荐的目标目录结构

当前先不立即重构，设计目标如下：

```text
storyboard-video-skill/
│
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
│
├── docs/
│   ├── UNIVERSAL_STORYBOARD_DESIGN.md
│   └── examples/
│       └── ACTION_PREVIS_WHOKING_VS_DAO_WANG.md
│
├── references/
│   ├── storyboard-core.md
│   ├── shot-language.md
│   ├── storyboard-formats.md
│   ├── continuity-system.md
│   ├── reference-image-system.md
│   ├── annotation-system.md
│   ├── scene-geography.md
│   ├── character-motion-dna.md
│   ├── action-choreography.md
│   ├── dialogue-direction.md
│   ├── horror-suspense.md
│   ├── comedy-timing.md
│   ├── commercial-product.md
│   ├── mv-performance.md
│   ├── short-form-video.md
│   ├── prompt-builder.md
│   └── storyboard-validator.md
│
└── templates/                 # 是否真的需要目录，在实现阶段再决定
    ├── narrative.md
    ├── action-previs.md
    ├── dialogue.md
    ├── commercial.md
    └── animatic.md
```

注意：

`templates/` 不是当前必须项。优先验证 `SKILL.md + references/` 能否稳定工作，再决定是否引入，避免过度工程化。

---

## 19. SKILL.md 未来职责

未来 `SKILL.md` 应保持相对短小，主要负责如下流程：

```text
1. Parse request
2. Determine production target
3. Determine PRIMARY / SECONDARY MODE
4. Build Reference Map
5. Extract Story Engine
6. Build Scene Geography / Continuity State when needed
7. Load mode-specific references
8. Design Beat Map
9. Design Shots / Panels
10. Select Annotation Profile
11. Compile requested output
12. Run Validators
13. Return Plan + Prompt + optional motion extension
```

不应该继续把：

- 所有镜头术语
- 所有动作规则
- 所有恐怖片规则
- 所有广告规则

全部塞进 `SKILL.md`。

---

## 20. 输出契约设计

### 20.1 默认完整输出

建议 V1 默认：

1. `Format / Production Target`
2. `Storyboard Creative Brief`
3. `Reference Map`（有参考图时）
4. `Continuity / Geography Notes`（需要时）
5. `Panel Table`
6. `Consolidated Generation Prompt`
7. `Negative Prompt`
8. `Validation Notes`
9. `Image-to-Video Extension`（用户要求时）

### 20.2 用户可指定输出级别

未来可支持：

```text
OUTPUT_MODE: PLAN_ONLY
OUTPUT_MODE: SHEET_PROMPT
OUTPUT_MODE: FULL
OUTPUT_MODE: FRAME_PROMPTS
OUTPUT_MODE: ANIMATIC
```

这样 Skill 可以同时适配“只想快速出图”和“专业前期规划”。

---

## 21. 开发阶段规划

### Phase 0 — Baseline Freeze

目标：保留当前行为作为对照组。

工作：

- 保留现有 SKILL.md / references
- 冻结典型测试输入和生产目标
- 建立三组首批测试样例集合：Action、Dialogue、Horror
- 统一硬约束、人工评分维度和回归规则
- 在真实 Skill 调用后再补充模型输出，不把未执行结果写成基线事实

Phase 0 基线资产位于 `tests/golden-cases/`。当前仍未修改运行逻辑，也未引入自动化 Validator。

### Phase 1 — Core Architecture

优先新增：

1. `storyboard-core.md`
2. Mode Router
3. Shot Cinematic Function
4. 新全局原则：Every Panel Needs a Cinematic Function
5. Output Contract

这一步先解决“系统如何思考”。

### Phase 2 — Continuity + Reference System

新增：

1. `continuity-system.md`
2. `scene-geography.md`
3. `reference-image-system.md`

这一步解决 AI Storyboard 最常见的一致性问题。

Reference Role System 已落地为 `references/reference-image-system.md`；本阶段随后补齐 `references/continuity-system.md` 与 `references/scene-geography.md`，并采用 `FULL / LIGHT / MINIMAL` 三档状态策略。

### Phase 3 — Annotation System

新增：

- PRO / SIMPLE / CLEAN / LEGACY_BLUE

统一红蓝箭头语义。

### Phase 4 — ACTION_PREVIS Deep Module

优先拿 Whoking vs Dao Wang 案例做回归测试。

新增：

- Motion DNA
- Action Cause → Effect
- Active Wide Reset
- Choreography Rules
- 20 Panel 5×4 Layout

### Phase 5 — Dialogue / Horror / Comedy

验证系统没有被“Action 思维”绑架。

重点测试：

- Silence 是否被保留
- Horror Stillness 是否被正确理解
- Comedy Hold 是否不被删掉

### Phase 6 — Commercial / MV / Short-form

扩展商业和平台内容。

### Phase 7 — Validator

把之前靠 Prompt 重复强调的问题变成系统检查。

### Phase 8 — Prompt Compiler Optimization

根据实际模型测试结果优化：

- GPT Image Storyboard Sheet
- 单帧 Prompt
- 图生视频 Prompt

---

## 22. 测试策略

Skill 开发必须“边开发边调效果”，不能只检查 Markdown 是否写得漂亮。

### 22.1 Golden Cases

长期目标至少维护以下 Golden Test：

1. 双人格斗：Whoking vs Dao Wang
2. 中国 80 年代山村男孩放学做饭
3. 两人室内对白 / 权力反转
4. 恐怖走廊 / 看不见的威胁
5. 产品广告
6. 舞蹈 / MV
7. 10 秒强 Hook 短视频

Phase 0 先冻结前三类核心风险案例：

- `ACTION_PREVIS`：Whoking vs Dao Wang
- `DIALOGUE`：室内对白 / 权力反转
- `HORROR_SUSPENSE`：恐怖走廊 / 看不见的威胁
- `PROCEDURAL_MONTAGE / ASSEMBLY`：Teleport Device Assembly（V2 Golden Case B）

具体输入、硬约束和评分口径见 `tests/golden-cases/`。

### 22.2 每个 Case 测什么

不是只看“图好不好看”，而是分项打分：

```text
Story Clarity
Shot Variety
Shot Necessity
Character Consistency
Spatial Continuity
Action Readability
Reference Fidelity
Annotation Readability
Prompt Contradiction
Final Panel Strength
```

### 22.3 Action 特殊指标

```text
Combat Geography
Cause-Effect Readability
Motion DNA Fidelity
Repeated Pose Count
Dead Air Count
Active Wide Reset Quality
```

### 22.4 回归原则

每次修改一个 Reference 或 SKILL.md，都至少回跑：

- 1 个 Action
- 1 个 Dialogue / Emotion
- 1 个 Horror / Stillness

避免为了动作效果优化，把其他题材做坏。

---

## 23. 关键设计决策记录（ADR-style）

### D001 — 不重写，基于现有 Skill 演进

原因：现有 Story Engine → Rhythm → Shot → Continuity 主链方向正确。

### D002 — “可见动作”降级为 Mode-specific Rule

原因：对白、恐怖、喜剧中静止具有叙事功能。

全局改为：

> Every panel must create a new cinematic function.

### D003 — 红=角色动作，蓝=摄影机运动

默认标准采用语义分离方案。

原因：一类箭头同时表达人物、攻击和 Camera 会产生歧义。

### D004 — 保留 LEGACY_BLUE Profile

原因：优秀现成 Prompt 可能依赖“红框+蓝色综合箭头”的视觉习惯，需要兼容和复现。

### D005 — Reference Image 强制角色化

原因：多图输入时必须明确谁控制身份、服装、场景、风格，否则一致性不可控。

### D006 — Continuity 作为 State

原因：仅在 Negative Prompt 中写“保持一致”无法可靠解决左右、道具和位置漂移。

### D007 — Motion DNA 独立于 Character Identity

原因：角色一致性不仅是脸和服装，还包括动作风格。

### D008 — Director Plan 与 Prompt Compiler 分层

原因：未来图像模型 / 视频模型变化时，导演逻辑应可复用。

### D009 — 暂不引入复杂代码/Schema 引擎

当前 Skill 仍以 Markdown Prompt Engineering 为核心。

先通过真实生成效果证明结构，再考虑 JSON Schema、脚本 Validator 或自动工具链。

---

## 24. 本阶段明确不做的事情

本轮建立设计基线和 V0 Golden Case 规范，但不立即：

- 重写 `SKILL.md`
- 重写现有 `shot-language.md` 的镜头词汇（本阶段仅补充其与 Annotation Profile 的职责说明）
- 修改现有 `storyboard-formats.md`
- 新建全部 references
- 引入代码执行器
- 强制 JSON 输出
- 强制所有任务使用复杂 Continuity State

当前已完成的 Phase 0 资产不代表真实模型效果已经验证；模型输出采集和评分将在实际调用 Skill 后进行。

这些将在后续按 Phase 分步实现并测试。

---

## 25. 预期最终形态

理想的 Universal Storyboard Skill 应该像一个小型导演团队：

```text
Story Analyst
     ↓
Mode Director
     ↓
Continuity Supervisor
     ↓
Action / Performance Designer
     ↓
Cinematographer
     ↓
Storyboard Designer
     ↓
Prompt Compiler
     ↓
Validator
```

但实现上仍然保持为一个清晰、轻量、可维护的 Skill，而不是堆叠多个互相重复的 Agent。

最终目标不是“生成更多描述”，而是：

> **让用户给出一个故事、脚本、参考图或粗略创意后，Skill 能像导演一样先做正确的镜头决策，再像 Storyboard Artist 一样组织画面，最后像 Prompt Engineer 一样将方案稳定地交给图像 / 视频模型执行。**

---

## 26. 下一步建议

Phase 0 已完成输入与验收协议冻结，Core Architecture、Reference Role、Continuity/Geography、Annotation Profiles 和 Action Previs 规则层已分阶段落地，后续开发建议严格按以下顺序：

```text
Step 1: Validator
Step 2: 继续扩展商业、MV、短视频
```

本文件作为后续开发的设计基线。实际实现中如果需要修改关键原则，应同步更新本文件中的“关键设计决策记录”，避免代码 / Skill 行为和设计文档长期漂移。
