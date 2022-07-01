---
title: Linux 常用命令
date: 2021-08-27 15:18:56
categories:
  - 学习笔记
tags:
  - Linux
  - Shell
---

## 简介

Linux 常用命令

<!-- more -->

### 环境查看

- 操作系统和位数信息
  `uname -m && cat /etc/*release`
- 处理器
  `arch`
- 系统内核
  `uname -a`
- 系统版本
  `cat /etc/lsb-release`
- 已加载的文件系统
  `cat /proc/mounts`
- PCI 设备
  `ls pci -tv`
- SB 设备
  `ls usb -tv`

### 结束进程命令

1. 查看端口被哪个程序占用
  `sudo lsof -i tcp:port`
2. 看到进程的 PID，可以将进程杀死。
  `sudo kill -9 PID`
