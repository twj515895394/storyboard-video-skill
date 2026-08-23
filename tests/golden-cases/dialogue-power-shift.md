# GC-DIALOGUE-001：厨房里的录音笔

## Case Card

- **Mode:** `DIALOGUE`
- **Production target:** 16:9 单张 Storyboard Sheet，8 Panel，4×2 网格，粗糙电影分镜风格，低频摄影机运动
- **Primary risk:** Eyeline、180 度轴线、Speaker/Listener Reaction、道具连续性、沉默的电影功能
- **Expected annotation:** `ANNOTATION_SIMPLE` 或 `ANNOTATION_CLEAN`

## Frozen Input

```text
请把下面场景做成 16:9、8 格、4×2 网格的粗糙电影对白分镜，模式为 DIALOGUE。

深夜，老公寓狭窄厨房。女儿林岚把一支录音笔放到餐桌中央，要求父亲承认他隐瞒了母亲病情的事实。父亲先背对她洗杯子，装作没有听见；林岚打开录音笔，播放一段母亲的声音。父亲停止动作但不回头。林岚起身绕到桌边，第一次进入父亲的正面视线，问“现在还要继续装吗？”父亲拿起录音笔想关掉，却在最后一刻把手收回，承认自己害怕。结尾停在父女隔着录音笔对坐，关系从控制转为共同面对。

要求：先建立厨房空间和两人位置，再通过说话者、听者反应、录音笔插入、沉默停顿和一次权力转移推进节奏。保持视线方向与屏幕方向连续，不要为了镜头丰富而频繁移动摄影机。最后一格要读出关系变化。
```

## Hard Constraints

- [ ] 必须生成 8 个 Panel，布局为 4×2。
- [ ] 前段必须建立厨房空间、父女位置和基本视线关系。
- [ ] 录音笔必须在相关 Panel 中保持位置与状态连续，不得凭空消失或换位。
- [ ] 至少包含 Speaker、Listener Reaction、道具 Insert、Silence/Hold 和 Power Shift。
- [ ] 默认保持 180 度轴线；如果越轴，必须明确把它作为权力关系变化而不是无意断轴。
- [ ] 沉默格必须有功能，不能被 Validator 当作 Action 模式的 Dead Air 删除。
- [ ] 摄影机运动应克制，不能用随机运动掩盖对白和反应逻辑。
- [ ] 最后一格必须表达父女关系已经从对抗转为共同面对。

## Human Review Prompts

- 即使去掉对白文字，观众能否看懂谁在施压、谁在回避、谁发生了变化？
- 父亲“不回头”和“收回关录音笔的手”是否形成递进？
- 录音笔是否承担了信息和关系功能，而不只是装饰？
- 沉默是否改变了权力关系或观众期待？
- 是否错误套用了 Action Previs 的连续动作规则？

## Minimum Regression Score

`Story Clarity`、`Continuity`、`Mode Fidelity`、`Final Panel Strength` 四项不得低于 2；`Shot Variety` 可以为 1，只要摄影机克制且服务对白。
