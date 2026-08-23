# Universal Storyboard Director — Supplement 02

## Procedural Montage 适用范围扩展与表现法边界

> 文档性质：**补充设计文档（Supplement）**  
> 适用仓库：`twj515895394/storyboard-video-skill`  
> 与既有文档关系：**不修改、不替换主设计文档，也不修改 Supplement 01；本文只追加新的适用范围判断。**  
> 背景：Teleport Device Assembly 是一个优秀 Golden Case，但它的价值不应被缩小为“装配 / 修理 / 制作类模板”。本文重新定义它所代表的更通用 Storyboard Pattern。

---

# 1. 核心修正

不应该把 Teleport Device 示例理解为：

```text
装配 / 修理 / 制作
→ 使用 Procedural Montage
```

更准确的抽象应该是：

> **当一个序列的主要观看乐趣来自“连续、可追踪、具有因果关系的状态推进”，并且需要通过一组高可读性的镜头把这个变化过程压缩成节奏化视觉叙事时，可以使用 Procedural Montage Pattern。**

因此：

```text
PROCEDURAL_MONTAGE != ASSEMBLY_TEMPLATE
```

而应该理解为：

```text
PROCEDURAL_MONTAGE = STATE-PROGRESSION + CAUSE-EFFECT + RHYTHMIC VISUAL COMPRESSION
```

`ASSEMBLY` 只是最直观的 Golden Case 之一。

---

# 2. 判断是否适合的真正标准

一个场景是否适合使用 Procedural Montage，不应该主要看“题材是什么”，而应该看下面几个条件。

## 2.1 是否存在清晰的 Start State → End State

例如：

```text
凌乱 → 整洁
素颜 → 完成妆造
普通人 → 全副武装
冷清店铺 → 营业高峰
原料 → 成品
陌生城市 → 找到目的地
证据碎片 → 拼出真相
身体正常 → 异变完成
普通房间 → 仪式空间
数据原始 → 分析结果
```

只要观众能够感受到：

> “某个东西正在一步一步变成另一个状态”

就具备使用该 Pattern 的基础。

## 2.2 中间过程是否值得被压缩展示

如果过程本身没有视觉价值，只需要一句话交代，那么不需要 Procedural Montage。

如果过程中的：

- 手部操作
- 物体变化
- 工具使用
- 空间变化
- 服装变化
- 信息变化
- 身体状态变化
- 环境变化
- 任务完成度

本身就构成观看乐趣，则适合。

## 2.3 是否存在可追踪的阶段性 State

不一定是机械零件。

State 可以属于：

```text
OBJECT
CHARACTER
BODY
COSTUME
ENVIRONMENT
INFORMATION
RELATIONSHIP
TASK
LOCATION
SYSTEM
ENERGY / EFFECT
```

这意味着 Procedural Montage 的“状态机”不应只理解成 `PROP_STATE`。

长期更通用的概念应该是：

```text
ENTITY_STATE_CONTINUITY
```

其中 `PROP_STATE_CONTINUITY` 是重要子集。

---

# 3. 更广泛的适用场景

以下不是要求都新增为一级 Mode，而是说明 Teleport Golden Case 所代表的表现法可以覆盖远比“装配”更大的范围。

## 3.1 变装 / 准备 / 身份切换

例如：

- 特工执行任务前快速穿戴装备
- 赛车手戴手套、头盔、系安全带、点火
- 新娘妆造过程
- 演员后台上妆、换装、戴假发、候场
- 战士逐件披挂铠甲
- 潜水员准备设备
- 宇航员穿航天服

核心状态：

```text
UNPREPARED
→ PARTIALLY PREPARED
→ READY
```

这类内容的视觉逻辑与 Teleport 非常接近，但目标状态不是“机器组装完成”，而是“人物进入任务状态”。

---

## 3.2 调查 / 解谜 / 信息拼接

例如：

- 侦探把照片、地图、票据、时间线逐步拼到一起
- 黑客从多组数据中定位目标
- 科学家逐步验证假设
- 主角根据线索找到隐藏地点
- 新闻调查逐步建立事件关系

核心状态：

```text
UNKNOWN
→ CLUES ACCUMULATE
→ PATTERN EMERGES
→ UNDERSTANDING / REVEAL
```

这里变化的不是 Prop，而是：

```text
INFORMATION_STATE
```

仍然可以使用：

- Insert
- ECU
- Overhead
- Fast Cut
- Rhythm Curve
- State Progression
- Final Reveal

