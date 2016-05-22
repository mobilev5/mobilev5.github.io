title: iOS技术周刊第13期
date: 2016-05-22
tags: iOS技术周刊
categories: iOS开发
author: 阿P
---

本周推荐__“错误”的使用 Swift 中的 Extension__及__用 SwiftyDB 管理 SQLite 数据库__。

<!--more-->

### 新闻
- Apple 10亿美元融资滴滴！这体现了Apple方面对中国开发者的信心。
- iAd 广告平台即将关闭，API 将停用。苹果支持页面上建议开发者移除应用中的 iAd.Framework，因为从7月1日开始，广告将不会被显示。如果开发者的应用内仍然包含停用的 iAd API，应用会崩溃。iAd 平台关闭后，开发者需要选择第三方广告平台。

### Swift
- [“错误”的使用 Swift 中的 Extension](http://mp.weixin.qq.com/s?__biz=MzI4NjAzODk0OQ==&mid=2652684222&idx=1&sn=041c351d60c4770eaee7290e67aa08b2&scene=23&srcid=0522LYiXuLIAoCGS5EVx8P6P#rd) 相较于OC，Swift中的extension更加强大好用。但，在具体使用中还是要注意很多东西的。


### 本地数据存储
- [用 SwiftyDB 管理 SQLite 数据库](http://www.appcoda.com/swiftydb/) [`SwiftyDB`](https://github.com/Oyvindkg/swiftydb) 使用`SQLite`就需要手动建表么？`SwiftyDB`可以实现`model`与表的对应关系。(话说，我还是更喜欢Java中注解的方式)

### 架构设计
- [MVVM奇葩说](http://www.cocoachina.com/ios/20160520/16004.html) 文中解读了一些我对`MVVM`的一些错误的认识，推荐给大家。
- [Clang Attributes黑魔法小记](http://blog.sunnyxx.com/2016/05/14/clang-attributes/) Clang Attributes 是 Clang 提供的一种源码注解，在结构设计的某些场景下可以起到很大的作用。