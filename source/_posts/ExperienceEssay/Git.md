---
title: Git 命令行使用
date: 2021-09-14 16:55:21
categories:
  - 经验随笔
tags:
  - Git
---

### 简介

Git 命令行总结

<!-- more -->

### init 初始化

- 初始化Git代码仓库

  ```console
  git init
  ```

### config 配置

- 查看配置

  ```console
  git config --list｜-l
  ```

- 查看系统配置

  ```console
  git config --system --list｜-l
  ```

- 查看用户（global）配置

  ```console
  git config --global --list｜-l
  ```

- 查看仓库配置

  ```console
  git config --local --list｜-l
  ```

### add 添加

- 添加所有新文件

  ```console
  git add .
  ```

### commit 提交

- 提交并添加更改

  ```console
  git commit -am 'message'
  ```

- 修改提交注释

  ```console
  git commit --amend
  ```

### remote 跟踪

- 跟踪远程分支到指定分支

  ```console
  git remote add origin/[branchName] [branchName]
  ```

- 修改远程仓库地址

  ```console
  git remote set-url origin [newUrl]
  ```

- 查看远程仓库地址

  ```console
  git remote -v
  ```

### pull 拉取

- 远程分支拉取

  ```console
  git pull origin [branchName]:[branchName]
  ```

- 允许不相关历史强制合并

  ```console
  git pull origin master --allow-unrelated-histories
  ```

### push 推送

- 推送到指定的远程分支

  ```console
  git push origin [branchName]
  ```

- 新建并推送到远程分支

  ```console
  git push --set-upstream origin [branchName]
  ```

### reset 重置

- 重置当前最后一次提交

  ```console
  git reset --soft HEAD^
  ```

### cherry-pick 应用提交

- 应用指定提交到当前分支

  ```console
  git cherry-pick [commitHash]
  ```

- 最近一次提交

  ```console
  git cherry-pick [branchName]
  ```

- 多个提交

  ```console
  git cherry-pick [commitHash] [commitHash] ...
  ```

- 一段提交不包含开头

  ```console
  git cherry-pick [commitHash]..[commitHash]
  ```

- 一段提交包含开头

  ```console
  git cherry-pick [commitHash]^..[commitHash]
  ```

-

### stash 储藏

- 储藏当前更改

  ```console
  git stash
  ```

- 读取储藏更改

  ```console
  git stash pop
  ```

### branch 分支

- 查看分支

  ```console list
  git branch -l
  ```

- 查看分支详情

  ```console
  git branch -vv
  ```

- 创建分支

  ```console
  git branch [branchName]
  ```

- 创建分支并指定跟踪分支

  ```console
  git branch [branchName] -t origin/[branchName]
  ```

- 删除本地分支

  ```console
  git branch -d|-D [branchName]
  ```

- 按指定提交创建分支

  ```console
  git branch [branch] [start point]
  ```

- 设置跟踪的远程分支

  ```console
  git branch --set-upstream-to origin/[branchName] [branchName]
  ```

- 修改跟踪的远程分支

  ```console
  git branch -u origin/[branchName] [branchName]
  ```

### checkout 检出

- 撤消当前分支所有修改

  ```console
  git checkout .
  ```

- 切换分支

  ```console
  git checkout [branchName]
  ```

- 创建并切换

  ```console
  git checkout -b [branchName]
  ```

- 创建并切换、设置跟踪的远程分支

  ```console
  git checkout -b [branchName] -t origin/[branchName]
  ```

### rebase 变基

- no branch, rebasing master （当前无分支，可变基到master分支）

  ```console
  git rebase --continue
  ```
