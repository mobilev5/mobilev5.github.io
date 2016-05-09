title: iOS技术周刊第11期
date: 2016-05-08
tags: iOS技术周刊
categories: iOS开发
author: 东子
---

本周推荐两篇性能优化的文章，关注一下Swift 3.0的改动。

<!--more-->

## 性能优化
[微信读书 iOS 性能优化总结](http://wereadteam.github.io/2016/05/03/WeRead-Performance/) 本文比较全面的介绍了微信读书 app 性能优化方案，总的来说解决线上问题还要靠提前预防，而性能指标和对应的检测工具是必不可少的。

[Re-architecting Pinterest's iOS app](https://engineering.pinterest.com/blog/re-architecting-pinterests-ios-app)
Pinterest 的一篇重构 app 的文章，介绍了最新版的一些性能优化方案，主要是解决 UI 卡顿问题，关键点是多线程和 AsyncDisplayKit。 为了此次重构，他们已经在最近的 3 周内更新了 4 个版本，体验了下感觉还是很不错的，但还是会有一些图片在展示完毕后会重新触发刷新，影响体验，看来这两大招也不是太轻易就可以玩的转的。最后还介绍了一个日志方案可以高效统计 app 的活动和性能，很遗憾并没有太多技术细节透漏。

## Swift
[Swift 3.0](https://swift.org/blog/swift-3-0-release-process/)
详细的改动见这里[swift-evolution](https://github.com/apple/swift-evolution)
对 3. 0的评价我们在此引用网络名人 Soulhacker 的[原话](https://twitter.com/soulhacker/status/728871743926902784) 「像 Swift 这样每个版本都破坏（无视）源码兼容性的语言也是没谁了 😂」

[BuildTimeAnalyzer-for-Xcode ](https://github.com/RobertGummesson/BuildTimeAnalyzer-for-Xcode)
Xcode 插件，检测比较耗费编译时间的 Swift 的代码，解决 Swift 因某些写法导致编译时间较长的问题。

## UI
[成熟的夜间模式解决方案](http://draveness.me/night/) 作者分享开发了 [DKNightVersion](https://github.com/Draveness/DKNightVersion) 的一些技术实现点。

# MVVM
[reactivecocoa-vs-rxswift](https://www.raywenderlich.com/126522/reactivecocoa-vs-rxswift) 作为后起之秀的 RxSwift 是不是更好用，看来也许会有结论（其实两个我都没怎么用过还....

## 工具
[Peer-to-peer NSLogger browsing](https://github.com/fpillet/NSLogger/pull/201)
来自 0xced 大神的Pull Request，可以让 NSLogger 无需在同一网络环境内即可远程打印 iOS 设备上的日志。

[Alfred 3 beta](https://www.alfredapp.com/preview/) 作为 Alfred 的 Mega Supporter 用户，第一时间用上了 Aflred 3，我想说找了好多粘贴板工具，最后 Alfred 终于增强了此功能，重归 Alfred 怀抱~ （拼音搜索啥时候支持呢...

## 后花园
[SwiftCon 期间的面基和八卦](http://blog.devtang.com/2016/05/07/swiftcon-2016-chat-notes/) 大V参加技术大会的一些见闻~

[CareKit](https://github.com/carekit-apple/CareKit) 苹果开源了 CareKit，值得一看。