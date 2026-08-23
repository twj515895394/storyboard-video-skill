# Annotation Profiles

Annotation Profile 定义故事板上的箭头、框线、颜色和标签如何表达导演意图。它只控制注释层，不改变 storyboard 图像本身的媒介、色彩或渲染风格。

当本文件与 `shot-language.md` 同时加载时，本文件负责 Profile 选择和 Profile 之间的冲突处理；`shot-language.md` 继续提供景别、角度、镜头运动和通用注释词汇。

## 1. Profile Selection

选择顺序：

1. 用户明确指定的 Profile 或颜色语义。
2. 用户明确要求复现某个旧 Prompt 的注释规则。
3. 任务模式和复杂度推断。
4. 默认 Profile。

| Profile | 默认场景 | 视觉复杂度 | 默认行为 |
|---|---|---:|---|
| `ANNOTATION_SIMPLE` | 普通 Narrative、Dialogue、Horror | 低 | 红=主体运动，蓝=摄影机运动，黑=短标签 |
| `ANNOTATION_PRO` | Action、Dance、追逐、复杂 Blocking | 高 | 分离主体、摄影机、构图、光线、节奏和标签语义 |
| `ANNOTATION_CLEAN` | Cinematic Keyframe、广告美术板、情绪板 | 最低 | 不画箭头，只保留 Panel Number、Shot Label 和必要构图说明 |
| `ANNOTATION_LEGACY_BLUE` | 用户明确要求兼容旧 Prompt | 特殊 | 红框=构图，蓝色综合箭头=旧规则；不作为默认值 |
| `CLEAN_EXTERNAL_METADATA` | 用户要求画面内零注释、Procedural Montage Clean Board | 中 | Panel 内无生产注释；完整 metadata 放在 Panel 外部 |

不要为了展示更多颜色而升级 Profile。只有当额外语义会改变 Panel 设计或模型执行结果时，才使用更复杂的 Profile。

## 2. `ANNOTATION_SIMPLE`

```text
RED   = subject / object movement
BLUE  = camera movement
BLACK = panel label, shot label, short action note
```

规则：

- 红箭头只指向人物、道具或力的移动方向。
- 蓝箭头只表达 Pan、Tilt、Track、Push、Pull、Orbit 等摄影机行为。
- 不用红框表达构图，不用蓝箭头表达人物动作。
- 黑色文字保持短小，不把整段故事塞进 Panel。

适合多数普通故事板；如果没有用户要求，不额外绘制绿色、橙色或紫色标记。

## 3. `ANNOTATION_PRO`

| 颜色/标记 | 唯一语义 |
|---|---|
| RED | Subject/Object Motion、Attack、Force、Travel |
| BLUE | Camera Movement |
| GREEN | Framing、Crop、Reframe、Composition、Negative Space |
| ORANGE | Lighting、Shadow、Flare、VFX Direction |
| PURPLE | Rhythm、Emotion、Music Hit、Vocal Cue |
| BLACK | Panel Number、Shot Label、Lens、Action Note |

规则：

- 一种颜色只承担一类语义，箭头旁边用短文字补充，不用颜色单独承担复杂含义。
- 红色和蓝色必须保持主体运动/摄影机运动分离。
- 构图框、裁切和重构图用绿色，不得复用红色 Frame Box。
- 黑白 storyboard 的绘画部分仍保持黑白；颜色只用于注释层。
- 不要求每个 Panel 使用全部颜色；空白比无意义的颜色更好。

## 4. `ANNOTATION_CLEAN`

只保留：

```text
Panel Number outside the frame
Shot Label
One short composition or lighting note when essential
```

不绘制运动箭头、红框、彩色力线或大段技术说明。适合用户要干净的 Cinematic Keyframe、广告美术板、情绪板，或注释会损害画面可读性的任务。

## 5. `ANNOTATION_LEGACY_BLUE`

仅在用户明确要求复现旧 Prompt 或提供了依赖该语义的参考图时启用：

```text
RED FRAME BOX = camera framing / frame composition
BLUE ARROW   = motion, attack, body travel, force, and camera movement
```

编译 Prompt 时必须显式写出这套语义，并声明它覆盖默认 Profile：

```text
Use ANNOTATION_LEGACY_BLUE only:
red frame boxes indicate camera framing;
blue arrows may indicate subject motion, attack direction, body travel, force lines, or camera movement.
Do not mix this legend with ANNOTATION_PRO.
```

Legacy Profile 不是推荐的新规范。只要用户没有明确要求兼容，就回到 `ANNOTATION_SIMPLE`、`ANNOTATION_PRO` 或 `ANNOTATION_CLEAN`。

## 6. Prompt Compiler

在整合 Prompt 中用短 Legend 固定语义：

```text
[ANNOTATION PROFILE]
Profile: ANNOTATION_PRO.
Red arrows = subject/object movement and force.
Blue arrows = camera movement.
Green marks = framing, crop, reframe, and composition.
Orange marks = lighting or VFX direction.
Purple marks = rhythm, emotion, or music hit.
Black text = panel labels and short shot notes.
Keep the storyboard drawing monochrome; use color only for annotations.
```

`ANNOTATION_SIMPLE` 只输出三行 Legend；`ANNOTATION_CLEAN` 明确“no arrows, no colored overlays”；`ANNOTATION_LEGACY_BLUE` 必须输出完整兼容声明。

### `CLEAN_EXTERNAL_METADATA`

Panel 画面内禁止箭头、标签、字幕、摄影机注释、技术标记和可读设备文字；不使用任何生产信息覆盖绘图。Panel 外部保留 `Panel Number`、`Header Chip`、`BEAT`、`CAMERA`、`ACTION`、`RHYTHM`、`ESCALATION`、`STATE` 与 `STYLE`。它不同于 `ANNOTATION_CLEAN`：后者仍可在画面外/边缘保留少量 Panel Number、Shot Label 或必要构图说明。

Compiler 声明：`CLEAN_EXTERNAL_METADATA: keep every panel image free of arrows, labels, captions, camera marks, technical marks, and readable device text; place all production metadata outside the panel frame.`

## 7. Annotation Validation

- `PASS`：每个启用的颜色只有一个语义，Profile 与 Mode/用户要求一致，画面媒介和注释颜色没有混淆。
- `WARNING`：用户要求复杂动作但选择 CLEAN、用户要求干净画面但选择 PRO、注释颜色过多、Panel 中出现过长技术文字。它们是可修复的 Profile/可读性问题，不单独阻断交付。
- `WARNING`：`CLEAN_EXTERNAL_METADATA` 下 Panel 内出现箭头、标签、字幕或技术标记。
- `BLOCKER`：仅当同一 Prompt 同时声明互相冲突的 Profile、同一颜色同时承担互相冲突的语义，或用户明确要求黑白画面却把注释颜色写成画面上色且无法通过“颜色仅用于注释层”消解时使用。

严重级别统一见 `references/storyboard-validator.md`。

## 8. Mode Interaction

- `ACTION_PREVIS`：默认 `ANNOTATION_PRO`，用于区分攻击/身体运动、摄影机运动、重构图和 Active Wide Reset。
- `DIALOGUE`：默认 `ANNOTATION_SIMPLE`，只在复杂 Blocking 或摄影机调度需要时升级为 PRO。
- `HORROR_SUSPENSE`：默认 `ANNOTATION_CLEAN` 或 SIMPLE，避免箭头提前解释威胁来源。
- `CINEMATIC_KEYFRAME`：默认 `ANNOTATION_CLEAN`。
- `COMMERCIAL`：产品信息复杂时可用 SIMPLE；用户要求美术板洁净时用 CLEAN。