因此这也是 Procedural Montage Pattern 的重要扩展场景。

---

## 3.3 训练 / 学习 / 能力成长

例如：

- 拳击训练蒙太奇
- 乐器练习
- 舞蹈排练
- 篮球投篮训练
- 学习某项技能
- 机器人通过训练逐步掌握动作

核心状态：

```text
UNSKILLED
→ REPETITION
→ CORRECTION
→ IMPROVEMENT
→ MASTERY / BREAKTHROUGH
```

这类内容不是传统“动作戏”。

它可能有大量身体动作，但真正推进的是：

```text
ABILITY_STATE
```

因此可以借用 Procedural Montage 的：

- Progress Tracking
- Repetition with Variation
- State Milestones
- Rhythm Compression
- Before / After Contrast

需要注意：不能机械重复同一动作，必须让每一组重复带来新的状态或能力变化。

---

## 3.4 医疗 / 救援 / 应急处理

例如：

- 紧急救援流程
- 医护人员准备器材
- 创伤处理的影视化表现
- 灾难现场搭建救援设备
- 消防员进入现场前的准备流程

核心状态：

```text
CRISIS
→ INTERVENTION STEPS
→ STABILIZATION / FAILURE / ESCALATION
```

重点仍是：

```text
PROCESS + STATE CHANGE + TIME PRESSURE
```

这类场景通常与 `HIGH ENERGY` 结合，但依旧不等于 Action Choreography。

---

## 3.5 烹饪 / 调酒 / 咖啡 / 手工艺

这是非常典型但不应被视为唯一代表。

例如：

- 拉面制作
- 鸡尾酒调制
- 手冲咖啡
- 木工作品
- 陶艺
- 锻造
- 绘画
- 胶片冲洗
- 首饰制作

核心状态：

```text
RAW MATERIAL
→ TRANSFORMATION
→ FINISHED RESULT
```

适合大量：

- Macro
- Insert
- Texture
- Tool Contact
- Repeated tactile beats

---

## 3.6 空间 / 环境改造

例如：

- 空房改造成工作室
- 舞台搭建
- 店铺开门前准备
- 节日布置
- 战场布防
- 实验室从关闭状态切换为运行状态
- 露营营地搭建

核心状态：

```text
SPACE_A
→ INTERMEDIATE CONFIGURATIONS
→ SPACE_B
```

State 属于：

```text
ENVIRONMENT_STATE
```

而不是单一 Prop。

---

## 3.7 数字 / 软件 / 信息工作流的视觉化

如果做的是电影化、广告化、概念化表达，也可以使用同一 Pattern。

例如：

- AI 系统从数据输入到结果生成
- 黑客入侵过程
- 数据分析过程
- 卫星锁定目标
- 飞船导航系统校准
- 软件部署/系统上线的视觉化广告

核心状态：

```text
INPUT
→ PROCESS
→ VALIDATION
→ ACTIVATION / RESULT
```

这里要避免变成枯燥 UI 教程。

重点仍然是：

> 把抽象过程转成可读的视觉状态推进。

---

## 3.8 犯罪 / 潜入 / 任务执行中的准备链

例如：

- 盗窃前准备工具
- 潜入前切断监控、复制门禁、进入目标区
- 间谍任务准备
- 越狱计划执行
- 银行劫案电影中的分工蒙太奇

核心状态：

```text
PLAN
→ PRECONDITIONS SATISFIED
→ ACCESS GAINED
→ OBJECTIVE READY
```

它可能与 Suspense、Crime、Action 混合。

这里说明一个重要原则：

> **Procedural Montage 可以是“主 Sequence Logic”，也可以只是其他类型中的局部表现法。**

---

## 3.9 生物 / 身体 / 科幻变异过程

例如：

- 机械义体安装
- 超能力觉醒
- 病毒感染的阶段变化
- 怪物变形
- 仿生人启动
- 机器人自我修复

核心状态：

```text
BODY_STATE_A
→ PHASE_1
→ PHASE_2
→ BODY_STATE_B
```

这里使用的是：

```text
BODY_STATE_CONTINUITY
EFFECT_STATE_CONTINUITY
```

因此 State Engine 的概念必须比 Prop 更广。

---

## 3.10 时间压缩式日常叙事

例如：

- 一个人从起床到出门
- 餐馆从空店到晚高峰
- 办公室从早晨到深夜
- 一个人搬入新家
- 一天的旅行准备
- 摄影师完成一场拍摄

