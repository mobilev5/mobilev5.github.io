title: iOS技术周刊第9期
date: 2016-4-24
tags: iOS技术周刊
categories: iOS开发
author: 吕品
---

本周主要重点关注性能优化，设计模式。

<!--more-->

### 性能优化

[内存恶鬼drawRect](http://bihongbo.com/2016/01/03/memoryGhostdrawRect/) 标题有点吓人，但是对于drawRect的评价倒是一点都不过分。在平日的开发中，随意覆盖drawRect方法，稍有不慎就会让你的程序内存暴增。

[iOS 高效添加圆角效果实战讲解](http://www.jianshu.com/p/f970872fdc22) 圆角（RounderCorner）是一种很常见的视图效果，相比于直角，它更加柔和优美，易于接受。但很多人并不清楚如何设置圆角的正确方式和原理。

[UIKit性能调优](https://yalantis.com/blog/mastering-uikit-performance/) 虽然很多文章和WWDC会议致力于UIKit的表现，但是这个话题对于很多iOS开发者仍然是模糊的。由于这个问题，我们把软件的快速与凉爽的用户界面的问题放在一起讨论。

### 设计模式

[组合化繁为简的力量](http://blog.makeex.com/2016/04/23/the-design-pattern-of-composite/) 本篇文章，主要是介绍了 GoF 23 种设计模式中的组合模式，这算是结构型设计模式中的平民级模式，因为它简单、易用，但效果，往往能助你化繁为简。

### 页面布局

[iOS皮肤框架](http://mp.weixin.qq.com/s?__biz=MzIwMzI0MTI5MA==&mid=2247483670&idx=1&sn=b1a76605965632fd078d7e3adaca19ee&scene=2&srcid=0423Hg7weJjEwBbLoHmsByTu&from=timeline&isappinstalled=0#wechat_redirect) JJSkin皮肤框架适用于所有iOS APP，如果你是一名iOS开发工程师，希望你读完本文，并且使用JJSkin在你的项目中。

[Beginning Auto Layout Tutorial](https://www.raywenderlich.com/50319/beginning-auto-layout-tutorial-in-ios-7-part-2) 这片文章是关于auto layout的介绍及使用，讲述的非常详细。

### 动画／绘图

[GPUImage2](https://github.com/BradLarson/GPUImage2) GPUImage 2 is the second generation of the GPUImage framework, an open source project for performing GPU-accelerated image and video processing on Mac, iOS, and now Linux.

[iOS粒子系统CAEmitterLayer](http://www.cnblogs.com/densefog/p/5424155.html) 关于CAEmitterLayer的介绍及各个参数的比较详细的介绍的原创文章，对于想使用粒子系统的可以进去查看下。

### 后花园
* [阿里宣布开源Weex](http://mp.weixin.qq.com/s?__biz=MzAxNDEwNjk5OQ==&mid=2650400054&idx=1&sn=a39f00d0313f91b888d7feba57439a60&scene=0#wechat_redirect)对于移动开发者来说，Weex主要解决了频繁发版和多端研发两大痛点，同时解决了前端语言性能差和显示效果受限的问题。

* [iOS遗留系统重构实践](http://mp.weixin.qq.com/s?__biz=MzA3ODg4MDk0Ng==&mid=2651112090&idx=1&sn=7e00d0da704f99ef93ffd815731f1fd9&scene=0#wechat_redirect) 本文分享了一个工程浩大的重构实践。