title: iOS 技术标签的知识范围及学习资源
date: 2016-2-27 19:30:00
toc: true
tags: 基础
categories: iOS开发
author: Yomi
---

本文旨在为每个技术标签的进阶提供一些参考，Hybrid，Swift，性能优化，设计模式，页面布局，本地数据存储，网络交互，音视频等。

<!--more-->

## Hybrid

### 初级

1. 熟练掌握`UIWebView` 的使用
2. 熟练掌握`UIWebView`和`HTML`页面的交互，包括`拦截请求`以及`JavascriptCore`
3. 熟练掌握譬如 **Charles** 等抓包工具的使用

### 中级

1. 熟练掌握`WKWebView`的使用，掌握其与`UIWebView`的不同之处
2. 熟悉整个`HTML`页面的加载流程，熟知常见的`DOM`元素以及相关事件
3. 熟练使用 **Safari** 对内嵌页面进行调试，掌握基本的`Javascript`书写
4. 了解`Javascript`跨域安全问题，掌握`NSURLCache`、`NSURLProtocol`的使用

### 高级

1. 熟练掌握`Javascript`以及`HTML5`特性，可独立完成一套完整的`HTML5`页面
2. 阅读`WebKit`以及`JavascriptCore`源码，了解它们的核心逻辑
3. 熟悉`HTTP`协议，以及基于`HTTP`的通讯协议，如`Soap`、`XMLRPC`等
4. 熟悉 **Web服务器** 的基本工作原理，可在应用内内嵌，如`GCDWebServer`、`CocoaHTTPServer`等

### 学习资源

