# Golden Case：Whoking vs Dao Wang Action Previs

> 用途：`ACTION_PREVIS` 模式的设计参考、提示词结构研究与后续回归测试样例。  
> 状态：Reference / Golden Case  
> 原始来源：用户从网络收集的优秀 Storyboard Prompt。  
> 原则：本文件首先保留原始 Prompt，不将它直接视为系统规范；其优秀部分将被抽象进入 Skill，而存在歧义的部分通过兼容 Profile 保留。

---

## 1. 为什么保留这个案例

这个案例很适合作为 Action Previs 的长期 Golden Case，因为它不仅描述“画什么”，还包含了较完整的导演控制层：

- Project Card
- Continuity Header
- Character Identity Priority
- Character Fighting Language
- Scene Geography
- Storyboard Sheet Format
- Visual Language
- Action DNA
- Shot Design Rules
- 20 Panel Beat Map
- Combat Rules
- Negative Prompt

尤其值得研究的是：

1. 角色不是只有外貌设定，而是有各自独立的 **Motion / Fighting Language**。
2. 20 个 Panel 不是平均分配，而是按动作因果和节奏设计。
3. 明确要求 `cause and effect`、`readable combat geography`。
4. 明确反对 repeated stare-down、dead air 和重复 posing。
5. Wide、Medium、Insert、ECU、High Angle、Low Angle 等镜头不是随机轮换，而是服务于动作可读性。
6. 最后一格是一个清晰的动作结果 / frozen-action silhouette，而不是普通 Hero Pose。
7. Storyboard Sheet 本身被当作一个“设计物”处理，而不是简单网格。

因此，本案例将用于验证未来 Universal Storyboard Skill 的 `ACTION_PREVIS` 模块是否真正比普通 Prompt Generator 更强。

---

## 2. 原始 Prompt（保留）

