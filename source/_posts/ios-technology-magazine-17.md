title: iOS技术周刊第17期
date: 2016-06-25
tags: iOS技术周刊
categories: iOS开发
author: makee
---

本周重点关注持续集成、App 动态化相关内容。

<!--more-->


### Swift

[Swift Style Guide](https://github.com/linkedin/swift-style-guide) 非常详细的 Swift 代码规范，想要统一团队成员代码风格的，可以参考下。

[Discovering Native Swift Patterns](https://realm.io/news/slug-nick-oneill-native-swift-patterns/) 实现特定的逻辑，代码有千万种写法，但真正契合语言理念的方式却不多，本文试图找到真正切合 Swift 语言的代码模式。

### 数据存储

[Concurrent Core Data, Now Easier Than Ever](http://holko.pl/2016/06/23/core-data/) 本文简单介绍了 iOS 10 中的 `NSPersistentContainer` 如何使得多线程操作在 Core Data 中变得简单。

### 界面布局

[『零行代码』解决键盘遮挡问题](http://draveness.me/keyboard/) 本文详细的分析了`IQKeyboardManager`的实现，揭开零行代码背后的密码。


### App 动态化

[iOS 热更新 － JSPatch 实现原理 + Patch 现场恢复](http://www.jianshu.com/p/41ed877aa0cd) 在 iOS 开发领域，由于 Apple 严格的审核标准和低效率，iOS 应用的发版速度极慢，稍微大型的 app 发版基本上都在一个月以上，所以代码热更新（_HotfixPatch_）对于 iOS 应用来说就显得尤其重要。

### 开源库

[轻量级的图表库 Graphs](https://github.com/recruit-mtl/Graphs) 使用 Swift 编写的开源图表库，使用起来非常简单。

[简洁优雅的网络库 Siesta ](https://github.com/bustoutsolutions/siesta) 又一款基于 Swift 开发的开源网络库，相比于 Alamofire，Siesta 提供了更多的功能，详细参看项目 README 中的对比表格。

[iOS 打包发布工具 ShenZhen](https://github.com/nomad/shenzhen) 没错，这个库的名字就是代表“深圳”（_爱国青年啊_）。作为持续集成中打包发布工具而言，还是非常强大的，目前已经支持了很多第三方托管平台了。

### 其它

[Jenkins 2.0 新时代：从 CI 到 CD](http://mp.weixin.qq.com/s?__biz=MzA5OTAyNzQ2OA==&mid=2649690418&idx=1&sn=a1112a9ce591329f3ed0c5d9dc451c3a&scene=23&srcid=0619NejoL0lLxZf5Fs8fXvEC#rd) Jenkins 终于迎来了 2.0 时代，2.0 最大的三个卖点分别是 Pipeline as Code、全新的开箱体验和 1.x 兼容性。小伙伴们，赶快着手升级起来吧！

[SHELL 编程之常用技巧](http://liwei.life/category/shell/) 作为 iOS 开发，对 shell 这种能提高工作效率的技术还是需要掌握的，这一系列文章简单易懂，可以作为入门基础。

[高效 MacBook 工作环境配置](http://xialeizhou.com/2016/06/23/%E9%AB%98%E6%95%88macbook%E5%B7%A5%E4%BD%9C%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/) 工欲善其事，必先利其器，工具永远都是用来解决问题的，没必要为了工具而工具，一切工具都是为了能快速准确的完成工作和学习任务而服务。本文记录 MacBook 整个配置过程，供新入手 MacBook 和觉得 MacBook 比较难用的同学参考。


