---
title: MacApiError 数据处理中流不可异步写入
date: 2021-08-27 11:30:44
categories:
  - 问题记录
tags:
  - Mac
  - NetCore
---

## 简介

最近在苹果本上运行 NetCore WebApi 时遇到的问题

<!-- more -->

## 出现的问题

1. 未做数据过滤时正常
2. 数据过滤，进行加密后报错
3. 去掉数据加密正常
4. Win 系列系统正常
5. Mac 系统不正常（其他 Linux 未做测试）

## 问题出现原因

1. 数据加密需要对返回内容做加密处理，并写入流
2. WebApi 数据处理中流不可直接异步写入，也不可同步写入

## 解决办法

1. 服务配置为允许同步写入: AllowSynchronousIO = true
   ```csharp
   services.Configure<KestrelServerOptions>(option => option.AllowSynchronousIO = true);
   services.Configure<IISServerOptions>(option => option.AllowSynchronousIO = true);
   ```
2. 异步、同步都可写入了
   ```csharp
   using var resWriter = new StreamWriter(resOrigin);
   await resWriter.WriteAsync(writeStr);
   await resWriter.FlushAsync();
   //将原始的请求和响应流替换回去
   context.Request.Body = reqOrigin;
   context.Response.Body = resOrigin;
   ```