```text
Create a 16:9 STORYBOARD SHEET image.

Use the two provided reference images for character identity only.
Image A is Whoking, the monkey kung fu master.
Image B is Dao Wang, the serpent kung fu master.

[PROJECT CARD]
Create a designed typographic masthead at the top of the sheet, not a table.

TITLE LOCKUP:
WHOKING VS DAO WANG: STONE COURTYARD DUEL

META LINE:
anime wuxia / relentless kung fu action / elegant staccato escalation

PRIORITY LINE:
nonstop action, readable combat geography, lyrical anime-wuxia motion, no repeated stare-down beats

MICRO BRIEF:
a 15-second anime-wuxia duel where Whoking attacks with agile monkey-style unpredictability and Dao Wang answers with serene snake-style precision. The sequence must be full of action from the very first panel to the very last panel. Do not repeat looking, staring, waiting, posing, or stillness beats. Start with action immediately and end on a strong action finish.

[CONTINUITY HEADER]
SEQUENCE ID:
WHOKING_DAOWANG_ANIME_WUXIA_20P

PART:
SINGLE

STYLE PACKET:
stylized cinematic anime-wuxia action previs, sculpted character forms, warm dusty stone courtyard palette, muted orange costume accents, graphite-black serpent scales, gray monkey fur, soft late-afternoon light, drifting dust haze, flowing cloth motion, elegant speed lines, crisp silhouettes, restrained motion blur, dynamic camera language, premium animated feature storyboard energy.

REFERENCE PRIORITY:
use Image A as Whoking identity reference and Image B as Dao Wang identity reference.

[SUBJECTS]
WHOKING:
monkey kung fu master, gray fur, amber eyes, red-orange short jacket, wrapped torso, loose gray pants, tail, agile and unpredictable.
His fighting language is spring-loaded monkey style: low crouches, sudden leaps, hand slaps, aerial changes of direction, playful feints, quick scrambling footwork, low-to-high attacks.

DAO WANG:
serpent kung fu master, black scaled serpent head with glowing red eyes, mustard-orange martial jacket, wrapped forearms and feet, dark loose pants, upright disciplined posture.
His fighting language is snake style: precise tracking, coiling evasions, narrow-line counters, throat-line strikes, exact hand shapes, minimal wasted motion, eerie calm.

[SCENE]
Set the duel in an open stone courtyard with worn paving, low steps, broken pillars, drifting dust, and open negative space for readable action. The environment should be simple and elegant. Show clear orientation, direction, counters, pressure changes, gliding landings, cloth arcs, tail curves, dust trails, and small stone-chip accents.

[STORYBOARD FORMAT]
Use a 5 x 4 grid for 20 panels.
This is a fast anime-wuxia action sheet.
Every panel should contain action or immediate action continuation.
No repeated face-off panels.
No repeated stillness panels.
No dead air.

Panel numbers should be outside the frames.
Add short shot labels and tiny micro-action captions under each panel.
Use red frame boxes to indicate camera framing.
Use blue arrows to indicate motion, attack direction, camera movement, body travel, and force lines.
Keep the sheet clean, premium, cinematic, and easy to scan.

[VISUAL LANGUAGE]
Rough hand-drawn storyboard look.
Loose black linework.
Gray tonal shading.
Red framing boxes.
Blue motion arrows.
Minimal but clear environment drawing.
Characters simplified but expressive and instantly distinguishable by silhouette.
Do not render as a finished illustration.
This must look like a professional anime action previs storyboard sheet.

[ANIME WUXIA ACTION DNA]
The choreography should feel anime-wuxia: flowing, elegant, sharp, airborne, lyrical, and heightened, but still readable.
Emphasize fast rhythmic cutting, dynamic perspective, strong silhouettes, cloth and tail flow, speed-line energy, and elegant cause-effect combat beats.
The action should feel mythic and fluid, not comedic.
Every movement should have a clear before-and-after logic.

[SHOT DESIGN RULES]
Use strong shot variety: wide, medium-wide, medium, close-up, extreme close-up, low angle, high angle, over-shoulder, tracking-feel shots, impact inserts, reaction inserts.

Maintain readable combat geography.
Include occasional wide resets, but they must still contain active movement, not static posing.
Do not clutter panels.
Prioritize one clear action idea per panel.

[20 PANEL ACTION BEAT MAP]
01. Action opening wide shot, Whoking is already airborne lunging across the stone courtyard toward Dao Wang.
02. Low-angle insert, Whoking’s foot skims a pillar edge or stone step to redirect momentum.
03. Medium action beat, Dao Wang slips off-line with a coiling snake-body motion as Whoking slashes past.
04. Close impact insert, forearm parry and hand-slap contact with dust and sleeve arc.
05. Medium-wide crossing beat, both fighters pass each other and instantly reverse direction.
06. High-angle action beat, Whoking flips over Dao Wang with tail and jacket trailing.
07. Extreme close-up insert, Dao Wang’s snake-hand shoots toward Whoking’s throat line.
08. Medium evasive beat, Whoking twists midair and lands in a sliding crouch while redirecting with one hand.
09. Wide active reset, both circle in motion around broken pillars, kicking up dust while re-engaging.
10. Medium escalation beat, Whoking springs from low to high in a rapid monkey-style combo.
11. Insert burst, hand slap, foot plant, cloth whip, stone chip, speed-line motion.
12. Low-angle counter beat, Dao Wang rises through the pressure with a precise upward serpent deflection.
13. Medium-wide clash, both collide in a sweeping exchange with flowing sleeves and clean silhouettes.
14. Close-up insert, Dao Wang coils under an overhead strike and threads a narrow counterline.
15. Dynamic tracking-feel shot, Whoking vaults off a low stone surface and attacks from an oblique angle.
16. Impact insert, Dao Wang catches or redirects the line at the wrist and shoulder with exact economy.
17. Wide escalation shot, both dash across the courtyard in a fast elegant crisscross exchange.
18. Extreme close-up insert, claws, wrapped hand, eyes, dust, and cloth all cutting past in sharp anime rhythm.
19. Medium finishing clash, Dao Wang threads inside Whoking’s final burst and turns the momentum.
20. Strong ending action frame, Dao Wang’s finishing snake-hand stops at Whoking’s throat while Whoking is still in motion and off-balance, with both bodies forming a dramatic frozen-action silhouette.

[IMPORTANT COMBAT RULES]
The action must read clearly.
Each panel must show cause and effect.
The sequence must feel nonstop, fast, graceful, and premium.
Whoking must feel agile, spring-like, mischievous, and fluid.
Dao Wang must feel serene, exact, economical, and intimidating.
Keep start and end action-heavy.
Avoid repetitive anticipation, repeated stare-downs, empty pause panels, and static hero poses.

[NEGATIVE]
no polished final illustration
no soft vague posing
no unreadable action clutter
no repeated staring panels
no repeated stillness panels
no comedy tone
no logos
no watermark
```

---

## 3. 可抽象进入 Skill 的优秀设计模式

### 3.1 Project Card 不是“参数表”

这个案例顶部使用：

```text
TITLE LOCKUP
META LINE
PRIORITY LINE
MICRO BRIEF
```

它的价值在于先让图像模型理解整张 Sheet 的“导演任务”，而不是马上进入 Panel 细节。

建议进入未来 Storyboard Sheet Prompt Builder。

---

### 3.2 Continuity Header

