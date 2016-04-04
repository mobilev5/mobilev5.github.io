title: iOS技术周刊第6期
date: 2016-4-1
tags: iOS技术周刊
categories: iOS开发
author: 阿P
---

本周重点关注iPhone SE发售；SQLite的锁机制和WAL技术。

<!--more-->

## 本地数据存储
- [浅析SQLite的锁机制和WAL技术](http://liuduo.me/2016/04/02/sqlitelockandwal/) SQLite的粗放型锁机制导致死锁的可能性，配合事务避免死锁。v3.7的WAL技术突破了处理高并发的瓶颈，极大的提升了性能。

## Swift
- [Swift Runtime分析：还像OC Runtime一样吗？](http://mp.weixin.qq.com/s?__biz=MzA3ODg4MDk0Ng==&mid=403153173&idx=1&sn=c631f95b28a0eb4b842a9494e43a30e5&scene=0#wechat_redirect) 纯Swift没有动态特性，但可以通过在方法、属性前添加`dynamic`修饰符来获取动态特性；继承Objective-C类后，继承的方法、属性有动态性，其他自定义方法、属性需要`dynamic`修饰；Swift特有的类型无法获取动态性。
- [Swift 2 throws 全解析 - 从原理到实践](https://onevcat.com/2016/03/swift-throws/) 深入解析Swift 2的异常处理机制。喵大的原创文章，值得一读。
- [Why Swift guard Should Be Avoided](https://medium.com/swift-programming/why-swift-guard-should-be-avoided-484cfc2603c5#.hm25x0vl3) 对`guard`的不同观点。

## React-Native
- [React-Native痛点解析之开发环境搭建及扩展](http://mp.weixin.qq.com/s?__biz=MzA3ODg4MDk0Ng==&mid=403225766&idx=1&sn=acd8e3ab7f234b97827c3e210c2d8673&scene=4#wechat_redirect) `Hello World`是学习的开始，搭建环境是`Hello World`的开始。

## 网络交互
- [Http缓存策略详解]()

## 后花园
- iPhone SE发售。“迄今最高性能的4英寸iPhone”、“一小部的一大步”。
- [AppStore 审核经验总结](http://mobilev5.github.io/2016/03/28/appstore-review-attention/)