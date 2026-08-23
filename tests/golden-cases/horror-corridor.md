# GC-HORROR-001：没有人的脚步声

## Case Card

- **Mode:** `HORROR_SUSPENSE`
- **Production target:** 16:9 单张 Storyboard Sheet，8 Panel，4×2 网格，黑白粗糙恐怖预演，保留大块负空间
- **Primary risk:** Offscreen Pressure、Negative Space、False Calm、Reveal Timing、静止镜头的有效性
- **Expected annotation:** `ANNOTATION_CLEAN`；只在确有必要时使用少量 `ANNOTATION_SIMPLE`

## Frozen Input

```text
请把下面场景做成 16:9、8 格、4×2 网格的黑白粗糙恐怖分镜，模式为 HORROR_SUSPENSE。

凌晨，一名护士回到已经停用的医院取落在值班室的手机。走廊只有一排忽明忽暗的顶灯，电梯门停在半开状态，远处没有人。她经过护士站时，看见地面有一串湿脚印从她身后延伸到前方，但她听不到脚步声。她停下，回头，走廊空无一物；顶灯恢复稳定，像什么都没发生。她继续走，手机在值班室里响起，屏幕显示来电者正是她自己。最后一格不要展示怪物，只让半开的电梯门内出现一小块比走廊更黑的空间。

要求：先建立普通空间，再植入错误细节，保持威胁大部分在画外，通过停顿、负空间、限制视线和假平静延迟揭示。不要把静止或等待判定为无效动作，不要提前展示实体怪物。
```

## Hard Constraints

- [ ] 必须生成 8 个 Panel，布局为 4×2。
- [ ] 必须先建立普通医院走廊，再逐步引入湿脚印、无声回头、异常来电和电梯黑区。
- [ ] 威胁在前 7 格不得被完整展示；恐怖来自画外压力和空间异常。
- [ ] 至少有一个有功能的静止/等待格、一个 False Calm 和一个延迟揭示。
- [ ] 走廊、电梯、护士站和值班室的空间关系必须连续，不得瞬移。
- [ ] Negative Space、受限可见性和视线方向必须参与构图，而不只是写在说明里。
- [ ] 不得把 Horror 规则套成 Action：不能要求每格都有明显身体动作或连续加速。
- [ ] 最后一格保持未解决状态，只展示半开电梯门内的异常黑区，不展示怪物。

## Human Review Prompts

- 第一格是否足够普通，能够支撑后续“错误细节”的反差？
- 湿脚印是否改变了观众对空间的判断，而不是只作为装饰？
- 静止格是否提高压力？如果删除它，节奏是否会变弱？
- 最后一格是否留下明确但未解释的威胁？
- 是否因为追求镜头变化而破坏了恐怖的延迟和负空间？

## Minimum Regression Score

`Story Clarity`、`Continuity`、`Mode Fidelity`、`Final Panel Strength` 四项不得低于 2；`Shot Variety` 不得为了丰富而牺牲 `Reveal Timing`。
