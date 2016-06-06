title: iOS技术周刊第15期
date: 2016-6-5
tags: iOS技术周刊
categories: iOS开发
author: 罗晟
---

WWDC 马上就要来了，会有什么样的惊喜和期待呢？

<!--more-->

### Swift

* 本周 Swift 3.0 放出了[第一个预览版的下载](http://ericasadun.com/2016/05/31/swift-3-0-preview-1-release-branch)，但过了几天之后又[撤下来了](http://ericasadun.com/2016/06/02/apple-withdraws-swift-3-0-preview-1/)。苹果解释说这仅仅是 Swift 3.0 Preview 1 的一个 snapshot，并不是最终版本，为了避免带来困扰才撤下了链接。看起来 Swift 3.0 还有挺长的一段路要走。
* [Cacao](https://github.com/PureSwift/Cacao) 这是纯 Swift 的一个跨平台 UIKit 实现，目前已经支持 Linux。

### 工具

* [JSONExport](https://github.com/Ahmed-Ali/JSONExport) 一个可以根据 JSON 来生成 model 的 Mac 应用。支持多种语言。
* [FastStub-Xcode](https://github.com/music4kid/FastStub-Xcode) 一个 Xcode 插件，可以根据头文件中的声明在 .m 中自动补全实现缺失的方法。
* [Documenting Your Swift Code in Xcode Using Markdown](http://www.appcoda.com/swift-markdown/) Swift 中的方法注释其实是支持 Markdown 的，这篇文章进行了一个比较详细的讲解。

### 动画

- [Expanding Collection](https://github.com/Ramotion/expanding-collection) 一个使用 Collection View 实现的卡片的展开和收起的动画效果。

### 其他

* [iOS 启动连续闪退保护方案](http://wereadteam.github.io/2016/05/23/GYBootingProtection/) 来自微信阅读团队，在启动的时候检测闪退，并可以针对闪退进行错误上报以及一定的错误修复。
* [Clang Attributes 黑魔法小记](http://blog.sunnyxx.com/2016/05/14/clang-attributes/) Clang Attributes 是 Clang 提供的一种源码注解，方便开发者向编译器表达某种要求。