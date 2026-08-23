# Action Choreography

本文件只适用于 `ACTION_PREVIS`。它把动作设计成可读的因果链，而不是一串独立的漂亮姿势。

## 1. Action State Graph

默认动作阶段为：

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

不要求每个场景完整经过所有阶段，但每个攻击、闪避和反击都必须能说明当前处于哪种阶段，以及它由哪一个前置状态触发。

## 2. Cause → Effect Contract

为每个动作建立最小因果记录：

```text
cause:
initiator:
action:
target_response:
contact_or_miss:
spatial_consequence:
next_state:
```

示例：

```text
cause: Whoking closes the distance from the west pillar
initiator: Whoking
action: spring-loaded oblique leap
target_response: Dao Wang steps off the attack line
contact_or_miss: miss, sleeve grazes dust
spatial_consequence: both cross the clash zone
next_state: Dao Wang has inside-line advantage
```

如果一个 Panel 同时包含起跳、旋转、攻击、被挡、落地和反击，拆成多个 Panel；一格只保留一个主要可读动作瞬间。

## 3. Action Panel Contract

每个 Action Panel 至少内部记录：

```text
panel:
function: ACTION | IMPACT | REACTION | GEOGRAPHY | TRANSITION
action_phase:
cause:
initiator:
subject_motion:
target_response:
contact_or_miss:
spatial_consequence:
shot_language:
camera_movement:
continuity_in:
continuity_out:
```

`subject_motion` 与 `camera_movement` 必须分开；红/蓝注释的语义也必须分开，除非用户明确启用 `ANNOTATION_LEGACY_BLUE`。

## 4. Action Readability Rules

1. 先保证空间和身体关系可读，再追求夸张透视。
2. 复杂动作优先使用 Wide、Full 或清晰的 Medium-wide；Insert 只强调关键触点。
3. 每个动作都要有发起者、目标、方向和结果；没有目标的动作只是姿势。
4. 反击必须回应上一格的动作或空间变化，不能凭空出现。
5. 受击、落地、滑行和恢复要改变 Continuity State，不能无理由回到起始状态。
6. 避免重复 stare-down、重复 anticipation、重复静态 Hero Pose 和无功能空白；这是 Action 的质量建议，不是全局静止禁令。
7. Shot Variety 由 Cinematic Function 驱动，不以镜头种类数量为 KPI。
8. 持续动作不等于每格都必须爆发；必要的 Impact Hold 或 Reaction 可以放慢节奏，但要有功能。Dialogue 的 Silence/Hold 与 Horror 的静止不受本条 Action 偏好影响。

## 5. Combat Geography

Action 设计先读 `scene-geography.md` 和 `continuity-system.md`：

- 初始 Panel 建立双方位置、Camera Axis 和主要锚点。
- 交错、越轴、改变优势位置后，更新空间关系和屏幕方向。
- Close-up / Insert 密集后安排 `ACTIVE_WIDE_RESET` 或其他能恢复地理的镜头。
- Active Wide Reset 必须仍然有运动：绕行、重新接近、滑步、奔跑、环境变化或新的攻击准备。
- Wide 镜头承担 Geography，Close-up/Insert 承担触感、反应或关键接触，不互相替代。

## 6. Escalation Design

升级至少改变一个变量：

```text
distance
speed
height
number_of_subjects
environmental_damage
available_space
injury_or_fatigue
emotional_control
```

升级不是单纯把动作形容词换成“更快、更强”。每一轮升级要给下一轮动作增加新的限制、风险或策略。

## 7. Ending Design

Action 结尾优先使用：

- 明确的动作结果或反转。
- 强剪影中的接触、停手、失衡或胜负倾向。
- 环境被改变后的余波。
- 下一次冲突即将发生的强悬念。

避免普通站立 Hero Pose。结尾必须能回收至少一个前面建立的动作、道具、空间或 Motion DNA 线索。

## 8. Action Validator

- `PASS`：因果链可读、角色动作风格区分清楚、空间关系持续、镜头服务功能、结尾有结果或悬念。
- `WARNING`：连续多个 Panel 缺少 Wide Geography、动作阶段重复、Motion DNA 只剩形容词、升级变量没有变化、动作发起原因/反击回应不够清晰，或结尾缺少冲击力。
- `BLOCKER`：仅当关键动作对象或结果缺失，或触发 Continuity/Geography 的不可调和硬矛盾（例如角色瞬移、身体状态回滚、无理由越轴）时使用。普通动作因果弱、结尾普通或没有完整覆盖动作阶段，按 `WARNING` 或 `NOTE` 处理。

严重级别统一见 `references/storyboard-validator.md`。

这些规则只作用于 `ACTION_PREVIS`，不得用于判断 Dialogue、Horror 或 Comedy 的静止和停顿是否有效。
