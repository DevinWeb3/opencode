# Fork 工作流

本仓库是 `anomalyco/opencode` 的 fork，用于二次开发并持续同步官方更新。

## Remote 结构

| remote   | 仓库                                  | 用途                  |
| -------- | ------------------------------------- | --------------------- |
| `origin` | `git@github.com:DevinWeb3/opencode.git` | 你的 fork，推送自己的改动 |
| `upstream` | `https://github.com/anomalyco/opencode.git` | 官方仓库，只拉更新    |

## 日常开发

- 所有改动在 `clt` 分支上进行，提交后 `git push`（推到 `origin`，即你的 fork）。
- 不要在 `dev` 分支开发；`dev` 留作上游镜像，方便对比和 rebase。
- 官方默认分支是 `dev`，不是 `main`；本地可能没有 `main`。

## 同步官方更新（Merge 方式）

在 `clt` 分支上执行：

```bash
git fetch upstream --prune
git merge upstream/dev
# 若有冲突：解决冲突后 git add <文件> && git commit，再 git push
git push origin clt
```

- 选 Merge 是因为 `clt` 已推送到远端，Rebase 会改写历史并需要 force push；Merge 保留分叉历史，团队协作更安全。
- 上游偶尔会 force-push 或重写历史，`--prune` 会清理已删除的远端分支引用。

## 回馈官方 PR

向官方提 PR 时，不要带上 fork 专属的提交：

1. 从 `clt` 切一个干净的临时分支，仅 cherry-pick 要贡献的提交（排除下面「Fork 专属文件」的改动）。
2. 推到你的 fork：`git push origin <临时分支>`。
3. 创建 PR：
   ```bash
   gh pr create --repo anomalyco/opencode --base dev --head <临时分支>
   ```

## Fork 专属文件

以下文件是 fork 本地维护的，提官方 PR 时务必排除其提交：

- `FORK-WORKFLOW.md`（本文件）
- `AGENTS.md` 顶部的 fork 说明段落

## 快速参考

```bash
# 同步上游
git fetch upstream --prune && git merge upstream/dev && git push

# 查看落后官方多少
git fetch upstream && git log --oneline clt..upstream/dev

# 仅 diff 官方某分支
git diff upstream/dev -- <path>
```
