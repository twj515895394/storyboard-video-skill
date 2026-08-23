# Reference Image System

参考图不是一组没有区别的“风格参考”。每张图必须先声明它控制什么、不控制什么，以及作用于哪个角色、场景、道具或全局视觉层。

## 1. 何时启用

- 用户提供图片、资产或明确说“参考这张图”时启用。
- 用户只提供文字设定时，不凭空创建 `Reference Map`。
- 一张图可以承担多个职责，但只有在用户明确要求或内容确实不可拆分时才这样做；默认优先拆成多个角色。
- 没有可识别的图像 ID 时，按出现顺序临时命名为 `REF_001`、`REF_002`，并在 Validation Notes 中说明这是推断命名。

## 2. Reference Roles

| Role | 控制内容 | 默认不控制 |
|---|---|---|
| `IDENTITY_REFERENCE` | 脸部身份、年龄感、肤色/毛色、面部几何、整体身份识别 | 服装、故事板绘制风格、动作姿态 |
| `COSTUME_REFERENCE` | 服装结构、材料、颜色、配饰、磨损 | 脸部身份、场景、绘制媒介 |
| `HAIR_REFERENCE` | 发型、发色、发量、轮廓 | 脸部身份、服装、动作 |
| `BODY_REFERENCE` | 身体比例、体型、身高关系、轮廓 | 脸部身份、服装材质、镜头语言 |
| `PROP_REFERENCE` | 道具外形、材质、尺寸、操作方式 | 角色身份、场景风格、摄影机运动 |
| `LOCATION_REFERENCE` | 场景布局、空间锚点、时间和环境质感 | 角色身份、服装、故事板媒介 |
| `ARCHITECTURE_REFERENCE` | 建筑结构、门窗、楼梯、柱体、动线 | 角色外观、光照方案、绘制风格 |
| `STYLE_REFERENCE` | 线条、笔触、渲染方式、构图语言、视觉媒介 | 角色身份、道具事实、场景地理 |
| `LIGHTING_REFERENCE` | 光源方向、色温、阴影形状、对比度、氛围 | 角色身份、服装结构、动作因果 |
| `COLOR_REFERENCE` | 色板、主辅色、饱和度、色彩脚本 | 几何结构、角色身份、镜头运动 |
| `COMPOSITION_REFERENCE` | 构图重心、留白、层次、主体比例、画面组织 | 角色身份、动作因果、故事情节 |
| `CAMERA_REFERENCE` | 视角、镜头高度、焦段感、画面比例、运镜气质 | 角色身份、服装、道具事实 |
| `MOTION_REFERENCE` | 动作质感、姿态语汇、速度、重心、运动轨迹 | 脸部身份、服装事实、场景结构 |

如果一张图同时提供多个职责，在 Map 中拆成多个 `role` 条目并写明相同的 `source`，不要用一个含糊的 `STYLE_REFERENCE` 代替所有控制项。

## 3. Reference Map

有参考图时，在 Director Plan 内部建立如下映射；不要求强制以 YAML 展示给用户：

```yaml
references:
  - id: REF_001
    source: attached-image-1
    role: IDENTITY_REFERENCE
    target: CHAR_001
    priority: required
    controls:
      - face identity
      - age impression
      - silhouette cues
    must_not_control:
      - costume
      - storyboard rendering style
    conflict_policy: identity wins for face and silhouette
```

每条映射至少回答：

- 这是哪一张图？`source`
- 它负责什么？`role`
- 作用于谁？`target`
- 允许改变哪些维度？`controls`
- 明确禁止改变哪些维度？`must_not_control`
- 与其他图冲突时谁负责？`conflict_policy`

## 4. Priority and Conflict Rules

优先级按以下顺序处理：

1. 用户在当前请求中的明确指令和硬约束。
2. 对应目标、对应职责的 Reference。例如 `IDENTITY_REFERENCE` 控制脸部身份，`LOCATION_REFERENCE` 控制场景结构。
3. 项目/场景上下文，例如既定服装状态、时间、光线方向和连续性状态。
4. 全局 `STYLE_REFERENCE`、`COLOR_REFERENCE`、`CAMERA_REFERENCE`。
5. Skill 默认值。

同一职责出现多张图时：

- 用户明确指定的优先级最高。
- 如果用户指定“以 A 为脸、以 B 为服装”，按职责拆分，不视为冲突。
- 如果同一职责的两张图互相矛盾，优先更明确、更近的用户说明；仍无法判断时保留两者并输出 `WARNING`，不要静默随机选择。
- `STYLE_REFERENCE` 不得覆盖 `IDENTITY_REFERENCE`、`LOCATION_REFERENCE` 或 `PROP_REFERENCE` 的事实约束。
- `CAMERA_REFERENCE` 只改变观看方式，不改变角色、道具或空间发生了什么。
- `MOTION_REFERENCE` 可以影响动作质感，但不得改变角色身份或凭空添加剧情结果。

简化优先级可以记为：

```text
user constraints
  > target-specific role reference
  > scene/project continuity
  > global style / color / camera reference
  > skill defaults
```

## 5. Prompt Compiler Format

将 Reference Map 编译到图像 Prompt 时，使用职责分离的自然语言：

```text
[REFERENCE PRIORITY]
REF_001 controls CHAR_001 identity, face, age impression, and silhouette only.
REF_002 controls CHAR_001 costume materials and accessories only.
REF_003 controls the courtyard layout and broken-pillar geography only.
REF_004 controls rough black-and-white storyboard linework only.
Do not let the style reference change character identity, costume facts, or scene geography.
```

不要只写“保持参考图一致”。应明确“谁控制什么”，并把禁止控制项写入 Prompt 或 Validation Notes。

## 6. Reference Validator

返回前检查：

- `PASS`：每张参考图都有明确 Role 和 Target，控制边界可以解释。
- `WARNING`：参考图没有可识别 ID、同一职责存在冲突、用户要求高度一致但没有 Identity Reference，或一张图承担了过多职责。
- `BLOCKER`：仅当两个参考图控制同一关键事实且无法确定优先级、角色身份被 Style Reference 覆盖，或 Location Reference 与 Continuity Geography 直接矛盾且无法决定时使用。ID 推断、职责过多或可补充的优先级问题保持 `WARNING`。

对没有参考图的任务，不输出“缺少 Reference Map”的错误；只有用户明确要求参考一致性但没有提供可用参考时，才记录 `WARNING`。

严重级别统一见 `references/storyboard-validator.md`。

## 7. Mode Interaction

- `ACTION_PREVIS`：优先 Identity、Motion、Costume、Location/Architecture；检查动作语言和空间锚点是否来自正确角色。
- `DIALOGUE`：优先 Identity、Costume、Location、Composition；检查视线和权力关系不能被风格图覆盖。
- `HORROR_SUSPENSE`：优先 Location、Architecture、Lighting、Composition；威胁是否可见必须由故事和模式规则决定，不由风格图擅自揭示。
- `CINEMATIC_KEYFRAME`：可以提高 Style、Lighting、Composition 的权重，但不能抹除用户指定的身份和道具事实。