```text
SEQUENCE ID
PART
STYLE PACKET
REFERENCE PRIORITY
```

这是很好的 Sheet 级上下文封装方式。

未来可扩展：

```text
MODE
ANNOTATION PROFILE
LOCATION ID
CHARACTER IDs
CONTINUITY VERSION
```

---

## 4. 最值得吸收的概念：Character Motion DNA

Whoking 与 Dao Wang 的差异不只来自外观。

Whoking：

```text
spring-loaded
low crouch
sudden leap
hand slap
aerial changes of direction
playful feint
scrambling footwork
low-to-high
```

Dao Wang：

```text
precise tracking
coiling evasion
narrow-line counter
throat-line strike
exact hand shapes
minimal wasted motion
```

这应该被抽象为：

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
```

未来 Action / Dance / Performance 模式都可以复用。

---

## 5. 最值得吸收的动作原则：Cause → Effect

本案例不是 20 个随机动作姿势。

例如：

```text
P01 Whoking 发起空中突进
  ↓
P02 踩柱边改变动量
  ↓
P03 Dao Wang 离线闪避
  ↓
P04 前臂格挡与掌击产生接触
  ↓
P05 两人交错并反向
```

这里已经形成：

```text
INITIATION
→ REDIRECTION
→ EVADE
→ CONTACT
→ SPATIAL CHANGE
```

未来 Action Choreography 模块应该优先学习这种结构，而不是学习具体猴拳动作。

---

## 6. Active Wide Reset

P09：

```text
Wide active reset, both circle in motion...
```

这个概念非常重要。

动作片连续使用 CU / Insert / Extreme Perspective 后，观众容易失去人物地理关系。

普通 Storyboard 会插一个 Wide Reset，但容易变成：

> 两个人重新站好互相看。

这个案例明确要求：

> Reset 仍然必须在运动中完成。

建议正式命名为：

```text
ACTIVE_WIDE_RESET
```

用于：

- 战斗
- 追逐
- 体育
- 舞蹈

---

## 7. One Clear Action Idea Per Panel

这是 Action Storyboard 的高价值规则。

不要让一格同时描述：

```text
角色跳起 → 转身 → 踢击 → 被格挡 → 落地 → 再反击
```

一格应锁定一个清晰关键瞬间。

复杂动作通过连续 Panel 的 Before / Contact / After 来读。

---

## 8. Shot Variety 的正确理解

本案例有：

- Wide
- Medium-wide
- Medium
- Close-up
- Extreme close-up
- Low angle
- High angle
- Tracking feel
- Impact insert

但它不是为了达到“镜头类型数量 KPI”。

例如：

- Wide：解释空间 / 大动作
- ECU：强调威胁或瞬间触点
- Insert：触感与节奏
- High Angle：展示翻越关系
- Tracking Feel：维持速度

未来 Validator 不应该简单判断“是否每种镜头都出现”，而应该判断镜头选择是否服务当前 Cinematic Function。

---

## 9. Final Panel 设计

P20 很值得保留作为原则案例：

```text
finishing snake-hand stops at Whoking’s throat
Whoking is still in motion and off-balance
both bodies form a frozen-action silhouette
```

它同时完成：

- 动作结果
- 人物关系
- 胜负倾向
- 剪影可读性
- 强视觉结束

这比随机“胜者摆英雄 Pose”更有叙事价值。

---

## 10. 与 Universal Skill 新规范的冲突：红蓝箭头

原案例：

```text
Red frame boxes = camera framing
Blue arrows = motion, attack direction, camera movement, body travel, force lines
```

这个规则作为单张图的视觉语言可以成立，但不适合作为 Universal Skill 的系统默认。

主要问题：

### 10.1 蓝箭头语义过载

同一种蓝色箭头可能表示：

- 人物移动
- 攻击方向
- 力线
- Camera Pan
- Camera Track

对于 Agent 和图像模型来说，语义不稳定。

### 10.2 Frame Box 与 Motion Color 没有体系对应

红色在这里表示 Frame；在其他 Storyboard 体系中红色常用于动作。

如果多个参考模板混用，会产生冲突。

---

## 11. 新系统中的处理方式

### 默认 `ANNOTATION_PRO`

```text
RED    = subject/object movement, attack, force, travel
BLUE   = camera movement
GREEN  = framing, crop, reframe, composition
ORANGE = lighting / VFX direction
PURPLE = rhythm / emotion / music hit
BLACK  = labels / lens / action notes
```

### `ANNOTATION_SIMPLE`

```text
RED   = subject movement
BLUE  = camera movement
BLACK = notes
```

### `ANNOTATION_CLEAN`

无箭头，只保留 Storyboard Frame 与标签。

### `ANNOTATION_LEGACY_BLUE`

用于完整复现本案例：

```text
RED FRAME = camera framing
BLUE ARROW = motion / attack / camera / force
```

因此：

> 我们不否定原 Prompt 的视觉方案，而是把它从“默认规则”降为“兼容 Profile”。

---

## 12. 不应全局吸收的规则

本案例多次强调：

```text
Every panel should contain action.
No stillness.
No dead air.
```

这些对 `ACTION_PREVIS` 是优点，但不能写成 Universal Skill 的全局原则。

否则会破坏：

- Dialogue 的停顿
- Horror 的静止
- Comedy 的 Hold
- Emotional Scene 的呼吸

因此新系统采用：

> **Every panel must create a new cinematic function.**

而在 `ACTION_PREVIS` 内再增加：

> Avoid dead air; active continuation is preferred unless a deliberate impact hold is required.

---

## 13. Golden Case 回归测试目标

未来每次修改 Action Previs 模块后，用本案例检查以下指标。

### 13.1 Storyboard Structure

- [ ] 是否仍然正确生成 5×4 / 20 Panel
- [ ] 是否有 Project Card
- [ ] 是否有 Continuity Header
- [ ] Panel Number 是否在 Frame 外
- [ ] Micro Caption 是否足够短

### 13.2 Character Fidelity

- [ ] Whoking / Dao Wang 是否能明显区分
- [ ] Identity Reference 是否没有被 Style Reference 覆盖
- [ ] Costume / silhouette 是否稳定

### 13.3 Motion DNA

- [ ] Whoking 是否持续体现 spring-loaded / low-to-high / aerial redirect
- [ ] Dao Wang 是否持续体现 precise / coiling / economical
- [ ] 两人是否没有变成相同动作风格

### 13.4 Combat Geography

- [ ] 初始左右关系是否可读
- [ ] Crossing 后空间变化是否合理
- [ ] Close-up / Insert 后是否能重新建立空间
- [ ] P09 Active Wide Reset 是否有效

### 13.5 Cause → Effect

- [ ] 每个攻击是否能找到发起原因
- [ ] Dodge / Block / Counter 是否回应上一动作
- [ ] 是否出现随机动作 Pose
- [ ] 是否出现瞬移

### 13.6 Shot Design

- [ ] Shot Variety 是否有目的
- [ ] 是否过度使用 ECU / Insert
- [ ] Wide 是否仍然承担 Geography
- [ ] Camera Movement 与 Subject Movement 是否区分清楚

### 13.7 Redundancy

- [ ] repeated stare-down = 0
- [ ] repeated static hero pose = 0
- [ ] redundant anticipation 尽量为 0
- [ ] 相邻 Panel 不应表达完全相同信息

### 13.8 Ending

- [ ] 最后一格是否有明确动作结果
- [ ] 是否形成强剪影
- [ ] 是否不是普通站立 Pose

### 13.9 Annotation

如果使用 `ANNOTATION_PRO`：

- [ ] 红箭头只表达 Subject / Object Motion
- [ ] 蓝箭头只表达 Camera Motion
- [ ] Reframe 使用绿色

如果使用 `ANNOTATION_LEGACY_BLUE`：

- [ ] 允许复现原始红框 + 蓝色综合箭头
- [ ] Prompt 中必须明确声明该 Profile，避免与默认规则混用

---

## 14. 后续可做的对照实验

建议在 Action Previs Phase 开发时，同一个 Whoking Story 使用三组 Prompt 做生成对比：

### A. Original Prompt

即本文件保存的原始 Prompt。

### B. Universal Skill + LEGACY_BLUE

使用新 Skill 的 Director Plan / Continuity / Motion DNA，但保持原来的红框蓝箭头视觉规范。

### C. Universal Skill + ANNOTATION_PRO

使用：

```text
RED = Subject Motion
BLUE = Camera Motion
GREEN = Framing
```

比较三组：

- 动作可读性
- 人物一致性
- 箭头可读性
- Combat Geography
- Sheet 美观度
- 图像模型遵循率

这可以验证“新系统理论上更规范”是否真的转化为更好的实际输出，而不是只在文档上更漂亮。

---

## 15. 本案例在项目中的定位

不要把整个原 Prompt 复制到 `SKILL.md`。

正确使用方式：

```text
Golden Case
   ↓ 抽象
Action Previs Principles
   ↓
references/action-choreography.md
references/character-motion-dna.md
references/annotation-system.md
   ↓
SKILL.md 只负责按模式加载
```

本文件长期保留原案例，作为设计来源和回归基线。
