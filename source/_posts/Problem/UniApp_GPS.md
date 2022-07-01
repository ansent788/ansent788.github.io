---
title: uniapp 判断 IOS 和 Android 的 GPS 是否开启并设置启动
date: 2021-08-25 14:33:16
categories:
  - 问题记录
tags:
  - UniApp
  - GPS
---

### uniapp 判断 IOS 和 Android 的 GPS 是否开启并设置启动

<!-- more -->

#### Native.js 原生 API

```js
let system = uni.getSystemInfoSync(); // 获取系统信息
if (system.platform === "android") {
  // 判断平台
  var context = plus.android.importClass("android.content.Context");
  var locationManager = plus.android.importClass(
    "android.location.LocationManager"
  );
  var main = plus.android.runtimeMainActivity();
  var mainSvr = main.getSystemService(context.LOCATION_SERVICE);
  if (!mainSvr.isProviderEnabled(locationManager.GPS_PROVIDER)) {
    uni.showModal({
      title: "提示",
      content: "请打开定位服务功能",
      showCancel: false, // 不显示取消按钮
      success() {
        if (!mainSvr.isProviderEnabled(locationManager.GPS_PROVIDER)) {
          var Intent = plus.android.importClass("android.content.Intent");
          var Settings = plus.android.importClass("android.provider.Settings");
          var intent = new Intent(Settings.ACTION_LOCATION_SOURCE_SETTINGS);
          main.startActivity(intent); // 打开系统设置GPS服务页面
        } else {
          console.log("GPS功能已开启");
        }
      },
    });
  }
} else if (system.platform === "ios") {
  var cllocationManger = plus.ios.import("CLLocationManager");
  var enable = cllocationManger.locationServicesEnabled();
  var status = cllocationManger.authorizationStatus();
  plus.ios.deleteObject(cllocationManger);
  if (enable && status != 2) {
    console.log("手机系统的定位已经打开");
  } else {
    console.log("手机系统的定位没有打开");
    uni.showModal({
      title: "提示",
      content: "请打开定位服务功能",
      showCancel: false, // 不显示取消按钮
      success() {
        var UIApplication = plus.ios.import("UIApplication");
        var application2 = UIApplication.sharedApplication();
        var NSURL2 = plus.ios.import("NSURL");
        // var setting2 = NSURL2.URLWithString("prefs:root=LOCATION_SERVICES");
        // var setting2 = NSURL2.URLWithString("App-Prefs:root=LOCATION_SERVICES");
        // var setting2 = NSURL2.URLWithString("app-settings");
        var setting2 = NSURL2.URLWithString(
          "App-Prefs:root=Privacy&path=LOCATION"
        );
        // var setting2 = NSURL2.URLWithString("App-Prefs:root=Privacy&path=LOCATION_SERVICES");
        application2.openURL(setting2);
        plus.ios.deleteObject(setting2);
        plus.ios.deleteObject(NSURL2);
        plus.ios.deleteObject(application2);
      },
    });
  }
}
```

#### permission 模块

> <https://ext.dcloud.net.cn/plugin?id=594>
