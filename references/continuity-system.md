# Continuity State System

Continuity 是跨 Panel 传递的状态，不是最后附加一句“保持一致”。状态只记录会影响下一格决策的事实，避免把 Skill 变成冗重的表格填充器。

## 1. State Activation

| 场景复杂度 | 状态级别 | 必须追踪 |
|---|---|---|
| Action、追逐、多人 Blocking、道具交互、状态变化 | `FULL` | Geography、方向、朝向、动作状态、道具、服装/伤势、光线 |
| Dialogue、Horror、普通 Narrative 多格场景 | `LIGHT` | 位置锚点、视线/屏幕方向、关键道具、揭示/情绪状态、必要光线 |
| 单张 Keyframe、海报、单一主体静态构图 | `MINIMAL` | 用户明确的身份、服装、道具和构图事实；不强制 Panel State |

模式规则可以提高状态级别，但不能无理由降低用户明确要求的连续性约束。

## 2. State Domains

```text
screen_direction:
position:
geography_anchor:
facing:
eyeline:
pose_or_motion_state:
travel_direction:
hand_or_contact_state:
prop_state:
costume_state:
damage_state:
lighting_state:
knowledge_or_reveal_state:
prop_state_continuity:
process_continuity:
```

不是所有任务都要填写全部字段。`FULL` 任务重点是能解释动作和空间，`LIGHT` 任务重点是能解释视线、道具和信息揭示。

## 3. Panel In / Out

每个 Panel 内部遵循：

```yaml
continuity_in:
  screen_direction:
  position:
  facing_or_eyeline:
  prop_state:
  state_before:
  state_after:
  body_or_emotion_state:
  reveal_state:

continuity_out:
  screen_direction:
  position:
  facing_or_eyeline:
  prop_state:
  state_before:
  state_after:
  body_or_emotion_state:
  reveal_state:
```

流程：

1. 从 Scene Geography 或上一 Panel 的 `continuity_out` 初始化本格 `continuity_in`。
2. 检查本格的动作、反应、镜头和故事信息是否与 `continuity_in` 相容。
3. 只把本格确实改变的字段写入 `continuity_out`；没有改变的字段沿用。
4. 将 `continuity_out` 传给下一格，不让模型每格重新发明角色状态。

## 4. State Transition Rules

- 角色换手、放下、拾起或接触道具时，必须明确发生在哪一格。
- 角色改变位置时，必须有可见的移动、切换、遮挡后合理接续或明确转场。
- 角色转身或改变屏幕方向时，必须说明原因，并更新 `facing` 与 `screen_direction`。
- 伤势、湿身、破损、血迹、服装变化和光线变化不能无故回滚。
- Reveal 状态只能向前推进，除非故事明确设计了误导、幻觉或信息被撤回。
- 镜头切换不等于状态重置；新镜头要继承上一格的事实状态。

### PROP_STATE_CONTINUITY

追踪关键道具从未处理、已拆出、已定位、已安装、已测试到已激活等阶段。状态默认单向推进；不可逆状态不能无原因回退。`State Before` 是本格开始时可见的道具状态，`State After` 是本格完成后传给下一格的状态，由 Director Plan 自动推导，不要求用户手填。

### PROCESS_CONTINUITY

把步骤依赖写成 `A → B → C`：只有前置步骤完成，后续步骤才可出现。若发生中断，必须说明当前步骤是否保持、失败、回退或改走替代路径；不得凭空跳过关键依赖。

## 5. Mode Scope

### `ACTION_PREVIS` / `FULL`

追踪角色位置、Camera Axis、攻击/防御状态、接触点、运动方向、关键道具、服装/伤势和环境变化。每次 Counter 都要回应上一格状态。

### `DIALOGUE` / `LIGHT`

追踪左右关系、Eyeline、说话者/听者状态、桌面或录音笔等关键道具、权力变化和沉默后的情绪状态。不要因为没有身体动作而清空状态。

### `HORROR_SUSPENSE` / `LIGHT`

追踪出口、人物位置、可见/不可见区域、威胁线索、光线变化和 Reveal Timing。静止格可以改变 `knowledge_or_reveal_state`。

### `NARRATIVE`

至少追踪关键道具、视觉母题、场景锚点和开头到结尾的情绪状态变化。

### `CINEMATIC_KEYFRAME` / `MINIMAL`

只验证单帧内部的身份、构图、光线、道具和用户硬约束，不强制创建跨 Panel 状态。

### `PROCEDURAL_MONTAGE` / `FULL`

强制启用 `PROP_STATE_CONTINUITY` 与 `PROCESS_CONTINUITY`。每个关键 Panel 记录 State Before / State After，并让下一格继承已完成的道具和过程状态。其他模式只有在确实存在道具状态需求时才启用这两个 Domain。

## 6. Continuity Validation

- `PASS`：当前 Panel 能从 `continuity_in` 推导，`continuity_out` 能支持下一格，关键变化有原因。
- `WARNING`：某个字段缺失但没有直接矛盾、道具状态无原因回退，或过程依赖表达不完整；可以通过补充 Continuity Cue 或下一格 Wide Reset 修复。
- `BLOCKER`：仅当左右关系、核心道具、身体状态、空间位置或揭示状态发生不可解释的反转/回滚，导致下一格无法接续时使用。普通字段缺失、光线变化不够明确或需要更完整的状态记录，按统一 Validator 记为 `WARNING` 或 `NOTE`。

严重级别统一见 `references/storyboard-validator.md`。

Validation Notes 只向用户展示摘要，例如“PASS：录音笔位置连续；WARNING：P07 后空间锚点变弱，建议 P08 使用 Wide Reset”，不默认输出完整状态 YAML。
