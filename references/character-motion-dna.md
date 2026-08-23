# Character Motion DNA

Motion DNA 描述角色“怎么动”，不是角色“长什么样”。它与 `IDENTITY_REFERENCE`、服装和 Continuity State 分开：Identity 负责可识别外观，Motion DNA 负责动作语言，Continuity 负责当前这一格的状态。

## 1. 何时启用

- `ACTION_PREVIS` 必须启用。
- Dance、Sports、MV Performance 或持续身体表演可以启用。
- 普通 Narrative 只有在角色动作本身是叙事核心时启用。
- Dialogue、Horror 默认不创建完整 Motion DNA；只记录会影响表演的姿态或节奏词。

## 2. Motion DNA Schema

```yaml
motion_dna:
  locomotion:
  stance:
  center_of_gravity:
  weight:
  acceleration:
  rhythm:
  attack_language:
  defense_language:
  hand_language:
  recovery_language:
  signature_moves:
  forbidden_motion:
```

每个字段写可观察的动作倾向，不写空泛形容词。比如“灵活”不够具体，应拆成“低重心、弹簧式起动、突然变向、落地后快速重组”。

## 3. Field Meaning

- `locomotion`：走、跑、滑、跃、翻、绕行或贴地移动的方式。
- `stance`：准备姿态、重心高度、身体开合和占地方式。
- `center_of_gravity`：重心稳定、前倾、后仰、盘绕、漂浮或不断切换。
- `weight`：轻盈、沉重、精确、黏地、弹性或惯性如何被看见。
- `acceleration`：突然爆发、逐渐加速、节拍式加速或保持匀速。
- `rhythm`：连续、断裂、错拍、蓄力-爆发、呼吸式停顿。
- `attack_language`：攻击线、接触方式、假动作、距离偏好和力的方向。
- `defense_language`：闪避、格挡、卸力、绕线、后撤或借力方式。
- `hand_language`：手掌、拳、指、武器握持和接触的独特习惯。
- `recovery_language`：落地、受击、失衡、回身后如何恢复动作。
- `signature_moves`：少量能反复识别角色的动作模式，不是完整编舞。
- `forbidden_motion`：会让角色动作风格漂移的姿态或运动方式。

## 4. Extraction and Priority

提取顺序：

1. 用户明确给出的动作规则。
2. 角色设定、参考素材或已有动作描述。
3. 场景对动作的物理限制。
4. 模式模板和 Skill 默认值。

Motion DNA 不能覆盖用户明确的剧情结果或 Continuity State。若两个角色的动作描述冲突，按角色分别建立 DNA，不把两者平均成同一种风格。

## 5. Example: Whoking vs Dao Wang

```yaml
CHAR_001_Whoking:
  locomotion: scrambling footwork, spring-loaded launches, aerial redirection
  stance: low crouch, open and playful, ready to coil or vault
  center_of_gravity: low, rapidly shifting
  weight: light, elastic, difficult to pin
  acceleration: sudden burst from stillness or low position
  rhythm: irregular feint, burst, scramble, rebound
  attack_language: slaps, oblique entries, low-to-high combinations
  defense_language: aerial changes of direction, improvised redirection
  signature_moves: pillar rebound, playful feint, midair redirect
  forbidden_motion: rigid upright stance, economical linear blocking only

CHAR_002_Dao_Wang:
  locomotion: precise tracking, measured steps, narrow-line advance
  stance: upright discipline, coiled and compact
  center_of_gravity: controlled and centered
  weight: economical, exact, quietly heavy at contact
  acceleration: minimal preparation, sudden precise release
  rhythm: calm observation, narrow counter, controlled recovery
  attack_language: throat-line strike, exact hand shapes, direct counterline
  defense_language: coiling evasion, small-angle deflection, inside-line counter
  signature_moves: serpent-like coil, wrist redirection, precise stop
  forbidden_motion: scrambling footwork, playful flailing, wasted wide swings
```

## 6. Sampling Rules for Panels

- 每个 Action Panel 只选择一个主要动作想法，并从角色 DNA 中选择 1–2 个可观察特征。
- 连续 Panel 可以改变动作阶段，但不能无原因改变角色的重心、节奏和手部语言。
- Signature Move 不能每格重复；重复时必须承担新的因果或结果功能。
- 受击、落地、失衡和恢复要使用 `recovery_language`，不能直接跳回英雄姿势。
- 对手的 Motion DNA 只影响对方的回应方式，不自动改变本角色的动作语言。

## 7. Motion DNA Validation

- `PASS`：每个主要动作角色有可观察的 DNA，连续 Panel 中风格稳定且彼此区分。
- `WARNING`：角色只使用空泛形容词、Signature Move 过度重复、某个动作阶段缺少恢复逻辑，或两个角色动作区分度不足但仍可解释。
- `BLOCKER`：仅当动作违反用户明确禁用项，或 Motion DNA 与已确定的 Continuity 身体状态直接矛盾时使用；动作同质化本身是 `WARNING`。

严重级别统一见 `references/storyboard-validator.md`。
