---
title: H5PlusAPI 学习内容整理 - Geolocation
date: 2021-08-24 14:10:26
categories:
  - 学习笔记
tags:
  - H5
  - 定位
  - UniApp
---

## 简介

Geolocation 模块管理设备位置信息，用于获取地理位置信息，如经度、纬度等。通过 plus.geolocation 可获取设备位置管理对象。虽然 W3C 已经提供标准 API 获取位置信息，但在某些平台存在差异或未实现，为了保持各平台的统一性，定义此规范接口获取位置信息。

<!-- more -->

## H5Plus 功能模块配置

```json
{
  "permissions": {
    "Geolocation": {
      "description": "位置信息"
    }
  }
}
```

## getCurrentPosition

获取当前设备位置信息

> 位置信息将通过手机 GPS 设备或其它信息如 IP 地址、移动网络信号获取，由于获取位置信息可能需要较长的时间，当成功获取位置信息后将通过 successCB 回调函数返回。

```js
void plus.geolocation.getCurrentPosition(successCB, errorCB, option);
```

1. successCB: ( GeolocationSuccessCallback )
   必选 获取设备位置信息成功回调函数
   回调参数: Position
2. errorCB: ( GeolocationErrorCallback )
   可选 获取设备位置信息失败回调函数
   回调参数: GeolocationError
3. options: ( PositionOptions )
   可选 获取设备位置信息的参数

## watchPosition

监听设备位置变化信息

> 位置信息将通过手机 GPS 设备或其它信息如 IP 地址、移动网络信号获取。 当位置信息更新后将通过 successCB 回调函数返回。 位置信息获取失败则调用回调函数 errorCB。

```js
Number plus.geolocation.watchPosition(successCB, errorCB, option);
```

1. successCB: ( GeolocationSuccessCallback )
   必选 获取设备位置信息成功回调函数
   回调参数: Position
2. errorCB: ( GeolocationErrorCallback )
   可选 获取设备位置信息失败回调函数
   回调参数: GeolocationError
3. options: ( PositionOptions )
   可选 获取设备位置信息的参数
4. 返回值: ( Number )
   用于标识位置信息监听器(watchId)，可通过 clearWatch 方法取消监听

## clearWatch

关闭监听设备位置信息

```js
void plus.geolocation.clearWatch(watchId);
```

1. watchId: ( Number )
   必选 需要取消的位置监听器标识，调用 watchPosition 方法的返回值

## GeolocationSuccessCallback

获取设备位置信息成功的回调函数

```js
void onSuccess(position);
```

1. position: ( Position )
   必选 设备的地理位置信息

## GeolocationErrorCallback

获取设备位置信息失败的回调函数

```js
void onError(error);
```

1. error: ( GeolocationError )
   必选 获取位置操作的错误信息

## Position

设备位置信息数据

- coords: (Coordinates 类型 )地理坐标信息，包括经纬度、海拔、速度等信息
- coordsType: (String 类型 )获取到地理坐标信息的坐标系类型
  > 可取以下坐标系类型： "wgs84"：表示 WGS-84 坐标系； "gcj02"：表示国测局经纬度坐标系； "bd09"：表示百度墨卡托坐标系，仅百度定位支持； "bd09ll"：表示百度经纬度坐标系，仅百度定位支持。
- timestamp: (Number 类型 )获取到地理坐标的时间戳信息
  > 时间戳值为从 1970 年 1 月 1 日至今的毫秒数
- address: (Address 类型 )获取到地理位置对应的地址信息
  > 获取地址信息需要连接到服务器进行解析，所以会消耗更多的资源，如果不需要获取地址信息可通过设置 PositionOptions 参数的 geocode 属性值为 false 避免获取地址信息。 如果没有获取到地址信息则返回 undefined。
- addresses: (String 类型 )获取完整地址描述信息
  > 获取完整地址描述信息，如果没有获取到地址信息则返回 undefined

## Address

地址信息

- country: (String 类型 )国家
  > 如“中国”，如果无法获取此信息则返回 undefined。
- province: (String 类型 )省份名称
  > 如“北京市”，如果无法获取此信息则返回 undefined。
- city: (String 类型 )城市名称
  > 如“北京市”，如果无法获取此信息则返回 undefined。
- district: (String 类型 )区（县）名称
  > 如“朝阳区”，如果无法获取此信息则返回 undefined。
- street: (String 类型 )街道信息
  > 如“酒仙桥路”，如果无法获取此信息则返回 undefined。
- streetNum: (String 类型 )获取街道门牌号信息
  > 如“3 号”，如果无法获取此信息则返回 undefined。
- poiName: (String 类型 )POI 信息
  > 如“电子城．国际电子总部”，如果无法获取此信息则返回 undefined。