* Apple 开发文档
* W3School：http://www.w3school.com.cn/
* HTML5中国：http://www.html5cn.org/
* HTTP协议详解：http://blog.csdn.net/gueter/article/details/1524447
* GCDWebServer：https://github.com/swisspol/GCDWebServer
* CocoaHTTPServer：https://github.com/robbiehanson/CocoaHTTPServer
* [Weex](http://mp.weixin.qq.com/s?__biz=MzAxNDEwNjk5OQ==&mid=401883911&idx=1&sn=938df448000d3c193a146c4f41b36f6c&scene=23&srcid=02239yd9HOhP7shWCE0LP0cH#rd)
* [JSPatch](Ehttp://mp.weixin.qq.com/s?__biz=MzAwNDY1ODY2OQ==&mid=400011053&idx=1&sn=81ed095f6fb9f7a4345ff50285264be1&scene=23&srcid=0223rWbN9Rk4GDeBuI78JmGC#rd)
* [React-Native](http://facebook.github.io/react-native/)

## Swift
### 入门
1. 熟练基本语法
2. 熟练OC混编能力
3. 了解函数式编程范式

### 进阶
1. 熟练掌握Swift高级用法
2. 深入剖析Swift源码

### 学习资源
* [The Swift Programming Language(Swift 2.1)](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/)
* [Using Swift with Cocoa and Objective-C (Swift 2.1)](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/BuildingCocoaApps/)
* [Swift 官网](https://swift.org/)
* [Swift 源码](https://github.com/apple/swift)
* [Swift翻译组](http://swift.gg/)
* [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa)

## 性能优化

### 入门
1. 熟悉Instruments基本使用，内存泄漏监测
2. 了解FPS，图层绘制基本原理
3. 了解App的加载及运行机制
4. 了解内存分配机制
5. 了解多线程机制

### 进阶
1. 熟练掌握Instruments各类分析工具
2. 熟练掌握FPS，内存，线程等运行机制
3. 深入DTrace细节
4. 自动化性能测试

### 学习资源
1. [DTrace 介绍](http://www.solarisinternals.com/wiki/index.php/DTrace_Topics_Intro)
2. [Apple 文档](https://developer.apple.com/library/prerelease/watchos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/)
3. [测试定位内存泄露](https://github.com/tapwork/HeapInspector-for-iOS)
4. FPS提升-AsyncDisplayKit: 
   * [Github 源码](https://github.com/facebook/AsyncDisplayKit)
   * [视频介绍](http://www.raywenderlich.com/86365/asyncdisplaykit-tutorial-achieving-60-fps-scrolling)

​	
## 设计模式

### 初级

1. 深入理解面向对象设计，并理解与编程范式之间的区别
2. 熟练掌握面向对象的基本设计原则（_SOLID_）
3. 熟悉`GoF`的23种面向对象设计模式
4. 熟悉一些辅助设计的框架，包括`AOP`、`IoC`等

### 中级

1. 熟练掌握常见的一些架构模式，如`MVC`、`MVP`、`MVVM`等
2. 熟练掌握分层架构（_常见的三层架构、N层架构等_），以及分层的基本原则
3. 熟悉常见的软件体系结构风格，包括 **管道-过滤器式**、**层次式**、**面向对象式**，以及它们之间的区别

### 高级

1. 熟悉常见的软件开发模式，包括敏捷、瀑布、迭代等，包括常见的`XP`、`SCRUM`等
2. 熟悉常见的软件设计方式，包括 **领域驱动设计**、**模型\数据驱动设计**、**测试驱动设计** 等
3. 熟悉企业级应用的架构模型，包括 **分布式**、**负载均衡** 等

### 学习资源

* 面向对象设计原则：http://www.cnblogs.com/feipeng/archive/2007/03/02/661840.html
* 23种设计模式：http://blog.csdn.net/longyulu/article/details/9159589
* 分层架构设计原则：http://www.cnblogs.com/chencheng/archive/2012/07/05/2575406.html
* 领域驱动设计：http://www.cnblogs.com/xishuai/category/572887.html
* 书籍:《敏捷软件开发：原则、模式与实践》
* 博客：http://casatwy.com/

## 页面布局

### 入门
1. 熟悉Xib, Storyboard工具
2. 熟悉Autolayout，SizeClass，UIStackView，CGRect等布局方式

### 进阶
1. iOS 多设备的布局优化
2. 深入理解布局相关的源码与机制
3. 多类布局方式的最佳实践

### 学习资源
1. 像素是如何绘制到屏幕上的: http://objccn.io/issue-3-1/
2. 页面布局，页面渲染的原理: https://developer.apple.com/library/ios/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/GraphicsDrawingOverview/GraphicsDrawingOverview.html#//apple_ref/doc/uid/TP40010156-CH14-SW14
3. AutoLayout: http://www.cocoachina.com/ios/20151021/13825.html
4. SizeClass
   * http://blog.callmewhy.com/2014/09/12/learn-ios8-size-class/    
   * http://onevcat.com/2014/07/ios-ui-unique/
5. UIView及其扩展接口深入理解 http://my.oschina.net/w11h22j33/blog/208574?fromerr=4ylkDitz
6. MASConstraint   https://github.com/SnapKit/Masonry

## 本地数据存储
### 入门
1. 熟悉各类本地数据存储，SQLite(FMDB)，CoreData，UserDefaults
2. 熟悉SQL基本语法
3. 了解NOSQL基本原理，并熟悉YapDatabase, Realm等
4. 熟练使用数据库查看工具SQLite Professional

### 进阶
1. 最佳实践

### 学习资源
  - [SQLite 官网](http://www.sqlite.org/)
  - CoreData
    - [Core Data Programming Guide](https://developer.apple.com/library/watchos/documentation/Cocoa/Conceptual/CoreData/index.html)
    - [框架详解](http://blog.csdn.net/kesalin/article/details/6739319)
    - [手动编写代码](http://blog.csdn.net/kesalin/article/details/6746117)
    - [使用绑定1](http://blog.csdn.net/kesalin/article/details/6757279)
    - [使用绑定2](http://blog.csdn.net/kesalin/article/details/6757412)
  - Realm
    - [Realm 官网](https://realm.io/)
    - [基础教程](http://www.jianshu.com/p/052c763d5693)
  - 源码
      - https://github.com/ChrislosChen/ANKeyValue
      - https://github.com/yapstudios/YapDatabase
      - https://github.com/ccgus/fmdb
      - https://github.com/marcoarment/FCModel

## 网络交互
### 知识范围
  - BSD Socket
  - CFNetwork
  - 缓存
  - HTTP / TCP 协议

### 学习资源
  - [网络缓存与离线存储](https://github.com/ChenYilong/ParseSourceCodeStudy/tree/master/02_Parse%E7%9A%84%E7%BD%91%E7%BB%9C%E7%BC%93%E5%AD%98%E4%B8%8E%E7%A6%BB%E7%BA%BF%E5%AD%98%E5%82%A8)
  - [CFNetwork](http://blog.csdn.net/kesalin/article/details/8801156)
  - [Socket](http://blog.csdn.net/kesalin/article/details/8798039)
  - [NSStream](http://blog.csdn.net/kesalin/article/details/8867781)
  - [iOS应用架构谈 网络层设计方案](http://casatwy.com/iosying-yong-jia-gou-tan-wang-luo-ceng-she-ji-fang-an.html)


## 音视频
### 学习资源
* 《Learning-AV-Foundation》 http://www.amazon.com/Learning-Foundation-Hands-Mastering-Framework/dp/0321961803
* https://developer.apple.com/av-foundation/