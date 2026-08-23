# Universal Storyboard Director — Supplement 01

## Procedural Montage、状态连续性与多轴描述补充设计

> 文档性质：**补充设计文档（Supplement）**  
> 适用仓库：`twj515895394/storyboard-video-skill`  
> 与主设计文档关系：**只补充，不替换、不重写、不要求回滚现有实现**。  
> 主设计文档 `docs/UNIVERSAL_STORYBOARD_DESIGN.md` 继续作为当前实现基线。  
> 本文记录由第二类 Golden Case —— **TELEPORT DEVICE ASSEMBLY** —— 引出的新发现，并给出可渐进接入现有实现的扩展方案。

---

# 1. 为什么需要这份补充文档

第一版设计主要围绕“通用故事板导演系统”建立了 Story Engine、Mode Router、Continuity、Reference Role、Motion DNA、Annotation Profile、Prompt Compiler、Validator 等核心能力。

在分析 **Whoking vs Dao Wang** 案例时，`ACTION_PREVIS / ACTION_CHOREOGRAPHY` 类型已经得到较充分验证：动作由人物之间的攻防、空间关系、身体轨迹和动作因果驱动。

但新的 **TELEPORT DEVICE ASSEMBLY** 案例暴露出另一种非常重要、且不能简单归入 Action 的故事板逻辑：

> 镜头很多、动作很快、剪辑密度很高，但真正推动序列前进的不是“角色对抗”，而是一个目标对象从初始状态逐步转变为完成状态。

该案例的核心变化链为：

```text
broken
→ scattered
→ core loose
→ core set
→ base aligned
→ cable live
→ lens set
→ clamp fixed
→ bolt locked
→ ring seated
→ antenna up
→ battery in
→ dial armed
→ tube sealed
→ screen awake
→ lever down
→ unstable
→ assembled
→ portal forming
→ unresolved
```

因此，它属于一种独立的 Storyboard Engine：

> **State-transition-driven Storyboard**

本文将这种一级逻辑命名为：

```text
PROCEDURAL_MONTAGE
```

中文可理解为：

> **程序性过程蒙太奇 / 操作过程蒙太奇**

`ASSEMBLY` 只是它的一个 subtype，而不是全部。

---

# 2. 本补充文档的兼容原则

由于主设计文档已经进入实现阶段，本补充设计遵守以下约束：

1. **不要求修改主设计文档。**
2. **不否定已经实现的 Storyboard Mode Router。**
3. **新增能力优先采用可选字段、扩展 Mode、附加 Validator 的方式接入。**
4. **避免为了理论上的“更优架构”立即重构已经完成的代码。**
5. 只有当实际 Golden Case 测试证明当前结构无法表达时，再考虑下一版本的内部模型升级。
6. 本文提出的“多轴描述”可以先作为内部 metadata 使用，不要求立即替换当前 `mode` 字段。

也就是说：

> **当前实现继续往前走，本补充作为后续扩展规则进入开发队列。**

---

# 3. 新增一级类型：PROCEDURAL_MONTAGE

## 3.1 定义

`PROCEDURAL_MONTAGE` 描述的是：

> 通过一系列顺序明确、具有前后依赖的操作，使某个目标对象、系统、身体状态、工作结果或环境状态发生可读的阶段性变化。

它与 Action 的本质区别：

### ACTION_CHOREOGRAPHY

主要关注：

```text
角色 A 动作
→ 角色 B 响应
→ 空间变化
→ 反击 / 躲避 / 压制
→ 新动作条件
```

核心连续性：

- Body Position
- Screen Direction
- Attack / Counter Line
- Spatial Geography
- Motion Continuity

### PROCEDURAL_MONTAGE

主要关注：

```text
当前状态
→ 操作
→ 状态变化
→ 下一操作条件成立
→ 下一状态
```

核心连续性：

- Prop State
- Assembly State
- Tool State
- Process Dependency
- Before / After Logic

因此二者可以都很“动态”，但底层 Storyboard Engine 完全不同。

---

# 4. PROCEDURAL_MONTAGE 的建议 Subtype

建议后续逐步支持：

```text
PROCEDURAL_MONTAGE
├── ASSEMBLY
├── REPAIR
├── CRAFT
├── COOKING
├── PREPARATION
├── TRANSFORMATION
├── ACTIVATION
├── EXPERIMENT
├── CONSTRUCTION
├── MAINTENANCE
└── WORKFLOW
```

说明：

### ASSEMBLY
零件逐步组合成完整对象。

### REPAIR
损坏状态逐步恢复为可用状态。

### CRAFT
原材料经过操作转变为成品。

### COOKING
食材经过步骤形成完成菜品，强调食材状态和容器状态。

### PREPARATION
人物为某项任务准备装备、服装、工具或环境。

### TRANSFORMATION
对象从一种形态逐步转变为另一种形态。

### ACTIVATION
设备已经基本完整，但需要逐步启动、通电、解锁、校准。

### EXPERIMENT
实验器材、样品、参数和反应状态逐步推进。

### CONSTRUCTION
结构、场景、机械或设施逐步搭建。

