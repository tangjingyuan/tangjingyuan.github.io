---
title: Android Instant App:从开发到发布(二)
date: 2019-08-25 22:01:08
tags:
- Android
- Instant App
- App Bundle
- Android App Bundle
---

## Android App Bundle

想要开发 Instant App 的话需要借助 [Android App Bundle](https://developer.android.com/guide/app-bundle/)，App Bundle 是 Google 推出的一种新的上传到 Play Store 的应用发布格式, 它包含了你应用所有已编译的代码和资源，使用 aab 作为扩展名。Google Play 会使用一种叫做**Dynamic Delivery(动态交付)**的应用服务模式, 它会将你的 App Bundle 根据每一个用户的设备配置（分辨率、语言、系统版本等）生成优化后的不同 apk，你不再需要自己在 gralde 中添加配置去生成不同的 apk，用户在 Play Store 下载应用时只会下到设备配置需要的。

除此之外，借助 App Bundle 还能够将应用根据功能分成不同的模块：

- Base Application module
- Dynamic Feature Module

Base Application 就是 Base APK，用户安装应用时直接装到设备上，Dynamic Feature Module 可以在需要的时候动态安装。举个例子，你的应用的首页点击某个按钮会进入一个二级页面，这个二级页面借助 Dynamic Feature Module 可以实现在需要的时候才去安装，安装完后会直接打开这个二级页面。App Bundle 的结构如下图所示：

![App Bundle Format](/images/Android_Bundle_Format.png)

> 想要了解更多关于 App Bundle 的信息可以参考 https://developer.android.com/guide/app-bundle

## 安装 Instant SDK

打开 `SDK Manager-> SDK Tools` 中选择安装 Google Play Instant Development SDK:

![Install Instant SDK](/images/install_instant_sdk.png)


## 调整项目结构

### 项目简单

如果你的项目真的很简单不复杂, 打出的包也很小，那么你只需要在 app module 的 manifest 文件中添加如下代码就好了：

```
<dist:module dist:instant="true" />
```
![manifest-instant-app](/images/instant_manifest.png)

### 项目复杂

对于复杂项目，开发 Instant App 最重要的两点是：

1. 解耦
2. 模块化
3. 组件化
4. 压缩大小

尤其对于代码很多，业务逻辑复杂，耦合度高，模块化和组件化做的又不好的应用来说，开发 Instant App 是需要花费很多时间把这些做好的，做好这些也可以为 App Bundle 以后的开发做好准备。但是想要做好这些并且一步到位显然需要花费很多的时间，而且风险很大，公司也未必允许你花费这么多时间，这么多成本做。所以对于这些做的并不好的应用来说，我推荐:
1. 新建一个 base app module（Phone & Tablet Module）
里面只包含 Instant App 入口 Activity, 并且在 manifest 中添加之前提到的代码
2. 抽出一个 Instant 需要的功能的 feature module
这个 feature module 里的可能需要跟 app module 里的 activity 有跳转等，所以可以根据实际需求导入路由框架来解开这种耦合，或者不想导入，自己通过 Intent Action 自己造轮子也可以。
3. 抽出 Full App 和 Instnat App 都要用到的公共组件（图片加载 Lib，网络请求 Lib，cache/db lib 等）

这样对现有 Full App 的影响也比较小, 也可以为后续的 App Bundle 开发打好基础，每个周期都做些解耦、组件化等功能。如图示：

![Instant Complex Project Structure Sample](/images/instant_complex_structure_sample.png)

## App Links

## Install Util

## 编译运行