只要重点是：

```text
阶段推进 + 时间压缩 + 可见变化
```

同样可以采用这种 Pattern。

---

# 4. 它不仅是一个“类型”，也是一种可复用 Sequence Pattern

这是本补充最重要的结论。

`PROCEDURAL_MONTAGE` 可以有两种使用方式。

## 4.1 Primary Sequence Logic

整段故事主要靠过程推进。

例如 Teleport：

```text
Broken Device
→ Assembly
→ Activation
→ Consequence
```

此时可以把整段标为：

```text
SEQUENCE_LOGIC: PROCEDURAL_MONTAGE
```

## 4.2 Embedded Sequence Pattern

它也可以嵌入其他类型中。

例如动作电影：

```text
NARRATIVE_DRAMA
→ PROCEDURAL_MONTAGE: 战士穿装备
→ ACTION_CHOREOGRAPHY: 战斗
→ DIALOGUE_PERFORMANCE: 战后对话
```

恐怖片：

```text
HORROR_SUSPENSE
→ PROCEDURAL_MONTAGE: 主角逐步封门、布置防线
→ HORROR_REVEAL
```

犯罪片：

```text
NARRATIVE_DRAMA
→ PROCEDURAL_MONTAGE: 准备盗窃
→ SUSPENSE
→ CHASE
```

广告：

```text
PRODUCT_DEMO
→ PROCEDURAL_MONTAGE: 产品制作 / 使用步骤
→ HERO SHOT
```

因此未来实现上不要把它写成：

```text
if scene == assembly:
    use procedural montage
```

而应该理解成：

> **任何 Sequence 只要其局部逻辑符合“状态推进式视觉压缩”，都可以调用该 Pattern。**

---

# 5. State 概念需要从 PROP_STATE 扩展为 ENTITY_STATE

Supplement 01 强调 `PROP_STATE_CONTINUITY` 是正确的，因为 Teleport Golden Case 最突出的是设备状态。

但从更广的适用场景看，长期概念可以扩展为：

```text
ENTITY_STATE_CONTINUITY
```

子类型可以包括：

```text
PROP_STATE
CHARACTER_STATE
BODY_STATE
COSTUME_STATE
ENVIRONMENT_STATE
INFORMATION_STATE
TASK_STATE
LOCATION_STATE
SYSTEM_STATE
EFFECT_STATE
RELATIONSHIP_STATE
```

注意：

> 这不是要求当前实现立刻新增一套复杂 State Engine。

当前已经在实现的 `PROP_STATE` 可以继续作为第一阶段。

本补充只记录未来扩展方向，避免把核心设计错误地锁死在“物体装配”上。

---

# 6. Teleport 示例真正值得复用的是“表现法”，不是题材

该示例真正优秀的可迁移能力包括：

```text
CLEAR START / END STATE
CAUSE → EFFECT SHOT CHAIN
ONE EXTRACTABLE MOMENT PER PANEL
HIGHLY READABLE INSERTS
RHYTHM CURVE
ESCALATION CURVE
STATE TRACKING
CAMERA VARIETY WITH PURPOSE
INTERRUPTION BEAT
FINAL CONSEQUENCE
CLEAN PANEL ARTWORK
EXTERNAL PRODUCTION METADATA
```

这些能力可以迁移到大量非装配内容。

例如“侦探拼出真相”：

```text
P01 blank board
P02 first photo
P03 map location
P04 timestamp
P05 phone record
P06 conflicting clue
P07 remove false lead
P08 connect two suspects
P09 missing gap
P10 final evidence
P11 pattern reveal
P12 realization CU
```

此时：

```text
STATE
```

不再表示机器零件，而表示：

```text
knowledge / evidence completeness
```

但 Teleport 的 Storyboard Grammar 依旧成立。

---

# 7. 什么时候不应该使用这种方式

为了避免它成为万能锤子，也必须明确边界。

以下场景如果“过程本身不重要”，就不应该强行使用 Procedural Montage。

## 7.1 纯情感停顿

例如：

- 两个人沉默对视
- 得知亲人离世后的反应
- 分手前无法开口

这类场景依赖表演、停顿、细微反应，而不是状态步骤压缩。

## 7.2 纯空间探索

如果重点是环境氛围和发现，而不是逐步完成任务，可能更适合 Narrative / Suspense / Exploration。

## 7.3 对话权力变化

虽然“关系状态”会变化，但如果真正的观看重点是台词和表演，不应为了 State 模型切成大量 Insert。