### MAINTENANCE
拆卸、检查、更换、重新安装、测试。

### WORKFLOW
更抽象的工作过程，可用于打印、冲洗、包装、整理、调试等。

---

# 5. 不把“快节奏”误判为 Action

TELEPORT DEVICE 案例非常适合建立一条分类规则：

> **动作强度、剪辑速度和 Storyboard 类型不是同一个维度。**

一个非 Action 场景完全可以拥有：

- whip pan
- crash-in
- fast inserts
- snap rhythm
- high cut density
- rising escalation
- impact beats

因此不应该出现：

```text
Action = Fast
Non-action = Slow
```

这样的隐式规则。

建议增加可选的 Energy / Pacing 描述。

---

# 6. 多轴描述模型：作为补充 metadata，而非立即替换 Mode Router

主设计中现有 `Storyboard Mode` 可以继续使用。

本文建议在内部逐步增加以下可选维度：

```text
SEQUENCE_LOGIC
ENERGY_PROFILE
OUTPUT_PROFILE
ANNOTATION_PROFILE
CONTINUITY_PRIORITY
```

它们不是必须一次全部实现。

## 6.1 SEQUENCE_LOGIC

描述：

> 这一组镜头主要靠什么逻辑推进。

建议候选值：

```text
NARRATIVE_DRAMA
DIALOGUE_PERFORMANCE
ACTION_CHOREOGRAPHY
PROCEDURAL_MONTAGE
CHASE_TRAVEL
HORROR_SUSPENSE
COMEDY_TIMING
PRODUCT_DEMO
MV_PERFORMANCE
EXPERIMENTAL_VISUAL
```

注意：这不是要求立即废弃原有 Mode Enum。

如果当前实现已经有：

```text
ACTION_PREVIS
DIALOGUE
HORROR
COMMERCIAL
...
```

可以先仅增加：

```text
PROCEDURAL_MONTAGE
```

等后续测试积累后再决定是否统一命名。

## 6.2 ENERGY_PROFILE

描述序列的运动能量和切镜速度。

建议初版：

```text
LOW
MEDIUM
HIGH
```

可选细化：

```text
SLOW_BURN
CONTROLLED
STEADY
RAPID_STACCATO
FLOWING_HIGH_ENERGY
CHAOTIC
PULSE_BASED
```

Teleport 示例：

```text
ENERGY_PROFILE: HIGH
PACING: RAPID_STACCATO
CUT_DENSITY: VERY_HIGH
```

## 6.3 OUTPUT_PROFILE

描述产物而不是故事类型。

例如：

```text
STORYBOARD_SHEET
ANIMATIC_SHEET
SEPARATE_FRAMES
SHOT_LIST
KEYFRAME_BOARD
IMAGE_GENERATION_PROMPT
VIDEO_PREVIS_PROMPT
```

这里特别需要记录一个设计判断：

> `ANIMATIC` 更适合作为 Output / Production Level，而不一定适合作为 Story Genre。

Action 可以是 Animatic，Dialogue 可以是 Animatic，Procedural Montage 同样可以是 Animatic。

但为兼容当前实现，不要求马上移动已有字段。

## 6.4 ANNOTATION_PROFILE

沿用主设计中的 Annotation Profile 思路。

Teleport 案例更接近：

```text
CLEAN_EXTERNAL_METADATA
```

特点：

- Panel 内不画箭头
- 不写标签
- 不写摄影机注释
- 不写动作字幕
- 所有生产信息放在 panel artwork 外部

## 6.5 CONTINUITY_PRIORITY

不同类型对 Continuity 的优先级不同。

Teleport：

```text
PROP_STATE
> PROCESS_DEPENDENCY
> CHARACTER
> SPATIAL
```

Whoking vs Dao Wang：

```text
SPATIAL
> BODY_ACTION
> SCREEN_DIRECTION
> CHARACTER
```

该字段初期甚至可以不输出给用户，只用于内部 Validator 和 Prompt Builder。

---

# 7. Continuity 扩展：PROP_STATE_CONTINUITY

主设计已有 Continuity System，本补充新增一个必须重视的领域：

```text
PROP_STATE_CONTINUITY
```

## 7.1 为什么它重要

AI 图像模型很容易发生：

- P07 已经装上的镜片在 P10 又掉回地上
- P11 已经竖起的天线在 P14 又恢复折叠
- P14 已插入电池，P18 却又没有电池
- 装置尺寸、连接关系、零件位置随镜头变化
- 已经发生的损坏 / 修复状态被模型“重置”

因此 Procedural Montage 必须把 Prop 当成具有状态的实体，而不是普通视觉元素。

---

# 8. 建议的 Continuity Domains 补充列表

在现有系统基础上，可逐步形成：

```text
CHARACTER_CONTINUITY
COSTUME_CONTINUITY
SPATIAL_CONTINUITY
SCREEN_DIRECTION_CONTINUITY
PROP_CONTINUITY
PROP_STATE_CONTINUITY
ENVIRONMENT_CONTINUITY
LIGHTING_CONTINUITY
EFFECT_CONTINUITY
TEMPORAL_CONTINUITY
PROCESS_CONTINUITY
```

