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

### 3. clone 初始化

- 默认克隆主分支

  ```bash
  git clone 远程仓库地址
  ```

- 指定clone某个分支

  ```bash
  git clone -b 远程分支名 远程仓库地址
  ```

### 4. config 配置

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

- 配置用户（global）用户

  ```bash
  git config --global user.name [your name]
  ```

- 配置用户（global）邮箱

  ```bash
  git config --global user.email [your email]
  ```

- 查看仓库配置

  ```bash
  git config --local --list｜-l
  ```

### 5. add 添加

- 添加所有新文件

  ```bash
  git add .
  ```

### 6. commit 更改

- 带注释添加更改

  ```bash
  git commit -am 'message'
  ```

- 修改更改注释

  ```bash
  git commit --amend
  ```

### 7. remote 跟踪

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

- 删除本地存在远程不存在的分支

  ```bash
  git remote prune origin
  ```

### 8. pull 拉取

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

### 9. push 推送

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

### 10. reset 重置

- 重置当前最后一次更改

  ```bash
  git reset --soft HEAD^
  ```

### 11. cherry-pick 应用更改

- 应用指定更改到当前分支

  ```bash
  git cherry-pick [commitHash]
  ```

- 最近一次更改

  ```bash
  git cherry-pick [branchName]
  ```

- 多个更改

  ```bash
  git cherry-pick [commitHash] [commitHash] ...
  ```

- 一段更改不包含开头

  ```bash
  git cherry-pick [commitHash]..[commitHash]
  ```

- 一段更改包含开头

  ```bash
  git cherry-pick [commitHash]^..[commitHash]
  ```

-

### 12. stash 储藏

- 储藏当前更改

  ```bash
  git stash
  ```

- 读取储藏更改

  ```bash
  git stash pop
  ```

### 13. branch 分支

- 查看当前远端分支情况

  ```bash
  git branch -a
  ```

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

- 按指定更改创建分支

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

### 14. checkout 检出

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

### 15. rebase 变基

- no branch, rebasing master （当前无分支，可变基到master分支）

  ```bash
  git rebase --continue
  ```

### 16. reset 重置某次更改

- 回滚到某次更改

  ```bash
  git reset commit_id
  ```

- 此次更改之后的修改会被退回到暂存区

  ```bash
  git reset --soft commit_id
  ```

- 此次更改之后的修改不做任何保留

  ```bash
  git reset --hard commit_id
  ```

### 17. rebase 重新设置基准

> 当两个分支不在一条线上，需要执行 merge 操作时使用该命令

- 撤销更改

  ```bash
  git log // 查找要删除的前一次更改的 commit_id
  git rebase -i commit_id // 将 commit_id 替换成复制的值
  进入 Vim 编辑模式，将要删除的 commit 前面的 `pick` 改成 `drop`
  保存并退出 Vim
  ```

- 解决冲突

  ```bash
  git diff // 查看冲突内容
  // 手动解决冲突（冲突位置已在文件中标明）
  git add <file> 或 git add -A // 添加
  git rebase --continue // 继续 rebase
  // 若还在 rebase 状态，则重复 2、3、4，直至 rebase 完成出现 applying 字样
  git push
  ```

### 18. revert 还原到更改

- 之前的更改仍会保留在 git log 中，而此次还原会做为一次新的更改

  ```bash
  git log // 查找需要还原的 commit_id
  git revert commit_id  // 还原到这次更改
  ```

- merge 节点的话，则需要加上 -m 指令

  ```bash
  git revert commit_id -m 1 // 第一个更改点
  // 手动解决冲突
  git add -A
  git commit -m ""
  git revert commit_id -m 2 // 第二个更改点
  // 重复 2，3，4
  git push
  ```

### 19. merge 合并分支

- 合并分支到当前分支

  ```bash
  git branch -a -vv // 查看所有分支是否为最新更改
  // 拉取最新更改
  git merge [branchName] // 合并为v祸从口出当前分支
  ```

### 20. fetch 抓取

- 清除无效分支

  ```bash
  git fetch -p // 删除本地存在，但远程不存在的分支
  ```

### 21. 撤销修改

要撤销在Git中的修改，可以使用以下命令：

- 如果你还没有把修改添加到暂存区，可以使用：

  ```bash
  git checkout -- <file>
  ```

  > 这将撤销指定文件的本地修改。

- 如果已经添加到暂存区，需要先取消暂存，然后再撤销修改：

  ```bash
  git reset HEAD <file>
  git checkout -- <file>
  ```

- 如果你已经提交了修改，你需要使用git revert或git reset来撤销更改。
  > 使用git revert创建一个新的提交来撤销之前的更改：

  ```bash
  git revert <commit>
  ```

  > 使用git reset将HEAD指针移回到之前的提交，这样你的工作目录中的文件就会恢复到那个状态。这个操作会丢失最近的提交，所以请小心使用。

  ```bash
  git reset --hard <commit>
  ```

  > 在这里，<commit>是你想要撤销的提交的哈希值。使用--hard选项会修改工作目录中的文件，以匹配指定的提交。如果不想影响工作目录中的文件，可以使用--soft选项或者不添加任何选项（默认为--mixed）。
