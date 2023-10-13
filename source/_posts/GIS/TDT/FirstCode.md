---
title: GIS - 天地图应用开发基础知识 - 编写第一个Map应用
date: 2021-08-23 13:57:07
categories:
	- 学习笔记
tags:
    - GIS
    - 天地图
---

## 简介

使用第三方 GIS 平台 - “天地图”，搭建 Map 应用。

<!-- more -->

## 注册账号

<https://uums.tianditu.gov.cn/register>

## 打开控制台

<https://console.tianditu.gov.cn/api/key>

1. 创建新应用（浏览器端）
2. 复制 Key 备用

## 示例代码

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8" />
    <meta name="keywords" content="天地图" />
    <title>GIS - 天地图应用开发基础知识 - 生成第一个Map应用</title>
    <script
      type="text/javascript"
      src="http://api.tianditu.gov.cn/api?v=4.0&tk=您的密钥"
    ></script>
    <style type="text/css">
      body,
      html {
        width: 100%;
        height: 100%;
        margin: 0;
        font-family: "Microsoft YaHei";
      }
      #mapDiv {
        width: 100%;
        height: 400px;
      }
      input,
      b,
      p {
        margin-left: 5px;
        font-size: 14px;
      }
    </style>
    <script>
      var map;
      var zoom = 12;
      function onLoad() {
        // map = new T.Map("mapDiv"); // 默认为：球面墨卡托投影
        map = new T.Map("mapDiv", {
          projection: "EPSG:4326", // 经纬度直投，编码为：EPSG:4326
        });
        map.centerAndZoom(new T.LngLat(112.54502, 37.86789), zoom);
      }
    </script>
  </head>
  <body onLoad="onLoad()">
    <div id="mapDiv"></div>
  </body>
</html>
```

![效果图片](image.jpg)
