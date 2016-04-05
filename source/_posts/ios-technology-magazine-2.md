title: iOS技术周刊第2期
date: 2016-3-6
tags: iOS技术周刊
categories: iOS开发
author: 元相
---

本周重点在性能优化和Hybrid方面，包含`AsyncDisplayKit`的实践，RN相关等，另外App开发人员也应该了解一些API设计，本期重点介绍API建模工具RAML，希望大家能够有所收获。

<!--more-->

## 原创
* [深度实践 AsyncDisplayKit](http://mobilev5.github.io/2016/03/05/asyncdisplaykit-practice/)
* [用RAML来构建API文档](http://mobilev5.github.io/2016/03/06/using-raml-build-api-doc/)

## Hybrid
* [UIWebView与JS的深度交互](http://www.cocoachina.com/ios/20150811/12985.html)，禁用获取的HTML文本中自带的 < img > 标签自动加载，并把下载图片的操作放在native端来处理，提高响应速度并且节省用户流量，同时也能在本地对图片各种操作，一个不错的交互实践。
* [JSPatch 实现原理详解](https://github.com/bang590/JSPatch/wiki/JSPatch-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86%E8%AF%A6%E8%A7%A3)
* [React-Native各类学习资源](https://github.com/ele828/react-native-guide#%E5%B7%A5%E5%85%B7)，资料很全面，包含开源app，以及一些工具和组件。

## 性能优化
* [iOS 保持界面流畅的技巧](http://blog.ibireme.com/2015/11/12/smooth_user_interfaces_for_ios/#more-41893)，很详细的分析了界面卡顿的原因，以及对应的解决方法的研究方向（*异步绘制*，*预排版*，*预渲染*，*全局并发控制*）和实践，相当不错哦，值得你拥有。
* [内存恶鬼drawRect - 谈画图功能的内存优化](http://mp.weixin.qq.com/s?__biz=MjM5NTIyNTUyMQ==&mid=447105405&idx=1&sn=054dc54289a98e8a39f2b9386f4f620e&scene=23&srcid=0108RhyzhXk9wUwQvnW3cmZT#rd)

## 网络
* [iOS的TCP/IP协议族剖析&&Socket](http://www.jianshu.com/p/cc756016243b)，主要介绍了TCP/IP的工作原理以及Socket的原理和实现细节。

## 绘图
* [神奇的Transform](http://ciechanowski.me/)，看看我们可以通过transform来做点什么，简直太好玩了，不服来看😏😏

## 工具
* [Swift基准测试套件](https://github.com/apple/swift/tree/master/benchmark)，包含基准测试驱动以及显示性能的一些度量，协助开发者创建更快更高效的代码。
* [Chisel-LLDB命令插件，让调试更Easy](http://blog.cnbluebox.com/blog/2015/03/05/chisel/)
* [Redux可预测化的状态管理](http://camsong.github.io/redux-in-chinese/index.html)

## 后花园
* [程序员VS武林高手：技术为外功，思维乃内力](http://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=402973995&idx=1&sn=87625ad8a3e3d5b16baa0890d6eedfe8&scene=23&srcid=03050zf9BxGthj6HoRiTCmb3#rd)，真气雄浑，滚滚不可测，吐气如剑，拈花摘叶，老司机今天带你重新认识当代武林高手－程序员。
* 大妈喊你还债了，[年前挖的坑都填了吗？技术债务偿还计划](http://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=402964742&idx=1&sn=60a657a4eeca714bee80e861406c6443&scene=23&srcid=0305FCtWZQFUXz2Xx4PePzCx#rd)😄😄
* [你不可错过的50部电影清单！（附评分、推荐语）](http://www.jianshu.com/p/ff4a34a5a136)
* [并发之痛 Thread，Goroutine，Actor](https://mp.weixin.qq.com/s?__biz=MzAwMDU1MTE1OQ==&mid=404242829&idx=1&sn=aacddf1c2c828281e6202eff8cd374f5&scene=1&srcid=0302Q1dT8nUezoEZLZzistUl&key=710a5d99946419d96e1c7481a2d8dae6400769cbc8bc9015cca38f9694d8c1ae46c0ff571ec714de57c149dd6aef6796&ascene=0&uin=Mzg1OTg2NQ%3D%3D)

