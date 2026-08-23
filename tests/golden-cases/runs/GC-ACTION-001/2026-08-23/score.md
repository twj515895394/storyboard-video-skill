# GC-ACTION-001 实测评分

评分规则：`0` 未满足，`1` 部分满足，`2` 清晰满足。评分基于三张生成图的人工视觉检查，不代表自动化模型评测。

## 总体评分

| 维度 | A 原始 Legacy | B Universal Legacy | C Universal PRO |
|---|---:|---:|---:|
| Story Clarity | 2 | 2 | 2 |
| Shot Necessity | 1 | 2 | 2 |
| Shot Variety | 2 | 2 | 2 |
| Continuity | 1 | 2 | 2 |
| Mode Fidelity | 2 | 2 | 2 |
| Character / Subject Fidelity | 2 | 2 | 2 |
| Prompt Consistency | 1 | 2 | 2 |
| Final Panel Strength | 2 | 2 | 2 |
| **平均分** | **1.63** | **2.00** | **2.00** |

## Action-specific 指标

| 指标 | A 原始 Legacy | B Universal Legacy | C Universal PRO |
|---|---:|---:|---:|
| Combat Geography | 1 | 2 | 2 |
| Cause → Effect Readability | 1 | 2 | 2 |
| Motion DNA Fidelity | 2 | 2 | 2 |
| Active Wide Reset Quality | 1 | 1 | 2 |
| Repeated Pose / Dead Air Risk | 1 | 2 | 2 |
| Ending Action Silhouette | 2 | 2 | 2 |

## Observations

### A：Original / Legacy

- 优点：20 格结构稳定，Whoking 与 Dao Wang 的外观和大体动作风格可区分，动作密度和结尾结果清楚。
- 问题：蓝箭头同时表达主体运动、力线和摄影机运动；没有稳定的 Continuity Header，空间关系主要依赖画面本身；动作因果比 B/C 更像连续姿势。
- 结论：适合作为原始对照组，不适合作为 Universal 默认注释规范。

### B：Universal + Legacy

- 优点：Project Card、Continuity Header、Camera Axis、动作阶段和角色动作语言让动作链更容易扫描；角色恢复和最终喉线停手更清楚。
- 问题：Legacy 蓝箭头仍然混合多种语义；虽然兼容旧 Prompt，但不适合作为新任务的默认 Profile。
- 结论：证明 Director Plan 结构有价值，同时保留 Legacy 作为兼容层是合理的。

### C：Universal + PRO

- 优点：红色主体动作、蓝色摄影机、绿色构图和紫色节奏的语义分离最清楚；第 19 格的 Active Wide Reset 能恢复庭院地理，第 20 格的结果和剪影最明确。
- 问题：注释层更复杂，普通故事板不应默认加载全部颜色；实际模型仍需更多案例验证是否能稳定遵循颜色语义。
- 结论：当前三组中最适合作为 Action Previs 推荐 Profile，但仍需要 Dialogue/Horror 回归确认不会过度动作化。

## Decision

暂定：`ACTION_PREVIS` 使用 `ANNOTATION_PRO` 作为默认推荐；`ANNOTATION_LEGACY_BLUE` 仅用于复现旧 Prompt 或兼容性实验。该结论来自单次对照，待后续回归后再固化为最终行为。
