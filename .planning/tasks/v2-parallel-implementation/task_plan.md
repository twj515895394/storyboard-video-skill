# V1 收尾与 V2 实现并行计划

**目标：** 在隔离工作区中并行完成 V1 规则层收尾与 V2 Ticket 实现，主线程完成结果审查、顺序合并，并在全部实现结束后统一执行回归测试。

**架构：** Worker A 负责 V1 Phase 20/21 的规则层收尾；Worker B 负责 V2 `01–08`，按既有依赖推进。两者分别使用隔离 worktree，避免同时覆盖主工作区；主线程先审查并整合 V1，再将 V2 结果按依赖顺序整合。测试阶段固定为所有实现合并后的单独阶段。

**技术栈：** 纯 Markdown Skill、Git worktree、`rg`、`git diff --check`；本阶段不引入执行器、JSON Schema 或自动化 Validator。

---

## 阶段

### Phase 1: 并行任务准备

- [ ] 确认当前项目与工作树基线
- [ ] 建立 V1 Worker 和 V2 Worker 的明确写入边界、验收标准与回传格式
- [x] 创建两个隔离 Worker
- [x] 记录两个 Worker 的启动失败原因：V1 为 CLI Proxy xAI 认证不可用，V2 为服务高负载
- [x] 由主线程接管实现，取消并行工作区合并路径
- **Status:** complete

### Phase 2: V1 规则层收尾（主线程接管）

- [ ] 完成 Soft Validator 独立 reference 与统一严重级别
- [ ] 将 Core、Continuity、Geography、Annotation、Motion DNA、Action 规则接入统一 Validator
- [ ] 更新主 Skill 的校验工作流
- [x] 按用户最新决定，将 V1 回归范围调整为 2–3 个既有 Golden Case，不补 10 条矩阵
- [ ] 完成静态引用、Markdown 和差异自检
- [x] Worker 结果已审查并移植到主工作区
- **Status:** complete

### Phase 3: V2 Ticket 实现（主线程接管）

- [x] 完成 Ticket 01–06：Validator、Single-Moment、Style 双层、CLEAN_EXTERNAL_METADATA、Rhythm Curve、Prop/Process Continuity
- [x] 完成 Ticket 07：Procedural Montage reference、Mode Router 与嵌入 Pattern
- [x] 完成 Ticket 08：Teleport Device Assembly Golden Case 与文档索引同步
- [x] 保持 V1 Action、Dialogue、Horror 模式规则边界
- [x] 完成静态引用、Markdown 和差异自检
- **Status:** complete

### Phase 4: 主线程审查与合并

- [x] 审查 Worker A 的 diff、文件边界、规则一致性和交接报告
- [x] 先整合 V1 结果，保留用户现有未提交改动，不执行破坏性清理
- [x] 审查 Worker B 的 diff，处理与 V1 的核心文件冲突
- [x] 按 V2 Ticket 依赖顺序整合 01–08
- [x] 更新规划文件、ticket 状态和必要的 Changelog/summary
- **Status:** complete

### Phase 5: 统一回归测试（用户指定延后）

- [ ] V1 新增 Validator 规则：选择 2–3 个既有案例（优先 Action、Dialogue、Horror）生成并评分
- [ ] V2 新增能力：选择 2–3 个案例（至少包含 Procedural Montage，并包含一个既有模式边界案例）生成并评分
- [ ] 检查核心指标和跨模式污染
- [ ] 不执行 10 条用例矩阵；以用户确认的 2–3 个/版本为准
- **Status:** pending

## 合并规则

1. Worker 只在自己的隔离 worktree 中修改，不直接写入主工作区。
2. Worker A 与 Worker B 都不得创建新的后台任务、线程或子 Agent。
3. 主线程负责最终采纳、冲突解决、提交与测试；Worker 的结论不自动视为通过。
4. 不在实现阶段执行 Golden Case 图像回归；只允许必要的静态完整性检查。
5. 不执行 `git reset --hard`、`git clean` 或其他破坏性清理。

## 当前风险

- V1 与 V2 都可能修改 `SKILL.md`、`references/storyboard-core.md` 和设计文档；必须以 V1 结果为基线审查 V2。
- V2 Ticket 01 与 V1 Phase 20 有职责重叠，合并时以 V2 spec 的统一 Validator 定义为准，避免重复创建两个 Validator 文件。
- 当前主工作区已有未提交 V1 改动，Worker 分支的基线与主工作区差异需要在合并前核对。

## 委派失败记录

| Worker | 结果 | 原因 | 处理 |
|---|---|---|---|
| V1 规则层收尾 | systemError | `auth_unavailable: no auth available (providers=xai, model=grok-4.5)` | 主线程接管 |
| V2 Procedural Montage | systemError | 服务高负载 | 主线程接管 |
