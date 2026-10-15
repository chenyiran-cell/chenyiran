---
name: Gitee Push
description: 提交项目到 Gitee 仓库（git add / commit / push）
---

## 目标仓库

- 远程地址：`https://gitee.com/chenyiran703/chenyiran703.git`
- 平台：Gitee（码云）

## 工作流程

1. **检查状态**：运行 `git status` 查看变更文件。
2. **暂存变更**：运行 `git add -A` 暂存所有变更（或按需指定文件）。
3. **提交**：运行 `git commit -m "<提交信息>"`，提交信息用中文简述本次变更内容。
4. **推送**：运行 `git push origin main`（若主分支为 master 则用 `master`）。
5. **确认结果**：检查 push 输出，确认成功推送到 Gitee。

## 首次使用（仓库未初始化时）

```bash
git init
git add -A
git commit -m "初始提交"
git branch -M main
git remote add origin https://gitee.com/chenyiran703/chenyiran703.git
git push -u origin main
```

## 注意事项

- 推送前确保本地分支与远程分支一致，如有冲突先 `git pull --rebase`。
- 提交信息应清晰描述变更内容，便于版本追溯。
- 若推送需要认证，使用 Gitee 账号密码或 SSH 密钥。
