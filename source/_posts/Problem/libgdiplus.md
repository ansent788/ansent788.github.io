---
title: MacM1 添加 libgdiplus 包
date: 2021-09-17 15:14:53
categories:
  - 问题记录
tags:
  - MacM1
  - NetCore
  - libgdiplus
---

### 简介

MacM1 添加 libgdiplus 包

<!-- more -->

### 安装 libgdiplus

```bash
brew install mono-libgdiplus
```

![install](install.png)

### 引用 CoreCompat

```bash
dotnet add package runtime.osx.10.10-x64.CoreCompat.System.Drawing
```
