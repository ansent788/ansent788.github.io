---
title: Mac 无线投屏 Scrcpy
date: 2021-09-24 16:27:11
categories:
  - 经验随笔
tags:
  - Mac
  - Scrcpy
  - 无线投屏
---

### 简介

安卓手机无线投屏到 Mac

<!-- more -->

### 安装 Scrcpy

国外源码 <https://github.com/mirrors/scrcpy>
国内镜像 <https://gitee.com/mirrors/scrcpy>

1. 清华源 homebrew 安装
   > <https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/>
2. 调试包 adb 安装
   `brew install android-platform-tools`
3. 安装 scrcpy
   `brew install scrcpy`

### 连接手机

1. 打开手机 USB 调试
   > 有些手机需打开“仅充电形式下打开 ADB 调试”，当通知档显示有 USB 调试即配置成功
2. 连接 USB
3. 运行 scrcpy
   `scrcpy`

### 无线投屏

1. 按连接手机步骤完成
2. 打开配置 adb 连接端口
   `adb tcpip [port]`
3. 查看手机 IP
   `adb shell ip route | awk '{print $9}'`
4. ‎ 拔下设备插头 ‎
5. 打开 adb 调试链接
   `adb connect [ip]:[port]`
6. 运行 scrcpy
   `scrcpy`
