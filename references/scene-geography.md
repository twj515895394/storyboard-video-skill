# Scene Geography

Scene Geography 是 Panel 设计前的简化空间模型。它不追求建筑施工图，而是确保观众知道角色在哪里、朝哪里走、镜头从哪条轴线上看，以及下一格为什么能接上。

## 1. 何时建立

至少满足以下任一条件时建立：

- `ACTION_PREVIS`、追逐、体育、战斗或复杂身体动作。
- 两个或以上角色发生 Blocking、接近、交错或位置交换。
- 场景存在门、窗、柱、楼梯、车辆、桌面、出口等关键空间锚点。
- 叙事依赖左/右关系、进入/离开方向、视线方向或路线选择。
- Close-up、Insert 或快速视角变化可能让观众失去地理关系。

普通单人情绪镜头和 `CINEMATIC_KEYFRAME` 不强制建立完整 Geography，但仍要保持基本空间合理性。

## 2. Geography Card

内部先创建最小 Geography Card：

```text
scene_id:
location:
camera_base_axis:
screen_left:
screen_right:
foreground_anchor:
midground_anchor:
background_anchor:
subject_positions:
movement_lanes:
entry_exit_points:
lighting_direction:
```

### 2.1 空间锚点

优先选择观众能反复认出的固定物：

- 建筑：门、窗、柱、楼梯、墙角、走廊尽头。
- 道具：桌、车、武器、电话、灯、旗帜、明显的地面标记。
- 环境：裂缝、树、霓虹灯、火源、特殊阴影或地面水迹。

每个 Panel 不必重新描述所有锚点，只需保留能证明位置的一个或两个。

### 2.2 Camera Axis

在多人场景中先确定一条 `Camera Base Axis`：

```text
CHAR_A  ─────────────  CHAR_B
             CAMERA BASE AXIS
```

默认保持 180 度关系：同一侧的人物保持相对屏幕方向，除非故事明确要求越轴、环绕或空间重置。越轴必须有视觉或叙事理由，不能由镜头随机变化造成。

## 3. Motion and Blocking

为每个主要主体记录：

```text
subject:
start_zone:
facing:
screen_direction:
destination_zone:
movement_reason:
interaction_point:
```

规则：

- 先确定起点、终点和原因，再选择夸张动作或镜头。
- 左到右、右到左、向镜头、离开镜头和垂直运动都要有明确含义。
- 角色交错后更新空间关系；下一格不能继续使用过期的左右位置。
- 插入特写可以隐藏位置，但不能改变位置；恢复全景时必须接回最近一次已知 Geography。
- 每个动作结果都要落在一个可解释的空间区域中，禁止瞬移。

## 4. Active Wide Reset

当连续使用 Close-up、Extreme Close-up、Insert 或强透视镜头后，检查观众是否仍知道角色在哪里。如果不确定，安排 `ACTIVE_WIDE_RESET`：

- 用 Wide / Full / Overhead 等镜头重建人物、锚点和运动方向。
- Reset 仍可包含奔跑、绕行、滑步、重新接近或环境变化。
- 不要把 Reset 写成两个角色静止站立互看，除非该停顿本身是剧情功能。

## 5. Mode Application

- `ACTION_PREVIS`：完整记录 Camera Axis、角色区域、运动路径、交错和 Active Wide Reset。
- `DIALOGUE`：重点记录两人左右关系、Eyeline、桌面/门等锚点和可能的越轴理由。
- `HORROR_SUSPENSE`：重点记录出口、不可见威胁来源、负空间和灯光/阴影方向；不要为了“清楚”过早展示威胁。
- `NARRATIVE`：只记录会影响叙事转折或最终画面的地理关系。

## 6. Geography Validation

- `PASS`：每个关键主体有可解释的区域、朝向和运动方向，关键锚点能在需要时重建空间。
- `WARNING`：连续多个 Close-up/Insert 后地理变得模糊，但可以通过一个 Wide Reset 修复。
- `BLOCKER`：仅当角色无因果瞬移、无理由越轴导致关系不可解释、出口/目标位置互相矛盾，或关键动作结果无法落到场景空间中时使用。地理变弱、锚点不足或 Active Wide Reset 不够理想，按统一 Validator 记为 `WARNING` 或 `NOTE`。

严重级别统一见 `references/storyboard-validator.md`。
