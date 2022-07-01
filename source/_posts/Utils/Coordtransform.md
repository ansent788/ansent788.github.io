---
title: 坐标转换
date: 2021-12-23 10:43:33
categories:
  - 工具
tags:
  - coordinate
  - wgs84
  - bd09
  - gcj02
  - transform
---

## 简介

提供了百度坐标（BD09）、国测局坐标（火星坐标，GCJ02）、和WGS84坐标系之间的转换的工具模块。

<!-- more -->

国测局坐标(火星坐标,比如高德地图在用),百度坐标,wgs84坐标(谷歌国外以及绝大部分国外在线地图使用的坐标)。

### 需转换的坐标，一行一条，格式如下：
分隔符：<input value="," onchange="oldSplitChange(this)" />
> 116.404<span name="old_split">,</span>39.915
> 116.404<span name="old_split">,</span>39.915
<textarea id="old_value" style="width:100%;height: 200px"></textarea>

<select id="coord_select">
  <option>bd09 to gcj02</option>
  <option>gcj02 to bd09</option>
  <option>wgs84 to gcj02</option>
  <option>gcj02 to wgs84</option>
</select>
<button onclick="ctf()">转换</button>

### 转换出的坐标
分隔符：<input value="," onchange="newSplitChange(this)" />
<textarea id="new_value" style="width:100%;height: 200px"></textarea>

<script src="/utils/coordtransform.js"></script>
<script>
  var old_split = ',', new_split = ',';
  function oldSplitChange(obj){
    if(obj.value != '') old_split = obj.value;
    document.getElementsByName('old_split').forEach(item=>{item.innerHTML = old_split});
  }
  function newSplitChange(obj){
    if(obj.value != '') new_split = obj.value;
  }
  function ctf() {
    var select = document.getElementById('coord_select'); //定位id
    var index = select.selectedIndex; // 选中索引
    var text = select.options[index].text; // 选中文本
    var tffn;
    switch(text) {
      //百度经纬度坐标转国测局坐标
      case 'bd09 to gcj02':
        tffn = coordtransform.bd09togcj02;
        break;
      //国测局坐标转百度经纬度坐标
      case 'gcj02 to bd09':
        tffn = coordtransform.gcj02tobd09;
        break;
      //wgs84转国测局坐标
      case 'wgs84 to gcj02':
        tffn = coordtransform.wgs84togcj02;
        break;
      //国测局坐标转wgs84坐标
      case 'gcj02 to wgs84':
        tffn = coordtransform.gcj02towgs84;
        break;
      default:
        tffn = coordtransform.bd09togcj02;
        break;
    }
    var old_coord = document.getElementById('old_value').value.split('\n').map(item=>{return item.split(old_split)});
    var new_coord = '';
    for(const item of old_coord) {
      if(new_coord != '') new_coord += '\n';
      var coord = tffn(item[0],item[1]);
      new_coord += coord[0] + new_split + coord[1];
      if(item.length>2){
        for (let index = 2; index < item.length; index += 2){
          coord = tffn(item[index],item[index + 1]);
          new_coord += new_split + coord[0] + new_split + coord[1];
        }
      }
    }
    document.getElementById('new_value').value = new_coord;
  }
</script>