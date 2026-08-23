# Findings & Decisions

## Requirements

- 冻结当前 V0 的对照基线，不修改现有运行逻辑。
- 建立三组 Golden Case：动作、对白、恐怖。
- 统一记录输入、生产目标、结构约束、评分维度和回归规则。
- 后续每次修改至少回归三组案例，避免 Action 优化破坏 Dialogue/Horror。
- 不把尚未执行的图像模型结果描述为实测结果。

## Research Findings

- 当前仓库是 Prompt Skill，主要运行契约在 `SKILL.md`，没有独立代码执行器、测试脚本或模型输出目录。
- 当前 `SKILL.md` 输出契约为四部分：创意简报、逐面板表、整合提示词、负面提示词与质量检查。
- Universal 设计文档将 `Project → Sequence → Scene → Shot → Panel`、Mode Router、Continuity State、Reference Role、Motion DNA 和 Validator 定义为后续目标，但明确本阶段不立即落地。
- 现有 Action Golden Case 已提供 Whoking vs Dao Wang 的完整素材，可作为动作基线来源；对白和恐怖需要以最小、可复现输入补齐。
- 当前工作区除了已配置的 `AGENTS.md` 与 `docs/agents/` 外没有项目代码改动。
- Phase 1 的最小实现应只增加核心规则层，不提前创建所有题材 references；现有 `storyboard-formats.md` 已提供题材节奏模板。

## Technical Decisions

| Decision | Rationale |
|----------|-----------|
| Golden Case 使用独立 Markdown 文件 | 人工可读、便于未来作为 Prompt 输入或迁移到测试工具 |
| 每个案例同时记录“硬约束”和“评分项” | 区分必须满足的结构要求与需要人工判断的质量指标 |
| Action 使用 20 Panel / 5×4 | 与既有 Whoking 案例和 Universal 设计文档一致 |
| Dialogue 使用 8 Panel、Horror 使用 8 Panel | 先覆盖核心行为，不扩大基线资产规模 |
| Validator 在 Phase 0 只定义检查口径 | D009 明确暂不引入脚本 Validator |
| `storyboard-core.md` 作为所有任务的必读核心参考 | 让 Mode Router、Cinematic Function 和输出契约有单一来源 |
| Primary Mode 决定冲突规则，Secondary Mode 只补充兼容约束 | 防止混合模式把 Action 规则错误扩散到 Dialogue/Horror |
| 保持四段默认输出，第四段合并 Negative Prompt 与 Validation Notes | 保留当前调用习惯，同时落地已确认的验证输出 |
| 无参考图时不创建 Reference Map | 避免把不存在的资产和控制关系伪造为事实 |
| 参考图按职责拆分，角色只控制声明的维度 | 防止 Style Reference 覆盖 Identity 或 Location Reference 改写角色外观 |
| 同一职责冲突时优先用户明确指令，无法判断则报告 WARNING | 让冲突可见，不静默选择不可追溯的图像来源 |
| Continuity 按模式和复杂度分级启用 | Action 需要完整状态，Dialogue/Horror 需要关键字段，单张 Keyframe 不应被表格绑架 |
| Scene Geography 先于动作 Panel 设计 | 先固定空间锚点、Camera Axis 和运动方向，减少越轴、瞬移和 Close-up 后失地理的问题 |
| Panel In/Out 是内部状态，不默认完整展示 | 保持用户输出简洁，同时让下一格有可传递的状态依据 |
| Annotation Profile 与画面媒介分离 | Profile 只定义注释语义和复杂度，不把黑白 storyboard 强行变成彩色画面 |
| `ANNOTATION_LEGACY_BLUE` 只显式启用 | 兼容旧 Prompt，但避免蓝色同时承担动作、攻击、摄影机和力线语义 |
| Motion DNA 独立于 Identity Reference | 外观一致和动作语言一致是两种不同约束，避免一张身份图承担动作设计 |
| Action Choreography 以状态转换而非姿势清单为单位 | 用 Cause → Effect 约束动作因果，减少“每格一个酷 Pose” |
| Action 深度规则只在 `ACTION_PREVIS` 启用 | 防止 Action 的持续运动、无 Dead Air 偏好污染 Dialogue/Horror |
| Validator 采用“基础硬校验 + 专业软校验” | 降低无效返工，保留领域规则作为建议和风险提示 |
| 只有明确用户约束冲突才默认 BLOCKER | 用户没有要求的审美或镜头质量不应阻断交付 |

## Issues Encountered

| Issue | Resolution |
|-------|------------|
| 当前没有独立执行器，无法自动采集“V0 实际输出” | 冻结输入与验收协议，并明确输出采集仍需人工调用 Skill 后完成 |

## Resources

- `SKILL.md`
- `docs/UNIVERSAL_STORYBOARD_DESIGN.md`
- `docs/examples/ACTION_PREVIS_WHOKING_VS_DAO_WANG.md`
- `references/shot-language.md`
- `references/storyboard-formats.md`