新增重点：

### PROP_CONTINUITY

保证对象本身身份、外观、尺寸、材质、设计语言一致。

### PROP_STATE_CONTINUITY

保证对象“当前处于什么阶段”一致。

### PROCESS_CONTINUITY

保证步骤之间依赖关系合理：

```text
A 未完成
→ 不允许出现依赖 A 的 C
```

例如：

```text
core not seated
→ cannot seal final tube
```

---

# 9. State Before / State After

对于 Procedural Montage，一个 Panel 最好不仅描述：

```text
ACTION: installs battery
```

而是内部明确：

```text
STATE_BEFORE:
battery = detached

ACTION:
scientist slams battery cell into slot

STATE_AFTER:
battery = installed
```

这会显著提高：

- 镜头因果可读性
- Prompt 一致性
- Validator 可检查性
- 后续视频生成扩展能力

---

# 10. Teleport Beacon 示例状态机

可以内部表达为：

```text
TELEPORT_BEACON

P01
assembly_state: broken
core: detached
base: misaligned
cable: disconnected
lens: detached
clamp: open
bolt: loose
ring: detached
antenna: down
battery: absent
tube: open
portal: off

P04
core: installed

P05
base: aligned

P06
cable: connected

P07
lens: installed

P08
clamp: fixed

P09
bolt: locked

P10
ring: seated

P11
antenna: raised

P14
battery: installed

P15
dial: armed

P17
tube: sealed

P18
screen: active

P19
lever: engaged

P20
power_state: unstable

P21
assembly_state: assembled

P22
portal: forming

P23
portal_reflection: high

P24
portal: active_unstable
sequence_state: unresolved
```

实现时不一定需要真的保存所有字段。

重点是 Skill 在规划阶段必须知道：

> **哪些状态已经不可逆地发生变化，并且后续镜头必须继承。**

---

# 11. 新原则：ONE PANEL = ONE EXTRACTABLE MOMENT

Teleport Prompt 中有一个非常值得固化的规则：

```text
one clear pose per character per panel
```

并明确禁止：

```text
ghost poses
duplicate silhouettes
onion-skin bodies
```

建议将其抽象为通用规则：

> **SINGLE-MOMENT RULE**

标准表述：

```text
One panel represents one visually extractable moment,
not an entire multi-step action sentence.
```

例如，不推荐：

```text
She grabs the battery, turns around, inserts it,
then pulls the lever.
```

应拆成：

```text
P14 — Battery Slam
P15 — Dial Twist
P19 — Lever Hit
```

这不仅适用于 Procedural Montage，同样适用于：

- Action
- Dialogue reaction
- Comedy timing
- Horror reveal
- Product demo

---

# 12. Shot Metadata Schema 补充

Teleport 案例使用：

```text
BEAT
CAMERA
ACTION
RHYTHM
ESCALATION
STATE
STYLE
```

这是一套非常成熟的 Storyboard Metadata 思路。

建议未来内部逐步扩展为：

```text
SHOT_ID
BEAT
PURPOSE
CAMERA
SUBJECT_ACTION
ENV_ACTION
RHYTHM
ESCALATION
STATE_BEFORE
STATE_AFTER
CONTINUITY
TRANSITION
STYLE
ANNOTATION
```

## 字段说明

### SHOT_ID
唯一镜头标识。

### BEAT
当前镜头的最小叙事节拍。

### PURPOSE
为什么这个镜头存在。

可使用主设计中的功能词：

```text
GEOGRAPHY
ACTION
REACTION
INFORMATION
EMOTION
RHYTHM
TRANSITION
IMPACT
REVEAL
PAYOFF
PROCESS_PROGRESS
STATE_CHANGE
```

其中本文建议新增：

```text
PROCESS_PROGRESS
STATE_CHANGE
```

### CAMERA
景别、角度、运动、镜头行为。

### SUBJECT_ACTION
人物、动物或主体行为。

### ENV_ACTION
机器、烟雾、光、液体、纸张、环境等动态。

### RHYTHM
当前 shot 的速度属性。

### ESCALATION
当前镜头在整个序列中的强度位置。

### STATE_BEFORE
镜头前关键对象状态。

### STATE_AFTER
镜头后关键对象状态。

### CONTINUITY
必须继承的视觉/空间/状态条件。

### TRANSITION
进入或离开该镜头的剪辑/运动关系。

### STYLE
当前 Storyboard Representation 要求。

### ANNOTATION
当前 Panel 使用的 Annotation Profile。

---

# 13. Storyboard IR：长期方向，不要求本阶段立即实现

Teleport Case 说明，如果 Skill 长期需要同时支持：

- Sheet Prompt
- 单镜 Prompt
- Animatic
- 红蓝箭头 Storyboard
- Clean Storyboard
- GPT Image
- Seedance / Video Prompt
- 中英文分镜表

那么内部最好不要直接“拼最终英文 Prompt”。

长期建议存在一层中间结构：

```text
Storyboard IR
```

示例：

