---
title: Android Instant App:从开发到发布(二)
date: 2019-08-25 22:01:08
tags:
- Android
- Instant App
- App Bundle
- Android App Bundle
---

# Android App Bundle

想要开发 Instant App 的话需要借助 [Android App Bundle](https://developer.android.com/guide/app-bundle/)，App Bundle 是 Google 推出的一种新的上传到 Play Store 的应用发布格式, 它包含了你应用所有已编译的代码和资源，使用 aab 作为扩展名。Google Play 会使用一种叫做**Dynamic Delivery(动态交付)**的应用服务模式, 它会将你的 App Bundle 根据每一个用户的设备配置（分辨率、语言、系统版本等）生成优化后的不同 apk，你不再需要自己在 gralde 中添加配置去生成不同的 apk，用户在 Play Store 下载应用时只会下到设备配置需要的。

除此之外，借助 App Bundle 还能够将应用根据功能分成不同的模块：

- Base Application module
- Dynamic Feature Module

Base Application 就是 Base APK，用户安装应用时直接装到设备上，Dynamic Feature Module 可以在需要的时候动态安装。举个例子，你的应用的首页点击某个按钮会进入一个二级页面，这个二级页面借助 Dynamic Feature Module 可以实现在需要的时候才去安装，安装完后会直接打开这个二级页面。App Bundle 的结构如下图所示：

![App Bundle Format](/images/Android_Bundle_Format.png)

> 想要了解更多关于 App Bundle 的信息可以参考 https://developer.android.com/guide/app-bundle

# 安装 Instant SDK

打开 `SDK Manager-> SDK Tools` 中选择安装 Google Play Instant Development SDK:

![Install Instant SDK](/images/install_instant_sdk.png)


# 调整项目结构

如果你的项目真的很简单不复杂
