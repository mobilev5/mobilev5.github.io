title: iOS技术周刊第16期
date: 2016-6-19
tags: iOS技术周刊
categories: iOS开发
author: 吕品
---

本周重点关注WWDC，大家抓紧时间观看这一波视频吧，[WWDC 2016 Videos](https://developer.apple.com/videos/wwdc2016/)

<!--more-->


### Objective-C

* [Objective-C 消息发送与转发机制原理](http://yulingtianxia.com/blog/2016/06/15/Objective-C-Message-Sending-and-Forwarding/?utm_source=tuicool&utm_medium=referral) 本文不讲述开发者在消息发送和转发流程中需要做的事，而是讲述原理。消息发送和转发流程可以概括为：消息发送是 Runtime 通过 selector 快速查找 IMP 的过程，有了函数指针就可以执行对应的方法实现；消息转发是在查找 IMP 失败后执行一系列转发流程的慢速通道，如果不作转发处理，则会打日志和抛出异常。

### 工具

* [Malibu](https://github.com/hyperoslo/Malibu?plg_nld=1&utm_source=tuicool&plg_usr=1&plg_uin=1&utm_medium=referral&plg_auth=1&plg_nld=1&plg_dev=1&plg_vkey=1) 一个基于NSURLSession的强大的网络交互框架。

### 动画

- [UI篇OC篇&UIDynamic详解](http://www.cnblogs.com/iCocos/p/4602835.html) 介绍了iOS中物理引擎的使用方法，介绍了吸附，推动，重力，碰撞等物理引擎的效果。

### 其他

* [WWDC 后苹果最新 App Store 审核条款！](http://www.cocoachina.com/appstore/20160617/16740.html) WWDC 2016 大会之后，苹果公司发布了四个全新平台：iOS，macOS，watchOS 和 tvOS。并且在此之后，苹果应用商店审核条款也同时进行了更新——貌似不算进行了更新，简直就是重写！上个版本的 30 个章节被修改成了 5 大章节，但原版英文版字数从 5000 多个英文单词增加到了 6000 多个英文单词。