title: iOS技术周刊第8期
date: 2016-4-17
tags: iOS技术周刊
categories: iOS开发
author: 宏伟
---

本周主要重点关注MVVM，SQLite数据库。

<!--more-->

### MVVM

[Service Oriented 的 iOS 应用架构](http://tech.glowing.com/cn/service-oriented-ios-architecture/) 值得一看的iOS应用架构。

### 设计模式

[蘑菇街App的组件化之路](https://mp.weixin.qq.com/s?__biz=MzA3ODg4MDk0Ng==&mid=402696366&idx=1&sn=ba8cbd75849b9657175c4b25bb0ac5b5&scene=2&srcid=0311YoPncvJPZrIVeGFIGtbN&from=timeline&isappinstalled=0&uin=MTcwODQ2NjAwMQ%3D%3D&key=710a5d99946419d9f3292e1b7809aa8ba9c2ff77c4026fd40cdcbb05c14087955671075ca2b03a38b906c3a437a2b7a7) 在组件化之前，蘑菇街 App 的代码都是在一个工程里开发的，在人比较少，业务发展不是很快的时候，这样是比较合适的，能一定程度地保证开发效率。慢慢地代码量多了起来，开发人员也多了起来，业务发展也快了起来，这时单一工程开发模式就会显露出一些弊端。

### SQLite数据库

[浅析SQLite的锁机制和WAL技术](http://liuduo.me/2016/04/02/sqlitelockandwal/) SQLite基于锁来实现并发控制。SQLite的锁是粗粒度的，并不拥有PostgreSQL那样细粒度的行锁，这也使得SQLite较为轻量级。当一个连接要写数据库时，所有其它的连接都被锁住，直到写连接结束它的事务。

### 页面布局

[AutoLayout下多行UILabel无法显示多行文本的问题](http://www.jianshu.com/p/d5d897ffe118) 在项目中的一个自定义UITableViewCell中有个多行UILabel，用来显示多行文本的。项目中用了第三方库Masonry来给视图添加约束。

### 工具

[IBM 出的 Swift 在线 Playground](https://swiftlang.ng.bluemix.net) 在线调试带有Playground功能的Swift代码，逼格不是一般的高~😄

### 后花园
* **[iOS再现安全漏洞](http://news.163.com/16/0414/14/BKKBS69800014AEE.html)**
* [如何作为一个好的程序员](https://github.com/ahangchen/How-to-Be-A-Programmer-CN/blob/master/README.md) 困难而高尚。将一个软件工程集体愿景变为现实，最困难的地方在于与你的同事和顾客相处。编程很重要，这需要强大的智力和技能。 但在好的程序员看来，相比构建一个让客户和各种各样的同事都满意的软件系统，（纯粹的）编程真的只是小孩子的玩意。在这篇文章里，我尝试尽可能简洁地总结那些当我21岁时，希望别人告诉我的事。