```yaml
shot_id: P14
beat: battery
purpose: process_progress
camera:
  size: insert
  angle: neutral
  movement: static
subject_action:
  actor: scientist
  action: slams battery cell into front slot
state_before:
  battery: detached
state_after:
  battery: installed
rhythm: fast
escalation: peak
continuity:
  ring: seated
  antenna: up
  core: installed
representation:
  style: graphite
annotation:
  profile: clean_external_metadata
```

然后由不同 Compiler 输出：

```text
Storyboard Sheet Prompt
Per-shot Prompt
Animatic Plan
Video Prompt
Annotation Overlay Instructions
```

再次强调：

> **Storyboard IR 是长期演进方向，不应为了实现本文而阻塞当前主设计开发。**

如果当前代码已使用结构化对象，只需逐步靠近即可。

---

# 14. Rhythm Curve：从逐镜节奏升级为序列节奏

Teleport Case 的 RHYTHM 不是 24 个互不关联的标签，而是有完整波形：

```text
hold
→ burst
→ fast
→ snap
→ fast
→ burst
→ fast
→ snap
→ fast
→ rise
→ fast
→ impact
→ burst
→ fast
→ snap
→ fast
→ rise
→ pause
→ hit
→ surge
→ peak
→ drop
→ final spike
→ unresolved
```

建议补充概念：

```text
RHYTHM_CURVE
```

也就是说，Skill 在设计 Panel 前最好先决定：

```text
SETUP
ACCELERATION
INTERRUPTION
RE-ACCELERATION
PEAK
DROP
FINAL_SPIKE
```

然后再把具体 shot 分配进去。

这比每一格独立决定 `fast / slow` 更稳定。

---

# 15. Escalation Curve 与 Rhythm 不完全相同

需要区分：

### RHYTHM

剪辑和动作速度。

### ESCALATION

剧情压力 / 风险 / 情绪强度。

例如 P18：

```text
RHYTHM: pause
ESCALATION: drop
```

这种短暂下降非常重要，因为它给 P19-P24 的再次冲高留出空间。

因此 Validator 不应该要求：

```text
每格越来越快
```

而应该检查：

> 是否存在有意图的整体曲线。

---

# 16. 双层 Style 系统

Teleport Prompt 非常明确地区分两种 Style：

## 16.1 FINAL_RENDER_STYLE

最终视频目标风格：

```text
retro anime sci-fi
cream/red graphic backdrop
cyan energy
clean cel edges
subtle film grain
```

## 16.2 STORYBOARD_REPRESENTATION_STYLE

故事板自身表现：

```text
colorless rough sketch
light graphite-gray lines
no shading
no color fills
no rendered lighting
simple lab shapes
```

这两个层级必须避免互相污染。

建议内部概念：

```text
FINAL_VISUAL_STYLE
STORYBOARD_DRAWING_STYLE
```

或：

```text
STYLE_LAYER_1 = FINAL_RENDER_TARGET
STYLE_LAYER_2 = STORYBOARD_REPRESENTATION
```

## 16.3 为什么重要

如果 Prompt 同时给模型：

```text
retro anime
cyan glow
cream red
film grain
```

又给：

```text
graphite storyboard
```

图像模型很容易把 Panel 直接画成彩色成稿。

因此 Compiler 应使用明确措辞：

```text
The final-video style reference does not apply to storyboard panel rendering.
Storyboard panels remain monochrome rough graphite only.
```

---

# 17. Style Swatch 作为独立区域

Teleport Case 使用：

> 3 tiny top swatches only for final render look

这是一个很好的 Sheet Design 方法：

- Panel 保持纯 Storyboard
- Style Swatch 单独告诉制作人员 / 模型最终视频的 Look

可以作为 Output Profile 的可选能力：

```text
STYLE_SWATCH_STRIP: enabled
```

但不应成为所有 Storyboard 默认要求。

适合：

- Commercial
- Sci-fi
- Animation
- MV
- Branded content
- Look development

---

# 18. Annotation 新 Profile：CLEAN_EXTERNAL_METADATA

主设计已有 `ANNOTATION_PRO`、`ANNOTATION_SIMPLE`、`ANNOTATION_CLEAN`、兼容 Legacy 等思路。

Teleport Case 可以进一步定义：

```text
ANNOTATION_PROFILE: CLEAN_EXTERNAL_METADATA
```

## Panel Artwork 内

禁止：

- arrows
- camera notes
- labels
- subtitles
- captions
- technical marks
- readable device text

## Panel Artwork 外

允许：

- panel number
- header chip
- BEAT
- CAMERA
- ACTION
- RHYTHM
- ESCALATION
- STATE
- STYLE

这意味着：

> Annotation 并不是“有没有生产信息”，而是“生产信息放在哪里”。

该区分值得保留。

---

# 19. Panel Artwork 与 Production Metadata 分层

建议未来 Storyboard Sheet 概念上分为：

```text
SHEET
├── VISUAL ARTWORK LAYER
└── PRODUCTION METADATA LAYER
```

### VISUAL ARTWORK LAYER

只负责：

- 构图
- 人物姿态
- 道具状态
- 环境
- 可视动作瞬间

### PRODUCTION METADATA LAYER

