---
title: Git 命令行使用
date: 2021-09-14 16:55:21
categories:
  - 经验随笔
tags:
  - Git
---

### 1. 简介

Git 命令行总结

<!-- more -->

### 2. init 初始化

- 初始化Git代码仓库

  ```bash
  git init
  ```

### 3. config 配置

- 查看配置

  ```bash
  git config --list｜-l
  ```

- 查看系统配置

  ```bash
  git config --system --list｜-l
  ```

- 查看用户（global）配置

  ```bash
  git config --global --list｜-l
  ```

- 查看仓库配置

  ```bash
  git config --local --list｜-l
  ```

### 4. add 添加

- 添加所有新文件

  ```bash
  git add .
  ```

### 5. commit 提交

- 提交并添加更改

  ```bash
  git commit -am 'message'
  ```

- 修改提交注释

  ```bash
  git commit --amend
  ```

### 6. remote 跟踪

- 跟踪远程分支到指定分支

  ```bash
  git remote add origin/[branchName] [branchName]
  ```

- 修改远程仓库地址

  ```bash
  git remote set-url origin [newUrl]
  ```

- 查看远程仓库地址

  ```bash
  git remote -v
  ```

### 7. pull 拉取

- 远程分支拉取 - 不变基

  ```bash
  git pull origin [branchName]:[branchName] --no-rebase
  ```

- 远程分支拉取 - 变基

  ```bash
  git pull origin [branchName]:[branchName] --rebase
  ```

- 允许不相关历史强制合并

  ```bash
  git pull origin master --allow-unrelated-histories
  ```

### 8. push 推送

- 推送到指定的远程分支

  ```bash
  git push origin [branchName]
  ```

- 推送到指定的远程分支 - 强制

  ```bash
  git push origin [branchName] --force
  ```

- 新建并推送到远程分支

  ```bash
  git push --set-upstream origin [branchName]
  ```

### 9. reset 重置

- 重置当前最后一次提交

  ```bash
  git reset --soft HEAD^
  ```

### 10. cherry-pick 应用提交

- 应用指定提交到当前分支

  ```bash
  git cherry-pick [commitHash]
  ```

- 最近一次提交

  ```bash
  git cherry-pick [branchName]
  ```

- 多个提交

  ```bash
  git cherry-pick [commitHash] [commitHash] ...
  ```

- 一段提交不包含开头

  ```bash
  git cherry-pick [commitHash]..[commitHash]
  ```

- 一段提交包含开头

  ```bash
  git cherry-pick [commitHash]^..[commitHash]
  ```

-

### 11. stash 储藏

- 储藏当前更改

  ```bash
  git stash
  ```

- 读取储藏更改

  ```bash
  git stash pop
  ```

### 12. branch 分支

- 查看分支

  ```bash list
  git branch -l
  ```

- 查看分支详情

  ```bash
  git branch -vv
  ```

- 创建分支

  ```bash
  git branch [branchName]
  ```

- 创建分支并指定跟踪分支

  ```bash
  git branch [branchName] -t origin/[branchName]
  ```

- 删除本地分支

  ```bash
  git branch -d|-D [branchName]
  ```

- 按指定提交创建分支

  ```bash
  git branch [branch] [start point]
  ```

- 设置跟踪的远程分支

  ```bash
  git branch --set-upstream-to origin/[branchName] [branchName]
  ```

- 修改跟踪的远程分支

  ```bash
  git branch -u origin/[branchName] [branchName]
  ```

### 13. checkout 检出

- 撤消当前分支所有修改

  ```bash
  git checkout .
  ```

- 切换分支

  ```bash
  git checkout [branchName]
  ```

- 创建并切换

  ```bash
  git checkout -b [branchName]
  ```

- 创建并切换、设置跟踪的远程分支

  ```bash
  git checkout -b [branchName] -t origin/[branchName]
  ```

### 14. rebase 变基

- no branch, rebasing master （当前无分支，可变基到master分支）

  ```bash
  git rebase --continue
  ```