## 7.4 连续动作表演

例如武术、舞蹈中一个完整连续动作链，如果拆成过度碎片化的 Procedural Insert，反而会破坏身体运动流。

因此：

> **有状态变化，不代表必须使用 Procedural Montage。必须同时满足“过程值得展示”和“压缩后的步骤可构成观看节奏”。**

---

# 8. 建议的适用性判断器

未来 Skill 可以内部进行简单判断：

```text
Q1. 是否存在明确 Start State / End State？
Q2. 中间状态是否可视觉化？
Q3. 步骤之间是否存在 Cause → Effect？
Q4. 过程本身是否构成观看价值？
Q5. 是否适合通过多镜头压缩时间？
Q6. 是否需要观众理解“进度 / 变化 / 完成度”？
```

如果大多数为 Yes：

```text
Procedural Montage Pattern = Strong Candidate
```

如果只满足 1-2 项：

```text
Do not force Procedural Montage
```

---

# 9. 与其他 Sequence Logic 的组合关系

Procedural Montage 不应该被设计成互斥类型。

常见组合：

```text
PROCEDURAL_MONTAGE + ACTION
PROCEDURAL_MONTAGE + SUSPENSE
PROCEDURAL_MONTAGE + COMEDY
PROCEDURAL_MONTAGE + PRODUCT_DEMO
PROCEDURAL_MONTAGE + NARRATIVE_DRAMA
PROCEDURAL_MONTAGE + HORROR
PROCEDURAL_MONTAGE + MV / MUSIC RHYTHM
```

例如：

### Procedural + Suspense

拆炸弹倒计时、紧急修复飞船、黑客突破最后一道防线。

### Procedural + Comedy

角色自信准备一顿大餐，但每个步骤都越来越失控。

### Procedural + Horror

主角逐个锁门、封窗、布置摄像头，同时背景中威胁逐步接近。

### Procedural + Product Demo

通过用户操作，让产品功能逐步显现。

### Procedural + MV

把化妆、穿衣、舞台准备等过程按音乐节拍组织。

因此未来 Router 更理想的能力不是只输出一个标签，而是能够识别：

```text
PRIMARY LOGIC
+
SECONDARY PATTERN
```

但该能力仍属于后续增强，不要求立即重构当前实现。

---

# 10. 对 Golden Case B 的重新定位

Teleport Device Assembly 应继续作为 Golden Case B，但测试目标应扩大。

它不只是验证：

```text
能不能画好装机器
```

而应该验证 Skill 是否具备：

```text
1. 识别状态推进式叙事
2. 建立 Start → End State
3. 设计中间可读阶段
4. 保持 Cause → Effect
5. 用镜头节奏压缩过程
6. 维护状态连续性
7. 插入 interruption / reversal
8. 让完成结果产生新的 narrative consequence
9. 将 Production Metadata 与 Panel Artwork 分离
10. 保证 One Panel = One Extractable Moment
```

只要这 10 项可以泛化，Teleport 就真正成为“通用能力 Golden Case”，而不是“装配模板样例”。

---

# 11. 推荐的术语调整

后续讨论中避免说：

> “装配/修理/制作过程使用 Teleport 模式。”

更推荐说：

> **对于以可追踪状态推进为主要观看逻辑、适合通过多镜头进行时间压缩的 Sequence，可以采用 Procedural Montage Pattern。Teleport Device Assembly 是这一 Pattern 的 Golden Case。**

这样既保留了具体案例，又不会把能力范围做小。

---

# 12. 本补充文档最终结论

Teleport 示例的真正价值不是：

```text
教 Skill 如何生成装配故事板
```

而是：

```text
教 Skill 如何把“一个逐步发生变化的过程”
转换成
“具有镜头可读性、节奏曲线、状态连续性和因果推进的视觉 Sequence”。
```

它可以应用于：

- 装配与修理
- 准备与变装
- 调查与解谜
- 训练与成长
- 医疗与救援
- 烹饪与手工
- 空间改造
- 数字工作流视觉化
- 潜入与任务准备
- 身体 / 科幻转化
- 时间压缩式日常叙事
- 以及其他任何满足“状态推进 + 因果步骤 + 视觉压缩”条件的内容

最终应坚持：

> **不要按题材套模板，要按 Sequence 的底层推进逻辑选择表现法。**

这也是 Universal Storyboard Director 与普通 Storyboard Prompt Template 的核心区别之一。
