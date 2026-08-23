# Findings

## 2026-08-23

- 当前分支为 `main`，HEAD 与远程同步到 `84f8abd`；两个 Procedural Montage supplement 已在提交历史中。
- V1 规则层主体改动仍在主工作区未提交，涉及 `SKILL.md`、README、设计文档及多份 references。
- V2 spec 位于 `.scratch/v2-procedural-montage/spec.md`，9 个 ticket 均已建立且尚未勾选完成项。
- V2 依赖关系为 `01–06 → 07 → 08 → 09`；本轮按用户要求先做实现，暂缓 09 和统一图像回归。
- 当前核心 references 尚未正式实现 V2 的 `PROCEDURAL_MONTAGE`、独立 Validator、Style 双层、CLEAN_EXTERNAL_METADATA、Rhythm Curve 和 Prop/Process Continuity。
- `SKILL.md` 与 `storyboard-core.md` 是两条工作流的高冲突文件；Worker 使用隔离 worktree，主线程负责顺序整合。