- postalCode: (String 类型 )邮政编码
  > 如“100016”，如果无法获取此信息则返回 undefined。
- cityCode: (String 类型 )城市代码
  > 如“010”，如果无法获取此信息则返回 undefined。

## Coordinates

地理坐标信息

- latitude: (Number 类型 )坐标纬度值
  > 数据类型对象，地理坐标中的纬度值。
- longitude: (Number 类型 )坐标经度值
  > 数据类型对象，地理坐标中的经度值。
- altitude: (Number 类型 )海拔信息
  > 数据类型对象，如果无法获取此信息，则此值为空（null）。
- accuracy: (Number 类型 )地理坐标信息的精确度信息
  > 数据类型对象，单位为米，其有效值必须大于 0。
- altitudeAccuracy: (Number 类型 )海拔的精确度信息
  > 数据类型对象，单位为米，其有效值必须大于 0。如果无法获取海拔信息，则此值为空（null）。
- heading: (Number 类型 )表示设备移动的方向
  > 数据类型对象，范围为 0 到 360，表示相对于正北方向的角度。如果无法获取此信息，则此值为空（null）。如果设备没有移动则此值为 NaN。
- speed: (Number 类型 )表示设备移动的速度
  > 数据类型对象，单位为米每秒（m/s），其有效值必须大于 0。如果无法获取速度信息，则此值为空（null）。

## PositionOptions

监听设备位置信息参数

- enableHighAccuracy: (Boolean 类型 )是否高精确度获取位置信息
  > 高精度获取表示需要使用更多的系统资源，默认值为 false。
- timeout: (Number 类型 )获取位置信息的超时时间
  > 单位为毫秒（ms），默认值为不超时。如果在指定的时间内没有获取到位置信息则触发错误回调函数。
- maximumAge: (Number 类型 )获取位置信息的间隔时间
  > 单位为毫秒（ms），默认值为 5000（即 5 秒）。调用 plus.geolocation.watchPosition 时为更新位置信息的间隔时间。 注意：在不同定位模块下支持范围值可能不同，如百度定位模块的间隔范围为大于等于 1 秒，如果设置的值小于最小值则使用最小值。
- provider: (String 类型 )优先使用的定位模块
  > 可取以下供应者： "system"：表示系统定位模块，支持 wgs84 坐标系； "baidu"：表示百度定位模块，支持 gcj02/bd09/bd09ll 坐标系； "amap"：表示高德定位模块，支持 gcj02 坐标系。 默认值按以下优先顺序获取（amap>baidu>system），若指定的 provider 不存在或无效则返回错误回调。 注意：百度/高德定位模块需要配置百度/高德地图相关参数才能正常使用。
- coordsType: (String 类型 )指定获取的定位数据坐标系类型
  > 可取以下坐标系类型： "wgs84"：表示 WGS-84 坐标系； "gcj02"：表示国测局经纬度坐标系； "bd09"：表示百度墨卡托坐标系； "bd09ll"：表示百度经纬度坐标系； provider 为"system"时，支持 wgs84 坐标系，默认使用"wgs84"坐标系； provider 为"baidu"时，支持 gcj02/bd09/bd09ll 坐标系，默认使用"gcj02"坐标系； provider 为"amap"时，支持 gcj02 坐标系，默认使用"gcj02"坐标系。 如果设置的坐标系类型 provider 不支持，则返回错误。
- geocode: (Boolean 类型 )是否解析地址信息
  > 解析的地址信息保存到 Position 对象的 address、addresses 属性中，true 表示解析地址信息，false 表示不解析地址信息，返回的 Position 对象的 address、addresses 属性值为 undefined，默认值为 true。 如果解析地址信息失败则返回的 Position 对象的 address、addresses 属性值为 null。

## GeolocationError

定位错误信息

- PERMISSION_DENIED: (Number 类型 )访问权限被拒绝
  > 系统不允许程序获取定位功能，错误代码常量值为 1。
- POSITION_UNAVAILABLE: (Number 类型 )位置信息不可用
  > 无法获取有效的位置信息，错误代码常量值为 2。
- TIMEOUT: (Number 类型 )获取位置信息超时
  > 无法在指定的时间内获取位置信息，错误代码常量值为 3。
- code: (Number 类型 )错误代码
  > 可取值:
  >
  > - PERMISSION_DENIED - 用户拒绝授权
  > - POSITION_UNAVAILABLE - 位置服务不可用，如系统定位服务关闭
  > - TIMEOUT - 定位超时，定位时超过 PositionOptions.timeout 设置的时间触发，此时通常可以重试
  > - 其它错误 - 5+ 扩展错误码，参考 5+ API 错误代码中的“Geolocation 模块错误”
- message: (String 类型 )错误描述信息
  > 详细错误描述信息。
