title: 现有iOS端图片缓存解决方案
date: 2016-3-22
tags: 网络
categories: iOS开发
author: 阿P
---

移动App中使用网络图片的情况很常见，如果缓存问题处理不好会很大程度上影响App的性能。本文简单介绍一下图片缓存流程及`GitHub`上现有解决方案。

<!--more-->

## 一般图片缓存及使用流程

最简单的缓存方式：

```swift
let imgData = NSData(contentsOfURL: NSURL(string: "http://7narze.com1.z0.glb.clouddn.com/1.jpg")!)
imgData?.writeToFile("cache patch", atomically: true)
let img = UIImage(data: imgData!)
imgView.image = img
```

很明显，下载图片和缓存图片时有主线程阻塞。那么优化成下面的样子：

```swift
dispatch_async(dispatch_get_global_queue(QOS_CLASS_DEFAULT, 0)) { () -> Void in
    let imgData = NSData(contentsOfURL: NSURL(string: "http://7narze.com1.z0.glb.clouddn.com/1.jpg")!)
    imgData?.writeToFile("cache patch", atomically: true)
    dispatch_async(dispatch_get_main_queue(), { () -> Void in
        let img = UIImage(data: imgData!)
        imgView.image = img
    })
}
```

这样是在子线程下载并缓存，性能上会好很多，但很明显这样不够用。

### 一般缓存流程

![一般流程](flow.png)

从上图，可以看出还有很多可优化点

```
异步下载
子线程解压缩
使用缓存 (内存/磁盘)
缓存解压缩后的图片
减少内存级别的拷贝
良好的接口
图片预下载
```

## 现有解决方案

好，下面我们简单介绍下目前`GitHub`上比较流行（star数据大于2000）的图片缓存库。

### SDWebImage

基础流程  
![SDWebImage流程](SDWebImage.png)

- 优点
  - 健全的Category
  - `SDWebImagePrefetcher`预加载
  - 独立的下载模块/缓存模块
  - 自定义`age`
- 缺点
  - 因缓存的图片数据已解码，故占用空间变大(这个也算不上缺点)
  - 没有处理减少内存级别的拷贝和`字节对齐`(`FastImageCache`对此有处理)
  - 图片属于静态资源，没有判断`Cache-Control`、`Last-Modified`或`ETag`(通用库做到这点有些麻烦)

### Kingfisher

`SDWebImage`的`Swift`版

流程基本和`SDWebImage`一致

- 优点
  - `Swift`编写
  - 使用很多最新API
  - 其他同`SDWebImage`
- 缺点
  - iOS 8 later
  - 其他同`SDWebImage`

### AFNetworking

基础流程图  
![AFNetworking流程](AFNetworking.png)

- 优点
  - 健全的Category
  - 缓存机制可定制(实现`AFImageCache`)
  - 依附`AFNetworking`
- 缺点
  - 功能单一
  - 缓存仅在内存中(默认实现)

### FastImageCache

无网络缓存功能，但却是[图片加载速度极限优化](http://blog.cnbang.net/tech/2578/)


## 总结

实际应用中推荐`SDWebImage`/`Kingfisher`结合`FastImageCache`使用。若要考虑HTTP的缓存机制，可以自己继承/实现封装。