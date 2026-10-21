---
name: mugsun-commit
description: 在 mugsun 各仓库提交和推送代码：写 Conventional Commits 提交信息、更新 CHANGELOG、检查迁移和分支规则。当用户说「提交」「写提交信息」「commit」「推上去」「更新 CHANGELOG」时使用。
---

# mugsun-commit：提交与推送

## 触发词

提交、commit、写提交信息、推送、push、更新 CHANGELOG、发版前整理。

## 必问参数

1. 仓库和分支：在哪个仓库、哪个本地分支上提交。
2. 改动是否已通过审查：普通代码要过一次代码审查，敏感改动还要过安全审查；只改文档、文案、样式、CHANGELOG 的，CI 绿即可（安全相关文档除外，先给安全官看）。
3. 是否要写 CHANGELOG：用户可见的变化都要写。

## 默认值

- 提交信息用 Conventional Commits：`<type>(<scope>): <主题>`，type 和 scope 用英文小写，主题用中文，不超过 50 字，句末不加句号，不用 emoji。
- type 取值：`feat`、`fix`、`perf`、`refactor`、`docs`、`test`、`build`、`ci`、`chore`、`revert`。破坏性变更在 type 后加 `!`，脚注写 `BREAKING CHANGE:`。
- 一个提交只做一件事，不相关的改动拆开。
- 提交身份：作者和提交者用 `mugsun <bydongxin@gmail.com>`，不再强制 noreply。用 `git -c user.name=mugsun -c user.email=bydongxin@gmail.com commit`，或者在自己的 worktree 里设仓库级 config。不要改机器的全局 git 配置；已经推送的历史不改写。身份不符不判阻塞，审查时仅提示。
- CHANGELOG 写在 `## [Unreleased]` 下对应分组里，每行不超过 80 字。
- 推送：先普通推送到同名远端分支，用 `gh run list --commit <sha>` 确认该 SHA 的同名必需检查全绿，再快进到 main。main 已前移时先 merge `origin/main`，merge 出的新 SHA 也要先在分支上跑绿。合入后删掉远端分支。禁止 force push，禁止 `--no-verify`。

## 不适用于

- 已经发布的提交历史的改写：不 force 推送，不 rebase 已推送的提交，不 amend 已推送的提交，不用 `--no-verify` 跳过钩子。
- 修改已发布的 Flyway 迁移：一律新增迁移。
- 删除或下线功能：先问项目负责人。

## 步骤

1. `git status` 和 `git diff --staged` 确认要提交的内容，不要带上无关文件（锁文件、日志、本地配置）。
2. 不要用 `git stash`。需要切换工作时，另建一个 `git worktree`。如果提交钩子会自动建 stash（如 lint-staged 默认行为），改用 `lint-staged --no-stash`。
3. 如果改了数据库：确认没有动已发布清单里的迁移；新迁移三套方言同号；本地跑一次已发布迁移校验脚本。
4. 更新 CHANGELOG：每行不超过 80 字，写清用户能看到的变化，不复述 diff。
5. 写提交信息并提交，跑一遍该仓库的测试命令。
6. 推送前用 `git log --format='%h %an <%ae> | %cn <%ce>' origin/main..HEAD` 确认身份是 `mugsun <bydongxin@gmail.com>`；不是的话，还没推送时可以用这个身份重做，已推送的不改。
7. 推送：先普通推送到同名远端分支，用 `gh run list --commit <sha>` 确认同名必需检查全绿，再快进到 main。main 已前移时先 merge `origin/main`，新 SHA 也要先在分支上跑绿。合入后删掉远端分支。禁止 force push，禁止 `--no-verify`。

## 产出

- 本地提交的 SHA 和提交信息。
- CHANGELOG 改动（如有）。
- 推送结果，或说明为什么还不能推（等审查、CI 未绿）。
