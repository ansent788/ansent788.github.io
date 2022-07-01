---
title: Mac 安装 PaddleX
date: 2021-09-08 15:19:54
categories:
  - 学习笔记
tags:
  - PaddlePaddle
  - PaddleX
  - VC
  - ML
  - Python
---

## 简介

飞桨全流程开发工具，集飞桨核心框架、模型库、工具及组件等深度学习开发所需全部能力于一身，打通深度学习开发全流程。PaddleX 同时提供简明易懂的 Python API，及一键下载安装的图形化开发客户端。用户可根据实际生产需求选择相应的开发方式，获得飞桨全流程开发的最佳体验。

<!-- more -->

## API 方式

推荐大家先安装 Anacaonda，而后在新建的 conda 环境中安装，以避免卸载时的麻烦。

> Anaconda 是一个开源的 Python 发行版本，其包含了 conda、Python 等 180 多个科学包及其依赖项。
> 使用 Anaconda 可以通过创建多个独立的 Python 环境，避免用户的 Python 环境安装太多不同版本依赖导致冲突。

1. [Anaconda 下载](https://www.anaconda.com/products/individual)
   > 选择 64-Bit Graphical Installer 下载
1. 创建 Python 环境
   > conda create -n [paddlexName] python=[version]
   > 注：_M1 版本 CPU 选择 3.8，Intel 建议选择 Python3_
1. 进入 Python 环境，部署依赖库
   > conda activate [paddlexName]
   > pip install cython
   > pip install pycocotools
1. 安装 PaddlePaddle 与 PaddleX
   > pip install paddlepaddle 或 paddlepaddle-gpu
   > pip install paddlex

### 架构不匹配可按如下方式创建环境

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

## GUI 方式

1. [PaddleX GUI 下载](https://www.paddlepaddle.org.cn/paddlex/download)
1. 解压后，修改文件夹名称为：[可执行文件名称].app
1. 拷贝到应用程序目录下，安装完毕

> 也可按文件夹中提示方式直接使用

## Restful 方式

[Mac 版下载地址](https://bj.bcebos.com/paddlex/PaddleX_Remote_GUI/mac/PaddleX_Remote_GUI.zip)
[Win 版下载地址](https://bj.bcebos.com/paddlex/PaddleX_Remote_GUI/windows/PaddleX_Remote_GUI.zip)

> 下载后解压按说明文件操作即可
> [详细说明](https://github.com/PaddlePaddle/PaddleX/blob/develop/docs/Resful_API/docs/readme.md)
