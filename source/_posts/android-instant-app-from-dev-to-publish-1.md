---
title: Android Instant App:从开发到发布(一)
date: 2019-08-24 12:09:52
tags:
- Android
- Instant App
- Publish
- Tapatalk
- Vimeo
- iQiyi
- Royal Clash
---

最近在开发 Instant App 与发布的时候遇到了很多问题，踩了很多坑。网上分享的文章，大多都是以新建一个 Instant App 为例，介绍了 Instant App 的结构等等，很少有基于现有 App 开发 Instant App 与发布的分享，毕竟 Instant App 发布的一个前提条件就是已经在 Play Sotre 中发布了完整版应用。所以我决定将自己开发与发布过程中的经验分享出来。文章一共会分为三篇：

1. [对 Instant App 进行介绍并讨论 Instant App 在从产品层面上值不值得开发（包括适合哪些应用开发）](/2019/08/24/android-instant-app-from-dev-to-publish-1/)
2. [介绍 Instant App 基于当前项目如何开发](/2019/08/25/android-instant-app-from-dev-to-publish-2/)
3. [介绍 Instant App 测试与发布](/2019/08/25/android-instant-app-from-dev-to-publish-3/)

## Instant App 是什么

按 Android Developers 上的说法，Instant App 是 Google 推出的一种无需安装的原生 Android 应用。这里的**无需安装**指的是不需要安装完整版的应用即可体验应用里的功能，Instant App 本身也是需要安装的。其实 Instant App 就是 Native App，只是它的体积很小（在 4MB 以内，Google 正在测试将 Instnat App 的体积放宽到 10MB），生存在一种特殊的 Sandbox 中。

## 与小程序的区别

以微信小程序为例，小程序一般是使用Web技术开发的H5页面，让用户不需要安装 Native App 即可在微信里享受到相同的功能，可以节省用户的手机存储空间，享受到微信流量带来的好处等等。小程序本质是打造微信自己的生态圈，提高用户粘度，将用户圈在微信的生态里。

Instant App 开发技术与 Native App 一致，使用 Java/Kotlin 进行开发，用户可以在 Instant 享受到跟 Native App 一样的优秀体验。它的目的是提供一种试玩的功能，用户可以在试玩后决定是否需要下载完整版应用。

## 案例

在开发 Instant App 之前，我们特意调研了下有哪些应用已经推出了 Instant App。令我颇感意外的是，爱奇艺也推出了 Instant App，我原本觉得只有海外或者是像我们一样专做海外市场的 App 才会推出 Instant App。

![image](/images/ming_xue.gif)

这里简单列出 Tapatalk（我们自己的产品）、Vimeo、爱奇艺、皇室战争这几个推出了 Instant App 的应用在 Play Store 的截图(爱奇艺的 Instant App 刚在 Play Store 看不到，就不贴了)， 大家可以自行体验。

![InstanApp-Case](/images/instant_app_cases.jpg)

## 用户安装与打开的路径

>并不是海外每个国家与地区的每个用户、设备都可以安装使用 Instant App，具体哪些地区哪些设备支持，可以自行 Google
 
目前一共可以分为三个路径：

- Try Now Button in Play Store
Try Now Button 其实就是上图中的**立即试用**按钮，用户通过在 Play Store 中输入关键词搜索到应用或者从外部点击 market链接跳转到 Play Store， 可以看到 Try Now Button。
- Browser
用户通过在浏览器点击 Google Search 结果中符合 Instant App Manifest 文件中注册的 host、pathPattern、pathPrefix 等规则的链接。
- Other
用户在 Telegram、Gmail 等第三方应用中点击了符合 Instant App Manifest 文件中注册的 host、pathPattern、pathPrefix 等规则的链接。

![InstantApp-Install-From-Gmail](/images/instant_app_install_from_gmail.gif)

安装后，以我的手机为例，除了在 Play Store 中可以搜到刚安装的 Instant App 外，你还可以在**设置->应用程序->Instant apps 选项**中看到，你也可以选择卸载或者查看该 App 支持捕捉哪些 host

![Find-InstantApp-in-Setting](/images/find_instant_app_in_setting.jpg)

## 值不值得做

作为开发者，在实现需求之前，我们也是需要去思考这个需求值不值得做，做了能为产品带来什么样的回报，如何去用数据反映这个需求好不好。在讨论这个话题之前，我们需要再一次明确 Instant App 的作用是什么：
>Instant App 本质是提供一种试用的服务给到用户

比如一个新闻聚合类的 App，用户试用 Instant App 后被内容所吸引或者认为阅读体验比网页优秀，那么可能就会愿意去下载安装完整版 App。所以对于 Instant App，我们应当以一个 **PV(启动 Instant App) -> Install Full App（安装完整版应用）** 的转化率、Session、新用户增加量等指标来衡量是否做的足够好。以我们自己的产品为例，上线 Instan App 后，转化率一直维持在 25% 以上，这意味着平均四个打开过 Instant App 的用户中就有一个会去安装 Full App。

在开发 Instant App 之前，我们也看过 Google Play Stories 分享的关于 [Vimeo 借助 Instant App 将 session 延长了130%，并且 Native App 用户增长了20%](https://developer.android.com/stories/instant-apps/vimeo)。

所以我认为对与专注于海外市场与有志于开拓海外市场的产品，Instant App 还是值得去做的。

## 如何改善转化率

对于如何改善转化率，我认为应当结合之前提到的用户安装与打开的路径来做。
1. 增加 Instant App 的曝光量
比如 Instant App 支持打开 Full App 分享出去的链接，Server 发给用户的邮件中的文章、商品之类的链接支持打开 Instant App，Smart banner(网页端上的提示下载 App 的 banner)支持点击打开 Instant App 等等
2. 关注改善非 Try Now Button 来源的 PV 量
Try Now Button 一般是用户在 Play Store 搜索 App 才能看到的，所以点击这个按钮的用户一般本来就是想要安装 Full App 的
3. 提供优质的体验与功能
最终还是要回到产品力层面，通过优秀的体验与功能来吸引用户安装 Full App

上线后可以在 Play Console 中的 PV 与转化数量等数据，当然你也可以通过自己添加统计事件来追踪。

![查看统计数据](/images/play_console_instant_app_statistics.png)

接下来将正式讲述 [Instant App 如何开发](/2019/08/25/android-instant-app-from-dev-to-publish-2/)
