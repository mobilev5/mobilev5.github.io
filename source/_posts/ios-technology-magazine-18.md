
title: iOS技术周刊第18期
date: 2016-07-03
tags: iOS技术周刊
categories: iOS开发
author: 沸沸腾
---

本期主要干货比较多，包括iOS当前热门的WebAPP技术，以及iOS10新特性，iOS安全问题等一系列干货文章，希望各位多看多了解。

<!--more-->

# Hybrid

[iOS端网页中js与OC如何交互(HybridApp)](http://www.codertian.com/2016/04/22/iOS-javascriptcore-call-native/) 在这里作者介绍了HybridApp的核心思想，既怎么使用JavaScriptCore框架进行JS语言与OC语言之间的相互调用，从而实现APP中某个模块的web化。

[ReactNative iOS源码解析（一）](http://awhisper.github.io/2016/06/24/ReactNative%E6%B5%81%E7%A8%8B%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/) ReactNative这个词目前是越来越火，天猫，携程等大型APP早已经在某个模块上使用了ReactNative，本文介绍了ReactJS和Native是怎么结合到一起的，并且分析了ReactNative（iOS）的底层结构与原理，这篇文章只是这方面技术分析的一部分，感兴趣的骚年可以关注作者的后续文章。

# 页面布局

[iOS逆向Reveal查看任意app 的界面](http://www.jianshu.com/p/060745d5ecc2) iOS逆向工程一直看起来都非常高大上，这边文章介绍通过Reveal来查看某些APP的界面信息，可以帮助我们快速了解一个陌生的APP界面结构的组成，跟着作者一步一步做下去你将会受益匪浅。

# 安全
[浅谈iOS应用安全自动化审计](http://www.cocoachina.com/ios/20160629/16859.html) iOS安全问题越来越受到我们的重视，iOS的安全漏洞也在快速增长中，针对我们自己的APP是不是可以开发一种检测软件来检测APP的安全性呢？本文就抛砖引玉的浅谈了作者在iOS审计方便所做的探索，希望在这方面能给我们一个不同思考起点。

# 内存管理

[alloc、init你弄懂50%了吗？](http://www.cocoachina.com/ios/20160627/16823.html) 关于OC的``alloc``和``init``你是不是就知道分配内存和初始化？就问你一句是不是真的了解这两个步骤究竟干了些什么？看完本篇文章我的内心是崩溃的😂，心里久久不能平静，不说了我去温习《计算机组成原理》去了，不对，《操作系统》也要去看看😂。

# 其它
[stackoverflow上关于iOS的票数最多（最常见）的15个问题](http://www.jianshu.com/p/ec0618c6cfa9) stackoverflow就是一个大型的开放的FAQ平台，你是问题制造者，也是答案提供者。这篇文章作者列出至今stackoverflow上关于iOS的票数最高（最常见）的15个问题，仅为了大家能够更方便、直接、快速的找到自己想要的答案，也许其中某个（些）问题就是你已经碰到或者即将碰到的。

[纯IPv6环境App适配的坑](http://mrpeak.cn/blog/ipv6/) 苹果公司在2016年6月1号开始，强制所有app必须支持纯IPv6的网络环境。这项举措将对IPv6的普及起到一定的推动作用，也体现了Apple作为国际大厂的担当。但是APP要支持纯IPv6的环境肯定会遇到一些小坑，本篇文章作者给了我们一些学习建议以及他遇到的一个坑。为此作者还录制了视频，但貌似作者快坚持不下去了😅。

[WWDC2016 Session笔记 - iOS 10  推送Notification新特性](http://www.jianshu.com/p/9b720efe3779) 这篇是关于iOS10中Notification的最新特性，废话不多说，干货一个。
