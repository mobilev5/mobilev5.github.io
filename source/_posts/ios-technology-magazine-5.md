title: iOS技术周刊第5期
date: 2016-3-27
tags: iOS技术周刊
categories: iOS开发
author: 罗晟
---

本周重点关注iOS 9.3，iPhone SE，Azer NPM撤包事件；iOS 微信浏览器打开App的解决方案；现有 iOS 端图片缓存解决方案

<!--more-->

## 新闻

* [iOS 9.3 中的动态库加载速度](https://github.com/stepanhruda/dyld-image-loading-performance?utm_campaign=iOS%2BDev%2BWeekly&utm_medium=email&utm_source=iOS_Dev_Weekly_Issue_243) 本周 iOS 更新到了 9.3，这个代码库发现在 iOS 9.3 中，动态库的加载速度比之前快了 75% 左右。
* Azer NPM 撤包事件 [链接1](http://zhuanlan.zhihu.com/codestory/20669077) [链接2](http://zhuanlan.zhihu.com/ibuick/20671763) 这是本周发生在 Node 社区中的一件大事情。作者因为对 NPM 公司处理版权问题的不满，撤下了自己所有的代码，导致很多著名项目如 Babel、React Native 等因为依赖问题而无法正常使用。这也引起了人们对于集中式依赖管理器的思考——看来通过 CocoaPods 引用的项目很有必要将第三方的代码也一起放进自己的仓库中。

## 综合

* [iOS 微信浏览器直接打开 App 的解决方案](http://mobilev5.github.io/2016/03/23/wexin-open-app/) 本文介绍了如何通过 iOS 9 中的 Universal Links 特性来突破微信浏览器的限制，直接在微信跳转至自己的 app。
* [Outlets: Strong! Or Weak?](http://scottberrevoets.com/2016/03/21/outlets-strong-or-weak/) 通过 @IBOutlet 定义的变量，到底应该是 strong 还是 weak 呢？

## 网络交互

* [HTTP 缓存](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=zh-cn) Google 对于 HTTP 缓存机制的介绍。
* [现有 iOS 端图片缓存解决方案](http://mobilev5.github.io/2016/03/22/image-cache-solution/) 移动 App 中使用网络图片的情况很常见，如果缓存问题处理不好会很大程度上影响 App 的性能。本文简单介绍一下图片缓存流程及 GitHub 上现有解决方案。

## 逆向

* [逆向工程实战分享](http://mobilev5.github.io/2016/03/24/crack-share/) 本文介绍了逆向工程的基本概念，以及分享一个逆向工程的实例。

## Swift

* [Swift 2.2 发布](https://swift.org/blog/swift-2-2-released/) Swift 2.2 发布，比较大的变化是用 `associatedtype` 代替了原先的 `typealias` 关键字；以及用 `#selector` 来指定 Objective-C 中的 selector。文章中有更详细的更新内容。
* [Surprises with Swift Extensions](https://pspdfkit.com/blog/2016/surprises-with-swift-extensions/?utm_campaign=iOS%2BDev%2BWeekly&utm_medium=web&utm_source=iOS_Dev_Weekly_Issue_243) 如果通过 Swift 给 Objective-C 中的类加扩展仍然需要添加前缀。

## Hybrid

*  [You Hybrid App is Going to Kill You](https://medium.com/teach-code/your-hybrid-app-is-going-to-kill-you-416041d27eac#.hzajpo3y1) 关于 Hybrid 应用，来听听一些不同的声音。

## 页面布局

* [MCUIViewLayout](https://github.com/mirego/MCUIViewLayout) 一个页面布局的框架。

## 动画

* [iOS Bubble Animation Tutorial](http://www.jackrabbitmobile.com/design/ios-bubble-animation-tutorial/) 一个实现气泡动画的教程。