---
name: github-auto-push
description: 每当完成或修改一个文件后，自动将项目变更提交并推送到 GitHub 远程仓库。Use when 每完成或修改一个文件, 提交到 GitHub, push 到 GitHub, or 同步到 chenyiran-cell/chenyiran. Do not use for 推送到 Gitee（用 gitee-push）或非 Git 任务.
---

# GitHub 自动提交推送

每完成或修改一个文件后，将当前项目 add → commit → push 到 GitHub 远程仓库。

## 远程仓库信息

- 远程地址：`https://github.com/chenyiran-cell/chenyiran.git`
- 远程名称：`github`（注意：`origin` 已被 Gitee 占用，**禁止**改动 origin）
- 主分支：`main`
- 工作目录：`d:\my_web`

## 首次配置（仅当 github 远程不存在时）

```powershell
git remote -v
```

若输出中没有 `github`，执行：

```powershell
git remote add github https://github.com/chenyiran-cell/chenyiran.git
```

## 执行步骤

### 1. 检查当前状态

```powershell
$env:GIT_PAGER = "cat"
git status --short
```

无变更则直接报告"工作区干净，无需提交"，不要创建空提交。

### 2. 暂存所有变更

```powershell
git add -A
```

### 3. 生成提交信息

根据刚完成/修改的文件内容，用中文生成简洁的提交信息，例如 `新增 15-1 数7游戏页面`、`修改 14-2 数据类型演示`。

```powershell
git diff --cached --stat
```

### 4. 执行提交（闭环关键）

**禁止**创建 `commit_msg.txt` 等临时文件传递提交信息，直接用 `-m` 参数：

```powershell
git commit -m "<生成的中文提交信息>"
```

### 5. 验证提交成功（基于 Git 事实状态）

不依赖终端回显判断，必须执行：

```powershell
git log --oneline -1
```

确认 HEAD 已前进且 message 与预期一致。

### 6. 推送到 GitHub

```powershell
git push github main
```

首次推送使用 `git push -u github main` 建立跟踪。

### 7. 验证推送成功

```powershell
git ls-remote github refs/heads/main
```

确认远程 hash 与本地 `git rev-parse HEAD` 一致。

## 冲突处理

push 被拒绝（non-fast-forward）时：

```powershell
git pull --rebase github main
git push github main
```

出现冲突文件时，先解决冲突、`git add` 后继续 `git rebase --continue`。

## 认证失败处理

HTTPS 推送若报认证错误，说明缺少 GitHub 凭据，不要反复重试。可改用本机已有的 SSH 密钥：

```powershell
git remote set-url github git@github.com:chenyiran-cell/chenyiran.git
git push github main
```

## Windows PowerShell 注意事项

- 不要使用 `&&` 或 `||` 链接命令，用分号或分行执行
- 不要使用 `cmd /c`，全部使用 PowerShell 原生命令
- 执行 Git 命令前设置 `$env:GIT_PAGER = "cat"` 防止分页器卡住
