---
name: gitee-push
description: 将当前项目的 Git 变更提交并推送到 Gitee 远程仓库。Use when the user asks to 提交代码, 推送项目, git push, 提交到码云, or 把项目提交上去. Do not use for Git 操作以外的任务.
---

# Gitee 提交推送

将当前项目完整地 add → commit → push 到 Gitee 远程仓库。

## 远程仓库信息

- 远程地址：`https://gitee.com/chenyiran703/chenyiran703.git`
- 远程名称：`origin`
- 主分支：`main`
- 工作目录：`d:\my_web`

## 执行步骤

### 1. 检查当前状态

```powershell
git status --short
git branch --show-current
git remote -v
```

确认有变更需要提交，且远程 origin 指向正确的 Gitee 地址。

### 2. 暂存所有变更

```powershell
git add -A
```

不要忽略已修改、已删除或未跟踪的文件。如果有不应提交的产物文件，应先加入 `.gitignore` 而非跳过暂存。

### 3. 生成提交信息

根据暂存区的变更内容，用中文生成清晰的提交信息。例如：
- `更新 14-1 页面`
- `新增 CSS 过渡动画示例`
- `修复首页导航链接`

获取变更摘要辅助生成信息：

```powershell
git diff --cached --stat
```

### 4. 执行提交（闭环关键）

**禁止**创建额外的 `commit_msg.txt` 或任何临时文件来传递提交信息。直接使用 `-m` 参数。

```powershell
git commit -m "<生成的中文提交信息>"
```

如果提示 "nothing to commit"，说明暂存区为空，回到步骤 2 检查是否漏了文件。

### 5. 验证提交成功（基于 Git 事实状态）

不要依赖终端回显或 exit code 判断。执行以下任一命令确认 HEAD 已前进：

```powershell
git log --oneline -1
git rev-parse HEAD
```

确认输出显示的最新提交的 message 和 hash 与预期一致。

### 6. 推送到 Gitee

```powershell
git push origin main
```

### 7. 验证推送成功

检查输出中是否包含 `* [new branch]` 或 `To https://gitee.com/chenyiran703/chenyiran703.git` 且无 `rejected`、`error`、`!` 等失败标记。

也可拉取远程 refs 确认：

```powershell
git ls-remote origin refs/heads/main
```

## 冲突处理

如果 push 被拒绝（non-fast-forward），执行：

```powershell
git pull --rebase origin main
git push origin main
```

## Windows PowerShell 注意事项

- 不要使用 `&&` 或 `||` 链接多个命令（PowerShell 5 不支持），用分号或分行执行
- 不要使用 `cmd /c` 或 `cmd.exe`，全部使用 PowerShell 原生命令
- 避免分页器卡住：执行 Git 命令前可设置 `$env:GIT_PAGER = "cat"`