负责：

- Shot ID
- Camera
- Beat
- Action Note
- Rhythm
- State
- Continuity
- Technical note

这会使 Prompt 更稳定，也方便未来导出不同格式。

---

# 20. Procedural Montage 的导演规则

建议未来 `references/procedural-montage.md` 或等价模块至少包含以下规则。

## 20.1 Show Progress, Not Repetition

每一个镜头必须造成：

- 新状态
- 新风险
- 新信息
- 新步骤
- 新节奏变化

至少一种。

不能只是连续拍“手在工作”。

## 20.2 Before / During / After Readability

关键步骤要让观众看得出：

```text
之前是什么
正在做什么
做完之后变成什么
```

不要求每一步都用 3 镜头，但整个序列必须可推断。

## 20.3 Tool / Part Recognizability

如果某零件控制叙事，装配前后都应可辨认。

## 20.4 Macro Inserts for Tactile Progress

适合使用：

- insert
- macro
- ECU
- overhead
- side-track
- low insert

强化“动作完成感”。

## 20.5 Wide Reset 只在需要重新解释整体状态时使用

Procedural Montage 不需要频繁 Wide。

Wide 的功能主要是：

- 展示零件总量
- 展示设备完成度
- 展示环境风险
- 展示最终结果

## 20.6 Interruptions Create Drama

纯步骤很容易变成教程。

因此需要：

```text
process
→ interruption
→ reaction
→ continue under higher pressure
```

Teleport 示例中：

```text
P12 spark jump
P13 instinct dodge
```

就是一个很好的 interruption beat。

## 20.7 Final State Should Be Cinematically Meaningful

不能只结束于：

```text
assembled
```

更好的结束：

```text
assembled → activated → consequence
```

Teleport：

```text
portal opens
→ portal reflection
→ scientist pulled forward
→ unresolved
```

形成新的悬念。

---

# 21. Procedural Montage 的典型叙事结构

推荐模板：

```text
1. HOOK / PROBLEM STATE
2. OBJECT / PARTS REVEAL
3. COMMIT TO PROCESS
4. EARLY PROGRESS
5. ACCELERATED PROCESS
6. INTERRUPTION / FAILURE SIGNAL
7. RECOVERY
8. FINAL ASSEMBLY / FINAL STEP
9. ACTIVATION
10. CONSEQUENCE / PAYOFF / NEW PROBLEM
```

短版本可以压缩。

24 Panel 可拆细成：

```text
P01-P03   Hook / Problem
P04-P10   Initial Assembly
P11-P13   Escalation + Interruption
P14-P19   Recovery + Final Assembly
P20-P22   Activation
P23-P24   Consequence / Unresolved End
```

---

# 22. Teleport Case 的结构拆解

## ACT 1 — Problem / Entry

```text
P01 Reflection Hook
P02 Floor Reveal
P03 Core Grab
P04 Core Seat
P05 Base Align
```

目的：

- 先用 goggles reflection 建立视觉 Hook
- 再揭示完整散落状态
- 快速进入操作

## ACT 2 — Assembly Escalation

```text
P06 Cable Thread
P07 Lens Seat
P08 Clamp Snap
P09 Bolt Lock
P10 Ring Drop
P11 Antenna Rise
P12 Spark Jump
P13 Instinct Dodge
P14 Battery Slam
P15 Dial Twist
P16 Glass Lift
P17 Seal Tube
P18 Panel Check
P19 Lever Hit
```

特点：

- 步骤推进
- 镜头尺度变化
- P12-P13 形成意外
- P18 短暂 pause
- P19 再次启动高潮

## ACT 3 — Result Becomes New Problem

```text
P20 Energy Surge
P21 Machine Orbit
P22 Portal Birth
P23 Goggle Flare
P24 Unresolved Pull
```

不是普通“完成展示”，而是：

```text
完成
→ 启动
→ 失控迹象
→ 新危险
```

---

# 23. Golden Case B 定义

本文建议正式将该案例作为：

```text
GOLDEN_CASE_B
NAME: TELEPORT_DEVICE_ASSEMBLY
SEQUENCE_LOGIC: PROCEDURAL_MONTAGE
SUBTYPE: ASSEMBLY
```

主要验证与 Golden Case A 完全不同的能力。

## Golden Case A — Whoking vs Dao Wang

验证：

```text
ACTION_CHOREOGRAPHY
```

重点：

- Character Motion DNA
- Fighting Language
- Spatial Geography
- Attack / Counter
- Cause / Effect
- Screen Direction
- Action Silhouette
- Annotation Arrow

## Golden Case B — Teleport Device Assembly

验证：

```text
PROCEDURAL_MONTAGE
```

重点：

- Prop State Machine
- Process Dependency
- State Before / After
- Rapid Montage
- Rhythm Curve
- Insert Shot Variety
- Final Style / Storyboard Style Separation
- Clean External Metadata
- Single Moment Per Panel
- Final Consequence

两个 Case 必须长期同时保留。

原因：

> 如果后续优化 Action Case 导致 Procedural Case 退化，说明 Skill 已经被过度动作化。

反之亦然。

