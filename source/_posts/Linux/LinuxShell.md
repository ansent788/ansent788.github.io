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

## 环境查看

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

## 查询命令

### 使用 `du` 命令

`du` 命令能够估算文件空间使用情况。下面是一些常用的 `du` 命令示例：

- **查看当前文件夹下每个子文件夹的大小**：

```bash
du -h --max-depth=1
```

> 其中，`-h` 选项表示以人类可读的格式显示文件大小（如 KB、MB、GB 等），`--max-depth=1` 表示只显示当前目录下一级子目录的大小。

- **查看指定文件夹的大小**：

```bash
du -sh /path/to/your/folder
```

> `-s` 选项表示只显示总和，即只显示指定文件夹的总大小，`-h` 同样是以人类可读的格式显示。你需要将 `/path/to/your/folder` 替换成实际的文件夹路径。

### 使用 `ncdu` 命令（需先安装）

在 Mac 系统里，你可以借助以下命令查看文件夹的大小：
`ncdu` 是一个交互式的磁盘使用情况分析工具，能更直观地查看文件夹大小。

- **安装 `ncdu` 命令**：

> 如果你还没有安装 `ncdu`，可以使用 Homebrew 进行安装：

```bash
brew install ncdu
```

- **使用 `ncdu` 查看文件夹大小**：

```bash
ncdu /path/to/your/folder
```

> 运行该命令后，会打开一个交互式界面，你可以使用上下箭头键浏览文件夹，按回车键进入子文件夹，按 `q` 键退出。你需要将 `/path/to/your/folder` 替换成实际的文件夹路径。

## 后台进程命令

1. 运行后台进程

```bash
sudo nohup [commandName] [> logFile] [2>&1] [&]
```

  > - commandName 命令名称
  > - \> logFile 输出日志
  > - 2>&1 错误日志重定向到输出日志
  > - & 后台运行进程

## 查看进程命令

1. 根据名称查看

```bash
sudo ps -fe | grep [commandName]
```

## 结束进程命令

1. 查看端口被哪个程序占用

```bash
sudo lsof -i tcp:port
```

2. 看到进程的 PID，可以将进程杀死。

```bash
sudo kill -9 PID
```

## 硬件挂载

1. 挂载

```bash
  sudo mount /dev/[deviceName] [Path(一艘为/mnt/[deviceName])]
```

2. 看到进程的 PID，可以将进程杀死。

```bash
sudo umount [Path]
```

## DNS管理

1. 编辑/etc/resolv.conf文件

  > 在文件中添加以下行：
  > nameserver [dnsHost]
  >
  > 请注意，如果您的系统使用resolvconf或其他网络管理服务，您可能需要按照该服务的规范来添加DNS服务器，以确保更改在系统重启后持久生效。在某些系统中，/etc/resolv.conf文件可能是一个符号链接，指向/etc/resolvconf/resolv.conf.d/head或其他位置，您可能需要编辑相应的文件。

## 权限设置

在Ubuntu中，您可以使用chmod和chown命令来设置文件夹的权限，并确保当前用户拥有所需的访问权限。以下是一些基本的命令示例：

1. 给当前用户设置文件夹的读权限：

```bash
chmod u+r 文件夹名称
```

2. 给当前用户设置文件夹的写权限：

```bash
chmod u+w 文件夹名称
```

3. 给当前用户设置文件夹的执行权限：

```bash
chmod u+x 文件夹名称
```

4. 同时给当前用户设置文件夹的读写执行权限：

```bash
chmod u+rwx 文件夹名称
```

5. 如果需要递归地应用权限到所有子文件和子文件夹，可以使用-R选项：

```bash
chmod -R u+rwx 文件夹名称
```

6. 确保当前用户是文件夹的所有者：

```bash
sudo chown $(whoami) 文件夹名称
```

  > 请将“文件夹名称”替换为您要修改权限的实际文件夹名称。如果您需要为组设置权限，可以使用g代替u。如果您需要为所有用户设置权限，可以使用a代替u。

## yum换源

在CentOS 8中更换YUM源，你可以按照以下步骤操作(阿里云源)：

1. 备份当前的YUM源：

```bash
sudo mv /etc/yum.repos.d/CentOS-Linux-BaseOS.repo /etc/yum.repos.d/CentOS-Linux-BaseOS.repo.backup
sudo mv /etc/yum.repos.d/CentOS-Linux-AppStream.repo /etc/yum.repos.d/CentOS-Linux-AppStream.repo.backup
```

2. 下载新的YUM源文件。你可以选择一个新的镜像源或者使用官方的源。这里以使用阿里云的源为例：

```bash
sudo curl -o /etc/yum.repos.d/CentOS-Linux-BaseOS.repo http://mirrors.aliyun.com/repo/Centos-8.repo
sudo curl -o /etc/yum.repos.d/CentOS-Linux-AppStream.repo http://mirrors.aliyun.com/repo/Centos-8.repo
```

3. 清除缓存并生成新的缓存：

```bash
sudo yum clean all
sudo yum makecache
```

4. 更新已安装的包：

```bash
sudo yum update
```
