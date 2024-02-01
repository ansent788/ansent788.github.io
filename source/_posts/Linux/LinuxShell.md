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
- USB 设备
  `ls usb -tv`

### 后台进程命令

1. 运行后台进程
  `sudo nohup [commandName] [> logFile] [2>&1] [&]`

  > - commandName 命令名称
  > - \> logFile 输出日志
  > - 2>&1 错误日志重定向到输出日志
  > - & 后台运行进程

### 查看进程命令

1. 根据名称查看
  `sudo ps -fe | grep [commandName]`

### 结束进程命令

1. 查看端口被哪个程序占用
  `sudo lsof -i tcp:port`
2. 看到进程的 PID，可以将进程杀死。
  `sudo kill -9 PID`

### 硬件挂载

1. 挂载
  `sudo mount /dev/[deviceName] [Path(一艘为/mnt/[deviceName])]`
1. 看到进程的 PID，可以将进程杀死。
  `sudo umount [Path]`
