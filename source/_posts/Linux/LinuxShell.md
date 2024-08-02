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

### DNS管理

1. 编辑/etc/resolv.conf文件

  > 在文件中添加以下行：
  > nameserver [dnsHost]
  >
  > 请注意，如果您的系统使用resolvconf或其他网络管理服务，您可能需要按照该服务的规范来添加DNS服务器，以确保更改在系统重启后持久生效。在某些系统中，/etc/resolv.conf文件可能是一个符号链接，指向/etc/resolvconf/resolv.conf.d/head或其他位置，您可能需要编辑相应的文件。

### 权限设置

在Ubuntu中，您可以使用chmod和chown命令来设置文件夹的权限，并确保当前用户拥有所需的访问权限。以下是一些基本的命令示例：

1. 给当前用户设置文件夹的读权限：
  `chmod u+r 文件夹名称
1. 给当前用户设置文件夹的写权限：
  `chmod u+w 文件夹名称`
1. 给当前用户设置文件夹的执行权限：
  `chmod u+x 文件夹名称`
1. 同时给当前用户设置文件夹的读写执行权限：
  `chmod u+rwx 文件夹名称`
1. 如果需要递归地应用权限到所有子文件和子文件夹，可以使用-R选项：
  `chmod -R u+rwx 文件夹名称`
1. 确保当前用户是文件夹的所有者：
  `sudo chown $(whoami) 文件夹名称`

    > 请将“文件夹名称”替换为您要修改权限的实际文件夹名称。如果您需要为组设置权限，可以使用g代替u。如果您需要为所有用户设置权限，可以使用a代替u。

### yum换源

在CentOS 8中更换YUM源，你可以按照以下步骤操作：

1. 备份当前的YUM源：
  `sudo mv /etc/yum.repos.d/CentOS-Linux-BaseOS.repo /etc/yum.repos.d/CentOS-Linux-BaseOS.repo.backup`
  `sudo mv /etc/yum.repos.d/CentOS-Linux-AppStream.repo /etc/yum.repos.d/CentOS-Linux-AppStream.repo.backup`
1. 下载新的YUM源文件。你可以选择一个新的镜像源或者使用官方的源。这里以使用阿里云的源为例：
  `sudo curl -o /etc/yum.repos.d/CentOS-Linux-BaseOS.repo http://mirrors.aliyun.com/repo/Centos-8.repo`
  `sudo curl -o /etc/yum.repos.d/CentOS-Linux-AppStream.repo http://mirrors.aliyun.com/repo/Centos-8.repo`
1. 清除缓存并生成新的缓存：
  `sudo yum clean all`
  `sudo yum makecache`
1. 更新已安装的包：
  `sudo yum update`

> 以上步骤将会把CentOS 8的YUM源更换为阿里云的源。你可以根据需要替换为其他的源。
