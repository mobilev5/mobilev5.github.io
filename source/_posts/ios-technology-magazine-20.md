title: iOS技术周刊第20期
date: 2016-7-17
tags: iOS技术周刊
categories: iOS开发
author: 东子
---

本周挖掘了一些不错的开源项目，分享给大家~

<!--more-->

## Mobile C++ 开源项目
介绍两个采用 Djinni 的 Mobile C++ 开源项目
采用的很多技术方案让人眼前一亮    

* [Twenty48
](https://github.com/boloutaredoubeni/Twenty48) An example project using React Native for the UI and C++ for the business logic via Djinni
 
* [Cookpit](https://github.com/kittinunf/Cookpit) 底层 C++ 用了 libcurl 和 lmdb，上层 demo iOS 用了 RxSwift，Android 是 kotlin，第三方依赖用的是 git subtree，同时用了 clang-format, C++ 库还搞了个 flowcpp，是源自 Redux。

## WWDC
* WWDC 2016 的视频看了几个了？还是来看看这个短平快的总结吧~  
[New stuff from WWDC 2016](https://gist.github.com/mackuba/e8fb4219c7ef611f47cdb66b93986d85)

## 设计模式
ReactiveCocoa 和 RxSwift 已经不够酷了，现在的思潮是基于 Flux 的 [ReSwift](https://github.com/ReSwift/) ! 一起看下同一位作者的两篇文章学习下吧~  
* [Real World Flux Architecture on iOS](http://blog.benjamin-encz.de/post/real-world-flux-ios/)  
* [Unidirectional Data Flow in Swift: An Alternative to Massive View Controllers](https://realm.io/news/benji-encz-unidirectional-data-flow-swift/)

## 后花园
* [Pokemon-Go-Controller](https://github.com/kahopoon/Pokemon-Go-Controller) Pokemon 最近火热火热啊~ 来看看这个有趣的开源项目，通过 Xcode 模拟 GPS 位置，实现不出家抓遍天下的梦想。当然成本也是很高的，两个 iOS 设备和一个 Mac，亲眼见识了旁边同学折腾的我表示玩起来很蛋疼... 但是在一天之后就出现了集成方向键的懒人版 Pokemon, George 同学已经玩的废寝忘食鸟....
* [iOSAppHook](https://github.com/Urinx/iOSAppHook) 专注于非越狱环境下 iOS 应用逆向研究，从 dylib 注入，应用重签名到 App Hook。看了这些逆向知识，做一个懒人版 Pokemon 应该就不成问题啦~
* [大厂都是这么测试滴...](https://code.facebook.com/posts/300815046928882/) 嗯，还不至于犯密集恐惧症吧....

