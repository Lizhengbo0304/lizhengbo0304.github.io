---
layout: post
title: Git常用命令速查
date: 2024-07-10 10:00:00 +0800
categories: [Git]
tags: [Git, 命令, 版本控制]
---

本文整理了日常开发中常用的Git命令，方便快速查阅。

## 仓库初始化与关联

### 初始化裸仓库
用于在服务器上创建一个不包含工作目录的仓库，通常作为远程仓库使用。

```bash
git init --bare <path/to/your/repo.git>
```

> **Tip:** 通常在服务器上创建裸仓库，作为团队协作的中央仓库。

### 添加远程仓库
将本地仓库与远程仓库关联起来。

```bash
git remote add origin <remote_repository_url>
```

> **Tip:** `<remote_repository_url>` 可以是 HTTPS 或 SSH 格式的地址。

### 关联本地分支到远程分支并推送
首次推送本地分支到远程仓库时使用，并建立跟踪关系。

```bash
git push -u origin <branch_name>
```

> **Tip:** `-u` 参数会在推送的同时建立本地分支与远程分支的跟踪关系，之后可以直接使用 `git push` 和 `git pull`。

### 查看远程仓库地址
查看当前本地仓库关联的远程仓库地址。

```bash
git remote -v
```

### 更改远程仓库地址
当远程仓库地址发生变化时，可以使用此命令更新。

```bash
git remote set-url origin <new_remote_repository_url>
```

> **Tip:** 当项目迁移或远程仓库地址变更时非常有用。

## 日常操作

### 获取远程仓库最新更改
拉取远程仓库的最新代码到本地，并与本地分支合并。

```bash
git pull
```

> **Tip:** 相当于 `git fetch` 和 `git merge` 的组合。

### 暂存未提交的更改
当你需要切换分支或执行其他操作，但当前工作区有未提交的更改时，可以使用 `git stash` 暂时保存。

```bash
git stash
```

> **Tip:** 使用 `git stash pop` 可以恢复最近一次暂存的更改并删除该暂存。

### 硬重置到指定提交
将当前分支强制重置到指定的提交，会丢弃该提交之后的所有更改，请谨慎使用。

```bash
git reset --hard <commit_hash>
```

> **Warning:** 这是一个危险的操作，会永久丢失未提交的更改和指定提交之后的所有历史记录。

### 强制推送本地仓库状态到远程仓库
当本地分支与远程分支历史不一致时，可以使用 `git push --force` 强制覆盖远程分支，请务必小心使用，以免覆盖他人工作。

```bash
git push origin <branch_name> --force
```

> **Warning:** 强制推送会覆盖远程仓库的历史，可能导致团队成员的代码丢失，除非你非常清楚自己在做什么，否则应避免使用。考虑使用 `git push --force-with-lease` 以更安全的方式进行强制推送。

## 常用操作补充

### 查看状态
查看工作区和暂存区的状态，了解哪些文件被修改、新增或删除。

```bash
git status
```

> **Tip:** 经常使用此命令可以帮助你了解当前仓库的状况。

### 添加文件到暂存区
将工作区的更改添加到暂存区，准备提交。

```bash
git add <file_name>
# 或者添加所有更改
git add .
```

### 提交更改
将暂存区的更改提交到本地仓库。

```bash
git commit -m "<commit_message>"
```

> **Tip:** 提交信息应清晰、简洁地描述本次提交的内容。

### 查看提交历史
查看项目的提交历史记录。

```bash
git log
```

> **Tip:** 可以使用 `git log --oneline --graph --all` 查看更简洁和图形化的历史。

### 创建新分支
基于当前分支创建一个新分支。

```bash
git branch <new_branch_name>
```

### 切换分支
切换到指定分支。

```bash
git checkout <branch_name>
# 或者 Git 2.23+ 版本推荐使用
git switch <branch_name>
```

### 创建并切换到新分支

```bash
git checkout -b <new_branch_name>
# 或者 Git 2.23+ 版本推荐使用
git switch -c <new_branch_name>
```

### 合并分支
将指定分支的更改合并到当前分支。

```bash
git merge <branch_to_merge>
```

> **Tip:** 合并前最好确保当前分支是干净的（没有未提交的更改）。

### 删除分支
删除本地分支。

```bash
git branch -d <branch_name>
```

> **Tip:** `-d` 选项只能删除已合并的分支，如果要强制删除未合并的分支，请使用 `-D`。

### 撤销文件修改
撤销工作区中对文件的修改，恢复到最近一次提交的状态。

```bash
git checkout -- <file_name>
```

> **Warning:** 此操作会丢失工作区中对该文件的所有未提交更改。

### 撤销暂存区文件
将文件从暂存区移回工作区，但保留工作区的修改。

```bash
git reset HEAD <file_name>
```

### 克隆远程仓库
将远程仓库克隆到本地。

```bash
git clone <remote_repository_url> [local_directory_name]
```

> **Tip:** `[local_directory_name]` 是可选的，如果不指定，则会以仓库名作为本地目录名。

### 配置用户信息
配置 Git 的全局用户名和邮箱，这些信息会记录在你的提交中。

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

> **Tip:** `--global` 参数表示全局配置，只配置一次即可。如果需要为特定项目配置，可以去掉 `--global` 参数在项目目录下执行。