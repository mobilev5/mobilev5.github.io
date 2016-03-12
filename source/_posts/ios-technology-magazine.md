title: iOS技术周刊第1期
date: 2016-2-28 20:30:00
tags: iOS技术周刊
categories: iOS开发
author: Yomi
---

本技术周刊旨在汇总每周的技术知识及行业动态，内容严格按照技术标签分类，望为每位iOS伙伴提供一些帮助，本篇涉及Swift，Hybrid, 性能优化等技术标签。

<!--more-->

## 沪江原创

* [探索Today Extension的奥秘](http://hujiangtechnology.github.io/2016/03/01/swift-today-extension-practice/)
* [iOS 技术标签知识范围及学习资源整理](http://hujiangtechnology.github.io/2016/02/27/tech-tags-study/)

## Swift

* [Is Apple Using Swift in iOS ?](https://medium.com/@ryanolsonk/is-apple-using-swift-4a6c80f74599#.m9wr5tr7k)  目前看来仅Calculator采用Pure Swift，任重道远
* [Increasing Performance by Reducing Dynamic Dispatch](https://developer.apple.com/swift/blog/?id=27)
* @Swift大会1月10号在北京召开，大会资源如下：
  * [Keynote](https://github.com/atConf/atswift-2016-resources)
  * [视频](http://www.imooc.com/video/11118)
* [使用 Swift 的面向协议编程定义 Segue 标识](http://swift.gg/2016/02/01/protocol-oriented-segue-identifiers-swift/)
* [Swift 也用 Jira 来管理Bug](https://bugs.swift.org/projects/SR/issues/SR-581?filter=allopenissues)

## Hybrid

* Chrome App 48.0.2564.87 版本开始使用WKWebView，https://appsto.re/cn/NVp8F.i
* [极致的Hybrid-Con2015](http://vdisk.weibo.com/s/sWYf0noAplM?from=page_100505_profile&wvr=6)

## 疑难杂症

* 当 iPhone 的应用跑在 iPad iOS 8.0.2 上时，编译出 <XIBName>~iphone.nib 这样的文件是无法识别的，会 Crash。http://koze.hatenablog.jp/entry/2015/08/21/093000 这篇文章介绍到了，解决这样的问题需要关闭 Size Class

## 越狱破解

* [在非越狱手机上进行App Hook](http://mp.weixin.qq.com/s?__biz=MjM5NTIyNTUyMQ==&mid=455373552&idx=1&sn=522bdeaa8a3a8a37423634f4e6ad0334&scene=23&srcid=0223kyi4SqD0Mu6UafGoaQ0T#rd)

## 性能优化

* View复用
  * [Telegram iOS版本](https://github.com/peter-iakovlev/telegram)，复用各种View，alloc的view都先缓存一下
  * [iOS 高性能异构滚动视图构建方案 —— LazyScrollView](http://pingguohe.net/2016/01/31/lazyscroll.html)
* [腾讯性能测试开源](https://github.com/TencentOpen/GT)
* [动态库加载性能检测及解决方案](https://github.com/stepanhruda/dyld-image-loading-performance)
* Watch Dog强制杀掉App，短时间内线程数达到峰值
  * ​http://stackoverflow.com/questions/25848441/app-shutdown-with-exc-resource-wakeups-exception-on-ios-8-gm
  * YYKit中有个方案可供参考：https://github.com/ibireme/YYDispatchQueuePool

## GitHub源码

* [告别“伪连接”！如何高效检测iOS中的真实连接状态](http://mp.weixin.qq.com/s?__biz=MzA3ODg4MDk0Ng==&mid=402123943&idx=1&sn=9de12e74c32510a245d6195836653d5f#rd) . https://github.com/dustturtle/RealReachability 
* A queue to keep and reusing objects. https://github.com/acoomans/ACReuseQueue
* Telegram iOS App. https://github.com/peter-iakovlev/telegram

## 工具

* CocoaPods 1.0.0.beta 4,  [查看1.0 更新记录](http://blog.cocoapods.org/CocoaPods-1.0/)
* [Xcode插件管理器](https://github.com/alcatraz/Alcatraz)
* 翻墙
  * https://shadowsocks.com/  这个翻墙软件，速度稳定，看YouTube上的视频720P极其流畅 
  * [海豚畅游](http://do1.glbproxy.tk/html/ec/index.html)， 目前仅支持Chrome和Android
* ​戴尔（DELL）专业P2415Q 23.8英寸16:9宽屏 LED背光 4K液晶显示器，效果与Macbook非常一致
* [Octotree](https://github.com/buunguyen/octotree/)， Code tree for GitHub and GitLab
* 技术学习视频网站，须翻墙，http://www.raywenderlich.com/
* [Replica - Web Developer Tool](https://itunes.apple.com/cn/app/replica-web-developer-tool/id1068196306?l=en&mt=8)，移动设备的Charles
* [`程序员鼓励师`的插件](https://github.com/poboke/Miku)

## 后花园

* Apple Pay来了 https://developer.apple.com/apple-pay/get-started/cn/
* 花生地铁WiFi_测试，可以在地铁里愉快地上网了
* 神奇的iOS 9.2.1版本 http://toutiao.com/i6252784551433601537/
* [2016 QCon 北京主题招募中](http://2016.qconbeijing.com/)
* 干货集中营， http://gank.io/
* [关于烂代码的那些事 － 为什么每个团队存在大量烂代码](http://mp.weixin.qq.com/s?__biz=MzAwMDU1MTE1OQ==&mid=403520625&idx=1&sn=c59f3944760a055ee7b6d1dda4431e0a&scene=23&srcid=0113zrH6MRCoo0NBX3AOOSDO#r)