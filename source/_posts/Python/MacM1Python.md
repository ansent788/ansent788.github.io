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

## ubuntu处理OpenGL

在Ubuntu系统中，libGL.so.1 是 OpenGL 的动态链接库，通常由 Mesa 或者其他图形驱动包提供。如果你遇到了需要安装 libGL.so.1 的情况，可以通过以下命令来安装：

```console
sudo apt-get update
sudo apt-get install libgl1-mesa-glx
```

这条命令会安装 Mesa 提供的 OpenGL 实现库，确保你的系统有图形界面的话，这通常是必须的。如果你在没有图形界面的服务器上运行需要 OpenGL 的程序，你可能需要安装头文件和开发库：

```console
sudo apt-get install libgl1-mesa-dev
```

如果你遇到了特定版本的依赖问题，可以使用 apt-file 工具来查找并安装所需的文件：

```console
sudo apt-get install apt-file
sudo apt-file update
sudo apt-file search libGL.so.1
```

然后根据 apt-file 提供的信息安装相应的包。
