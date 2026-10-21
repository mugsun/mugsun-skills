---
name: mugsun-commit
description: 在 mugsun 各仓库提交和推送代码：写 Conventional Commits 提交信息、更新 CHANGELOG、检查迁移和分支规则。当用户说「提交」「写提交信息」「commit」「推上去」「更新 CHANGELOG」时使用。
---

# mugsun-commit：提交与推送

## 触发词

提交、commit、写提交信息、推送、push、更新 CHANGELOG、发版前整理。

## 必问参数

1. **仓库和分支**：在哪个仓库、哪个本地分支上提交。
2. **改动是否已通过审查**：普通代码要过一次代码审查，敏感改动还要过安全审查；只改文档、文案、样式、CHANGELOG 的，CI 绿即可。
3. **是否要写 CHANGELOG**：用户可见的变化都要写。

## 默认值

- 提交信息用 Conventional Commits：`<type>(<scope>): <主题>`，type 和 scope 用英文小写，主题用中文，不超过 50 字，句末不加句号，不用 emoji。
- type 取值：`feat`、`fix`、`perf`、`refactor`、`docs`、`test`、`build`、`ci`、`chore`、`revert`。破坏性变更在 type 后加 `!`，脚注写 `BREAKING CHANGE:`。
- 一个提交只做一件事，不相关的改动拆开。
- CHANGELOG 写在 `## [Unreleased]` 下对应分组里，**每行不超过 80 字**。
- **推送**：只有分支最新的 commit 已经在分支上跑过同名必需检查、并且全绿，才可以快进推入 main。推送前用 `gh run list --commit <sha>` 自己确认。不要 force push。

## 不适用于

- 已经发布的提交历史的改写：不 force 推送，不 rebase 已推送的提交，不 amend 已推送的提交。
- 修改已发布的 Flyway 迁移：一律新增迁移。
- 删除或下线功能：先问项目负责人。

## 步骤

1. `git status` 和 `git diff --staged` 确认要提交的内容，不要带上无关文件（锁文件、日志、本地配置）。
2. **不要用 `git stash`**。需要切换工作时，另建一个 `git worktree`。如果提交钩子会自动建 stash（如 lint-staged 默认行为），改用 `lint-staged --no-stash`。
3. 如果改了数据库：确认没有动已发布清单里的迁移；新迁移三套方言同号；本地跑一次已发布迁移校验脚本。
4. 更新 CHANGELOG：每行不超过 80 字，写清用户能看到的变化，不复述 diff。
5. 写提交信息并提交，跑一遍该仓库的测试命令。
6. 推送：**只有分支最新的 commit 已经在分支上跑过同名必需检查、并且全绿，才可以快进推入 main。** 推送前用 `gh run list --commit <sha>` 自己确认。远端已经前进时，先合并 `origin/main`、等新的 HEAD 再跑绿，再推。**不要 force push。**

## 产出

- 本地提交的 SHA 和提交信息。
- CHANGELOG 改动（如有）。
- 推送结果，或说明为什么还不能推（等审查、CI 未绿）。