---

# 24. Golden Case B 验收指标

建议每次影响 Storyboard Core、Prompt Compiler、Continuity、Mode Router 的修改后，至少检查以下项目。

## 24.1 类型识别

必须识别为过程 / 装配驱动，而不是 martial action。

## 24.2 状态推进

24 格中的关键设备状态必须单向推进，不能随机回退。

## 24.3 Cause → Effect

例如：

```text
grabs core
→ seats core

threads cable
→ cable live

slams battery
→ battery in

pulls lever
→ energy surge
```

必须成立。

## 24.4 Panel 单瞬间

不得一格出现同一人物多个时间状态。

## 24.5 Camera Variety

镜头变化要服务装配可读性，而不是随机炫技。

## 24.6 Rhythm Curve

不能 24 格完全同速。

至少应存在：

- Hook
- Acceleration
- Interruption
- Pause / Drop
- Final Spike

## 24.7 Prop Consistency

装置整体设计语言、主要零件、比例需要可追踪。

## 24.8 Style Separation

Storyboard Panel 必须保持 graphite rough sketch。

最终动画风格只能进入：

- Style Swatch
- Style Header
- Final Render Note

不能污染 Panel。

## 24.9 Annotation Placement

Panel 内无箭头和技术标签。

Metadata 在框外。

## 24.10 Ending

不能停在普通“机器修好了”。

必须体现：

```text
portal activation
+ consequence
```

---

# 25. 对当前实现的非破坏式接入建议

由于主设计已经开发中，建议按以下最小改动顺序。

## Step S1 — 仅增加 Golden Case 文档与测试数据

优先做到：

- 保留原 Prompt
- 建立期望分类
- 建立验收 Checklist

无需修改核心代码。

## Step S2 — Mode Router 增加 PROCEDURAL_MONTAGE

如果当前 Router 是 Enum / 分类 Prompt，只新增一个类别：

```text
PROCEDURAL_MONTAGE
```

不要因此重构其他 Mode。

## Step S3 — Continuity 增加 PROP_STATE

先支持简单形式：

```text
prop_state_before
prop_state_after
```

无需马上实现完整 State Machine Engine。

## Step S4 — Prompt Builder 加入 Single-Moment Rule

这是低成本、高收益规则。

## Step S5 — Style 分层

如果现有代码只有一个 `style`，可以先增加可选：

```text
final_visual_style
storyboard_drawing_style
```

旧输入仍然兼容单 `style`。

## Step S6 — CLEAN_EXTERNAL_METADATA Profile

允许 Storyboard 信息存在，但明确全部在 panel frame 外。

## Step S7 — Rhythm Curve

先作为 planning 文本，不需要复杂数据结构。

## Step S8 — Storyboard IR

只有当前面的实际测试证明 Prompt 拼接已经难维护时，再正式推进。

---

# 26. 建议的实现优先级

按价值 / 改动成本排序：

```text
P0  Golden Case B
P0  PROCEDURAL_MONTAGE recognition
P0  PROP_STATE continuity
P0  Single-Moment Rule

P1  Final Style vs Storyboard Style
P1  CLEAN_EXTERNAL_METADATA
P1  Rhythm Curve

P2  Multi-axis metadata
P2  Continuity Priority
P2  Storyboard IR
```

这样不会干扰当前主设计已经开始的实现。

---

# 27. 与现有 Action 设计的关系

本补充不改变 Action 模块。

而是明确：

```text
ACTION_CHOREOGRAPHY
```

和：

```text
PROCEDURAL_MONTAGE
```

属于两种不同 Sequence Engine。

共享能力包括：

- Shot Language
- Reference Role
- Continuity Core
- Prompt Compiler
- Validator
- Style System
- Output Profiles

差异能力包括：

### ACTION_CHOREOGRAPHY

重点模块：

```text
Motion DNA
Combat Geography
Attack / Counter
Body Trajectory
Screen Direction
```

### PROCEDURAL_MONTAGE

重点模块：

```text
Process Steps
Prop State
Before / After
Dependency
Assembly Progress
```

这意味着未来 Universal Storyboard 的正确结构更接近：

```text
COMMON CORE
   ↓
SEQUENCE-SPECIFIC LOGIC
   ↓
OUTPUT COMPILER
```

而不是一个巨大的 Prompt 同时处理所有类型。

---

# 28. 关键设计决策记录

## Decision S01

**Procedural Montage 作为独立一级 Sequence Logic。**

原因：它的核心驱动力是状态转换，而不是人物动作对抗。

## Decision S02

**Fast-paced 不等于 Action。**

节奏 / 能量与故事类型分离。

## Decision S03

**Animatic 更倾向于 Output / Production Profile。**

不强制立即调整当前枚举。

## Decision S04

**Prop 必须具有 State Continuity。**

静态 Prop Identity 不足以支持装配、修理、制作等序列。

## Decision S05

**One Panel = One Extractable Moment。**

禁止在单格中描述多个连续身体状态。

## Decision S06

**Storyboard Art Style 与 Final Render Style 分离。**

二者可以完全不同。

## Decision S07

