---
title: npm 命令行使用
date: 2022-04-11 09:44:21
categories:
  - 经验随笔
tags:
  - npm
---

### 简介

npm 使用经验与命令总结

<!-- more -->

### node-sass 问题

- MacM1 下无法编译 node-sass，问题出现是因 node-sass 不支持 arm64，node16 后支持了 M1 安装包已升级成为 arm64包。

  > 解决办法，去 Node.js 官网下载对应版本的 [x64](https://nodejs.org/dist/v16.14.2/node-v16.14.2-darwin-x64.tar.gz) 版本，然后按手动复制到 /usr/local
  >
  > ***注：其他版本同理***
