title: iOS微信浏览器直接打开App的解决方案
date: 2016-3-23
toc: false
tags: 疑难杂症
categories: iOS开发
author: Yomi
---

微信浏览器默认屏蔽了App的Scheme跳转，我们无法直接从微信中跳转至App中。但神奇的是市面上有两类App可以正常跳转。

<!--more-->

1. 大众点评，小红书，BiliBili，京东等
2. 豆瓣等

第一类，这些是腾讯入股的，应该有白名单一说，让腾讯给你100万，或许你就能获得这个能力。

第二类，查看豆瓣页面在微信中跳转的地址：

```
https://www.douban.com/doubanapp/dispatch?uri=/movie/5327268&from=mdouban&page=movie&model=B&copy=1&channel=card_movie&download=1
```

而不是我们看到的自定义scheme，原因只有一个，它就是 `Universal Links`

配置 Universal Links 可参看[苹果文档](https://developer.apple.com/library/ios/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW1)。

第二类解决方案有几个注意点：

1. Universal Links 可以由系统来做选择，在短信或其他应用中，常按选择打开方式，若选择Safari打开，则后续的跳转会默认跳Safari
2. Universal Links只在iOS 9及以上系统支持



参考：

* App Search API Validation Tool， https://search.developer.apple.com/appsearch-validation-tool/
* 配置Universal Links，可参看[苹果文档](https://developer.apple.com/library/ios/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW1)
* Deferred Deep Linking in iOS，http://tech.glowing.com/cn/deferred-deep-linking-and-branch-sdk-in-ios/