**Annotation 是 Presentation Layer，而不是核心 Storyboard Data。**

即使 Panel 内无箭头，Camera / Action / State 信息仍然必须存在。

## Decision S08

**Storyboard IR 作为长期演进方向，不阻塞现阶段开发。**

---

# 29. 后续需要继续收集的 Golden Case

为了验证“全能”而不是“两个类型都能做”，建议后续继续收集：

```text
C — DIALOGUE_PERFORMANCE
D — HORROR_SUSPENSE
E — COMEDY_TIMING
F — PRODUCT_DEMO / COMMERCIAL
G — MV_PERFORMANCE
H — NARRATIVE_DRAMA
```

每增加一个 Golden Case，不应该立即重构 Core。

正确流程：

```text
新增 Case
→ 找出当前结构无法表达的能力
→ 判断是 Type-specific 还是 Common Core
→ 追加 Supplement / ADR
→ 小步实现
→ 回归全部 Golden Cases
```

这样能够防止 Skill 被某一个优秀 Prompt “带偏”。

---

# 30. Golden Case B 原始 Prompt

> 以下内容作为原始案例保存。它是研究材料，不直接等价于系统规范。其中特别是 Panel 内 Annotation 规则，应由系统 Profile 决定，而不是全局继承。

```text
Create a 16:9 image. Project: TELEPORT DEVICE ASSEMBLY | tense sci-fi assembly | fast-paced multiple cuts | Goal: 15-second 24-panel animatic of the character assembling a broken teleport beacon Continuity: ID TELEPORT-ASSEMBLY-01 | Part SINGLE | Identity source: Image #1 for character identity, Image #2 for teleport device design language | Style packet: final video may use retro anime sci-fi with cream/red graphic backdrop, cyan energy, clean cel edges, subtle film grain; storyboard panels themselves must be colorless rough sketch only, light graphite-gray lines, no shading, no color fills, no rendered lighting, simple lab shapes. Scene: A calm scientist rapidly rebuilds a scattered teleport device before its unstable core collapses. Summary: goggles reveal the broken device, then fast cuts track her hands snapping every part into place until the portal flickers alive. Location: cluttered sci-fi workshop floor, parts spread in a semicircle around a central cracked base, loose cables, glass cylinder, antenna, clamps, battery cell, bolts, smoke residue. Roles: one woman scientist with platinum bob, round dark goggles, white coat, blue tie, shoulder strap; precise, urgent, controlled movement. Start -> End: device in separate broken parts reflected in goggles -> assembled beacon hums open with unstable portal pull. Props/Effects: scattered teleport base, glass tube, top ring, antenna, hoses, power cell, warning lights, cyan core glow shown only as simple outline glow in sketch panels. Must read: reflected broken machine becomes a complete machine through rapid cause-effect assembly. Final-video style keyframes: 3 tiny top swatches only for final render look: retro cel sci-fi glow / soft grain paper texture / clean cyan energy against cream-red graphic space. Not storyboard sketch style. Sheet design: warm off-white paper, expressive sci-fi title masthead, compact continuity header, 24 colorless sketch panels in 4 columns x 6 rows, fine modular graphite borders, small panel header chips only, bottom track board: BEAT / CAMERA / ACTION / RHYTHM / ESCALATION / STATE / STYLE. No table-like header. Video-readability layer: each panel is one extractable shot with clear shot order, camera angle, action, rhythm and prop state outside panel artwork. No arrows, labels, captions, subtitles, logos, watermarks, readable device text, or technical marks inside panel images. Panel rules: low-detail light-gray sketch panels; one clear pose per character per panel; no ghost poses, duplicate silhouettes, onion-skin bodies, dense black pencil, charcoal, heavy cross-hatching, rendered shadows, or concept-art finish. Sequence: 24 panels, 4x6 grid.

Track board:
BEAT: P01 reflect | P02 reveal | P03 hand | P04 core | P05 base | P06 cable | P07 lens | P08 clamp | P09 bolt | P10 ring | P11 antenna | P12 spark | P13 dodge | P14 battery | P15 twist | P16 glass | P17 seal | P18 screen | P19 lever | P20 surge | P21 orbit | P22 portal | P23 flare | P24 pull
CAMERA: ECU | whip wide | insert | crash-in | overhead | side track | macro | low insert | top insert | push-in | tilt-up | reaction CU | dutch | insert | orbit | high angle | macro | OTS | low wide | whip pan | arc | locked | ECU | pullback
RHYTHM: hold | burst | fast | snap | fast | burst | fast | snap | fast | rise | fast | impact | burst | fast | snap | fast | rise | pause | hit | surge | peak | drop | final spike | unresolved
ACTION: sees parts | steps in | grabs core | sets core | aligns base | threads cable | seats lens | clamps side | locks bolt | drops ring | raises antenna | spark jumps | leans away | slams cell | twists dial | lifts glass | seals tube | checks panel | pulls lever | energy climbs | device turns | portal opens | goggles flare | pulled forward
STATE: broken | scattered | core loose | core set | base aligned | cable live | lens set | clamp fixed | bolt locked | ring seated | antenna up | spark active | smoke | battery in | dial armed | glass ready | tube sealed | screen awake | lever down | unstable | assembled | portal forming | reflection bright | unresolved
STYLE: graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite | graphite

Sequence:
01 Reflection Hook: ECU 85mm / broken teleport parts reflected across both round goggles, face still / strip: reflect / ECU / sees parts / hold / tension / broken / graphite
02 Floor Reveal: whip pan 24mm wide / cut from goggles to scattered device pieces around her boots / strip: reveal / whip wide / steps in / burst / rise / scattered / graphite
03 Core Grab: insert macro / gloved hand snatches cracked energy core from wires / strip: hand / insert / grabs core / fast / rise / core loose / graphite
04 Core Seat: crash-in close / she drops the core into the open base socket / strip: core / crash-in / sets core / snap / rise / core set / graphite
05 Base Align: overhead / both hands rotate the heavy circular base into alignment / strip: base / overhead / aligns base / fast / rise / base aligned / graphite
06 Cable Thread: side track / she drags a thick cable through the frame and plugs it in / strip: cable / side track / threads cable / burst / surge / cable live / graphite
07 Lens Seat: macro insert / small lens module clicks over the core housing / strip: lens / macro / seats lens / fast / surge / lens set / graphite
08 Clamp Snap: low insert / side clamp slams shut on the device shell / strip: clamp / low insert / clamps side / snap / surge / clamp fixed / graphite
09 Bolt Lock: top insert / screwdriver-like tool tightens a single key bolt / strip: bolt / top insert / locks bolt / fast / surge / bolt locked / graphite
10 Ring Drop: push-in 35mm / top ring descends into her hands and locks around the cylinder / strip: ring / push-in / drops ring / rise / surge / ring seated / graphite
11 Antenna Rise: tilt-up / she pulls a thin antenna upright as the device silhouette grows / strip: antenna / tilt-up / raises antenna / fast / peak / antenna up / graphite
12 Spark Jump: reaction CU / small energy spark snaps from device toward her goggles / strip: spark / reaction CU / spark jumps / impact / spike / spark active / graphite
13 Instinct Dodge: dutch angle / she leans back, coat swinging, one hand protecting the core / strip: dodge / dutch / leans away / burst / spike / smoke / graphite
14 Battery Slam: insert / rectangular power cell slams into front slot / strip: battery / insert / slams cell / fast / peak / battery in / graphite
15 Dial Twist: orbit close / her fingers twist a round dial hard, knuckles tense / strip: twist / orbit / twists dial / snap / peak / dial armed / graphite
16 Glass Lift: high angle / transparent cylinder shell is raised over the glowing core / strip: glass / high angle / lifts glass / fast / peak / glass ready / graphite
17 Seal Tube: macro / gasket compresses and the tube locks flush / strip: seal / macro / seals tube / rise / peak / tube sealed / graphite
18 Panel Check: OTS / over her shoulder, blank control panel wakes with simple light shapes, no readable text / strip: screen / OTS / checks panel / pause / drop / screen awake / graphite
19 Lever Hit: low wide / she plants her stance and pulls the main lever down / strip: lever / low wide / pulls lever / hit / spike / lever down / graphite
20 Energy Surge: whip pan / cables tense, smoke curls, device shakes in place / strip: surge / whip pan / energy climbs / surge / peak / unstable / graphite
21 Machine Orbit: arc shot / camera circles the fully assembled beacon, her silhouette behind it / strip: orbit / arc / device turns / peak / peak / assembled / graphite
22 Portal Birth: locked symmetry / simple circular portal shape opens inside the cylinder / strip: portal / locked / portal opens / drop / awe / portal forming / graphite
23 Goggle Flare: ECU / portal reflection blooms in both goggles, her mouth tight with focus / strip: flare / ECU / goggles flare / final spike / spike / reflection bright / graphite
24 Unresolved Pull: pullback wide / assembled beacon tugs coat, tie and loose papers toward the portal as she braces forward / strip: pull / pullback / pulled forward / unresolved / final spike / unresolved / graphite
```

---

# 31. 本补充文档最终结论

TELEPORT DEVICE ASSEMBLY 说明 Universal Storyboard 不能只按照“题材”分类，而应该至少意识到三个彼此独立的问题：

```text
这段内容靠什么逻辑推进？
它的节奏和能量是多少？
最终要以什么生产格式表达？
```

但考虑到现有设计已经开发中，本阶段不建议推翻现有 Router。

最实际的演进路径是：

```text
保留当前设计
+
新增 PROCEDURAL_MONTAGE
+
新增 PROP_STATE_CONTINUITY
+
新增 Single-Moment Rule
+
逐步分离 Final Style / Storyboard Style
+
将 Teleport Case 纳入 Golden Regression
```

长期再根据多个 Golden Case 的真实表现，决定是否正式升级为：

```text
Sequence Logic
+ Energy Profile
+ Output Profile
+ Annotation Profile
+ Continuity Priority
+ Storyboard IR
```

核心原则仍然是：

> **不为了架构漂亮而重构；以真实故事板生成效果作为每一次抽象升级的依据。**
