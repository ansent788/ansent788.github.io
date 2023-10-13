---
title: MacM1 配置 Python 环境
date:  2022-06-21 11：58
categories:
  - 经验随笔
tags:
  - Mac M1
  - Python
  - Anaconda
---

## 简介

MacM1 部署 Python 环境后，大多数 AI 框架支持总是出问题，这里分享下自己的经验。

<!-- more -->

## 安装 Anaconda

推荐大家先安装 Anacaonda，而后在新建的 conda 环境中安装，以避免卸载时的麻烦。

> Anaconda 是一个开源的 Python 发行版本，其包含了 conda、Python 等 180 多个科学包及其依赖项。
> 使用 Anaconda 可以通过创建多个独立的 Python 环境，避免用户的 Python 环境安装太多不同版本依赖导致冲突。

[选择 64-Bit Graphical Installer 下载](https://www.anaconda.com/products/individual)

## 处理架构不匹配问题

```console
# 创建一个可以安装 x64 包的虚拟环境
CONDA_SUBDIR=osx-64 conda create -n [paddlexName] python=[version]

# 激活该环境
conda activate [paddlexName]

# 验证该环境支持平台
python -c "import platform;print(platform.machine())"

# 确保该环境为创建的包为 x64 架构所用
conda env config vars set CONDA_SUBDIR=osx-64

# 退出该环境
conda deactivate

# 重新激活该环境
conda activate [paddlexName]

# 查看环境变量，确定是osx-64
echo "CONDA_SUBDIR: $CONDA_SUBDIR"
```

安装完成后可进入 Python 进行测试