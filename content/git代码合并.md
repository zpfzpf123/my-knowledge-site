---
title: "git代码合并"
date: 2025-08-13
tags:
  - Git
---

# Git 协作冲突处理指南（拉取、合并、上传）

## 目录
1. 分支协作规则  
2. 标准流程（推荐：功能分支）  
3. 冲突解决步骤  
4. 同一分支协作流程（不推荐）  
5. 常见注意事项  

## 1. 分支协作规则
1. 每个人在自己的功能分支开发，例如 `feature/login`、`feature/order-api`。  
2. 不要多人长期共用同一个开发分支。  
3. 推送前必须先同步远端最新代码。  
4. 合并到主分支通过 PR/MR 完成。  

## 2. 标准流程（推荐：功能分支）

### 2.1 切换到自己的分支并检查状态
```bash
git checkout feature/your-task
git status
```

### 2.2 提交本地代码
```bash
git add .
git commit -m "feat: your change"
```

### 2.3 拉取远端最新代码
```bash
git fetch origin
```

### 2.4 将主分支最新内容整合到当前分支（推荐 rebase）
```bash
git rebase origin/main
```

### 2.5 解决冲突后继续 rebase
```bash
git status
git add <已解决文件>
git rebase --continue
```

### 2.6 本地测试通过后推送
```bash
git push origin feature/your-task
```

如果你这个分支之前已经推送过，且做了 `rebase`，用：
```bash
git push --force-with-lease origin feature/your-task
```

## 3. 冲突解决步骤

### 3.1 识别冲突文件
```bash
git status
```

### 3.2 打开冲突文件，处理以下标记
```text
<<<<<<< HEAD
你的代码
=======
对方代码
>>>>>>> origin/main
```

### 3.3 删除冲突标记，保留正确逻辑后标记为已解决
```bash
git add <文件名>
```

### 3.4 继续流程
1. 如果在 `rebase` 中：`git rebase --continue`  
2. 如果在 `merge` 中：`git commit`  

### 3.5 放弃本次操作（必要时）
1. 放弃 rebase：`git rebase --abort`  
2. 放弃 merge：`git merge --abort`  

## 4. 同一分支协作流程（不推荐）

如果你们必须在同一分支（如 `develop`）协作：

### 4.1 先提交本地代码
```bash
git add .
git commit -m "feat: your change"
```

### 4.2 拉取并重放本地提交
```bash
git pull --rebase origin develop
```

### 4.3 有冲突就解决后继续
```bash
git add <文件>
git rebase --continue
```

### 4.4 推送
```bash
git push origin develop
```

## 5. 常见注意事项
1. `git pull` 默认可能是 merge，建议使用 `git pull --rebase`。  
2. 避免使用 `git push --force`，优先 `git push --force-with-lease`。  
3. 每天开工前先同步一次，减少大冲突。  
4. 小步提交、频繁提交，冲突更容易处理。  

如果你要，我可以再给你一版“你们项目分支名专用模板”（例如固定 `main/develop/release` 的完整命令清单），你直接复制执行就行。