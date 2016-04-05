title: iOS技术周刊第3期
date: 2016-3-13
tags: iOS技术周刊
categories: iOS开发
author: 东子
---

本周重点推荐新开源的通用下载模块，另外`本地数据存储`和`页面布局`标签有新内容了~

<!--more-->

## 开源
* [通用下载模块](https://github.com/HujiangTechnology/HJMURLDownload)开源了，基于 NSURLSession 开发，支持多任务、后台下载、断点续传。介绍文章见[这里](http://mobilev5.github.io/2016/03/13/meeting-common-urldownloader/)

## Swift
* [Swift 下 OpenGL 截图容易导致的内存泄露](http://firestudio.cn/2016/03/11/take-screenshot-of-eaglview-may-easily-cause-memory-leaks-in-swift/)

## Hybrid
* [Basecamp 新出的一个 hybrid 框架]( https://github.com/turbolinks/turbolinks-ios) Basecamp 一直都在尝试 hybird 这套开发方式，技术方案选用了 WKWebView，需要 iOS 9 以上，初步看下来是一套很不错的整套方案，期待后续文章专门介绍。

## 页面布局
* [UIStackView介绍](http://www.jianshu.com/p/654eaf0c63c1)，iOS 9之后增加了新的布局神器UIStackView, 文中介绍了UIStackView的常用属性，并根据相应的demo阐述其在实际生产中的作用。

## 本地数据存储
* [走进 Core Data 的世界]( http://liuduo.me/2016/03/12/gointocoredata/) ，Core Data作为一个OS X和iOS中自带的数据存储框架，很早就存在了。因其存在一些缺点使得很多人放弃使用而采用其它方案。但苹果依然在每个iOS版本中不断对其改进，例如在iOS 8中加入了BetchUpdate，解决了之前一直令人诟病的批量更新问题，使得Core Data更为强大。
	
## 动画
* [LayerPlayer](https://github.com/scotteg/LayerPlayer)，LayerPlayer - Layer Player explores the capabilities of Apple's Core Animation API
一套可以快速熟悉 Core Animation API 的项目，可以调节各种参数，非常适合入门学习

## 工具
* [GT](https://github.com/TencentOpen/GT) 腾讯开源的一套性能检测工具
* [Injection for Xcode](https://github.com/johnno1962/injectionforxcode) 结合 [DeveloperExtras.m](https://github.com/artsy/eigen/blob/master/Artsy%2FView_Controllers%2FApp_Navigation%2FARTopMenuViewController%2BDeveloperExtras.m)  用就很方便了，每次触发 Injection，然后重新 pop 和 push 当前页，可以不需要编译实现快速代码预览
视频可以看[这里](http://artsy.github.io/blog/2016/03/05/iOS-Code-Injection/)。

## 后花园
* gitignore 很多时候我们都是参考 [gitignore.io](https://www.gitignore.io/api/xcode%2Cobjective-c%2Cswift), 但具体情况还是需要具体分析，下面两种情况需要增改 gitignore 文件
	* git 库 为 Library 库时，完全可以过滤掉 *.xcworkspace 和 Podfile.lock 文件，参考 [ORStackView](https://github.com/orta/ORStackView/blob/master/.gitignore)
	* git 库 为项目库时，可以考虑忽略 xcshareddata *.xcscmblueprint 等 
	参考开源项目 [eigen](https://github.com/artsy/eigen/blob/master/.gitignore)
此外当你的 gitigonre 发生了变化，新增了一些忽略文件的时候，一定要记得使用 ```git rm --cached ``` 命令删除 git 库的索引，不然 gitignore 不会生效。

* @import 和 使用 Objective-C++ 生成的 *.mm 的文件不兼容，遇到这种情况暂时只能回退到老的方式，期待后续支持。参考 https://github.com/stripe/stripe-ios/issues/220

