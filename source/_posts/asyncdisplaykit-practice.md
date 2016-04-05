title: 深度实践 AsyncDisplayKit
date: 2016-3-5
tags: 页面布局
categories: iOS开发
author: 元相
---

追求极致的用户体验，从来都是我们锲而不舍的追求，对于iOS用户而言，这更是容不得一点马虎。随着时间的推移，现如今，谁还能忍受得了一个页面打开后，半天没有结果😭，出来之后滑动卡顿，点击个按钮半天木有反应啊，有木有？还好，还有`Facebook`，这位互联网IT界的大佬，为我们带来了福音，它就是我们今天要讲的` AsyncDisplayKit`。

<!--more-->

## 前言
`AsyncDisplayKit` 可以给我们带来很棒的用户体验，主要是通过优化以下3点：

 * 图像解码
 * 页面布局
 * 页面元素渲染

通过把以上几项操作放在后台线程，从而避免了阻塞用户主线程。所以，通过这个库，如果使用得当，即使是复杂的页面布局我们仍可以获得丝滑般的无限接近60FPS的页面滚动体验，这一点，通过常规的`UIKit`优化一般是达不到这个效果的😏。

在本文中，我们通过一个开始的初级项目（主要是使用了`UICollectionView`，有一些滚动体验不佳），通过使用`AsyncDisplayKit`来优化提升它的性能。跟着我走，你将会学会如何在你自己的项目中使用  ` AsyncDisplayKit ` 。

>
注意: 在开始之前, 你最好对 Swift, Core Animation 和 Core Graphics这些知识已经有所了解.

## 开始了
开始之前，如果有时间的话，你最好看一下[AsyncDisplayKit的介绍](https://code.facebook.com/posts/721586784561674/introducing-AsyncDisplayKit-for-smooth-and-responsive-apps-on-ios/)，这样你会对 `AsyncDisplayKit`的主要功能有一个初步的认识。项目源码*(EMAIL ME:1043370115@QQ.COM)*，先跑起来看看（需要Xcode 6.3 和 the iOS 8.3 SDK 以上环境）。为了能比较明显的看到使用这个库的差异，最好在老一点的设备上运行，如果是模拟器，看到的性能提升不明显，你懂的😏。运行起来，看起来是这样的:
![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/10/IMG_0002.png)
像你现在看到的，这个app的主页面是一个众多动物卡片组成的一个` UICollectionView `,每一个卡片包含了一张图片，一条文字描述，以及一个由主图片经过模糊处理的背景。<br>

 滚动一下这个页面，注意它的帧率，（我使用的是iPad 3，大概只有20FPS），失帧很严重啊，所以你的直观触觉就是界面很不流畅，卡顿实实在在的。好吧，在这篇文章结束的时候，我们的目标就是要把它搞到（或者无限接近）60FPS。<br>

 >
 注意: 这个项目里你看到的所有图片都是放在本地地资源文件里面的，没有一个是从网络下载来的。

## 测试响应能力
在使用`AsyncDisplayKit`优化你的项目之前，先通过`Instruments`来检测一下你的应用的响应能力，这一点很重要，可以确保你知道优化后`AsyncDisplayKit`给你带来了什么变化。<br>
最重要的是，影响性能的因素中，无非就是`CPU`，`GPU`这两块，所以优化前，你应该首先弄清楚你的性能瓶颈在哪里，究竟是受制于`CPU`还是`GPU`，是哪一个降低了你应用的FPS。搞清楚这个之后，你可以看到`AsyncDisplayKit`是如何利用它的特性来帮你优化的。<br>
如果你有时间的话，你可以使用`Instruments`来监测一下我们一开始提供的那个项目的性能瓶颈，你会发现它是受制于`CPU`的。

## 准备切换到AsyncDisplayKit
在已存在的项目上使用`AsyncDisplayKit`很简单，就是把`view hierarchies`或者/和 `layer trees`替换为`display node hierarchies`。  `Display nodes `是`AsyncDisplayKit`中很重要的一个概念，它是基于views之上并且线程安全的，这意味着我们平常习惯于在主线程中做的那些views有关的部分工作现在可以脱离主线程了，是不是很惊奇？没错，这就是最大的魅力所在，所以你就可以把有限的资源去处理更重要的事情了，比如`touch event`或者`scroll view`的滚动。所以接下来第一步，就是去掉`view hierarchy`。<br>

### Removing the View Hierarchy
打开 `RainforestCardCell.swift`，在 `awakeFromNib()` 删除所有的 `addSubview(...)` 调用, 像这样:

```swift
override func awakeFromNib() {
  super.awakeFromNib()
  contentView.layer.borderColor =
    UIColor(hue: 0, saturation: 0, brightness: 0.85, alpha: 0.2).CGColor
  contentView.layer.borderWidth = 1
}
```

替换`layoutSubviews()` 为下面的：

```swift
override func layoutSubviews() {
  		super.layoutSubviews()
}
```

替换`configureCellDisplayWithCardInfo(cardInfo:)` 为下面的：

```swift
func configureCellDisplayWithCardInfo(cardInfo: RainforestCardInfo) {
  //MARK: Image Size Section
  let image = UIImage(named: cardInfo.imageName)!
  featureImageSizeOptional = image.size
}
```

删除`RainforestCardCell`中所有的`view`属性，剩下来的像这样：<br>

```swift
class RainforestCardCell: UICollectionViewCell {
  var featureImageSizeOptional: CGSize?
  ...
}
```

然后保存并运行，结果像这样：
![](http://www.raywenderlich.com/wp-content/uploads/2014/10/IMG_0001.jpg)
现在都是一些空的cells,所以你滚动起来相当顺滑，我们的目标就是当这些cell填上内容之后，仍然保持这样的触感。在你每做一步之后，你可以使用`Instruments’s Core Animation template`来观察app的帧率有什么变化。<br>

### Adding a Placeholder
在`RainforestCardCell.swift`中添加一个属性`placeholderLayer`：

```swift
class RainforestCardCell: UICollectionViewCell {
  	var featureImageSizeOptional: CGSize?
  	var placeholderLayer: CALayer!
  		...
 }
```

使用placeholder是因为我们的cell的内容展示的时候是异步的，为了不让用户看到空的cell。这就像我们从网络下载图片的时候的做法一样，当图片下载完成之前先设置一个placeholder。

在`awakeFromNib()`中，配置`placeholderLayer`，然后该方法如下：

```swift
override func awakeFromNib() {
   	super.awakeFromNib()
  	placeholderLayer = CALayer()
  	placeholderLayer.contents = 	UIImage(named:"cardPlaceholder")!.CGImage
  	placeholderLayer.contentsGravity = kCAGravityCenter
  	placeholderLayer.contentsScale = UIScreen.mainScreen().scale
   	placeholderLayer.backgroundColor = UIColor(hue: 0, saturation: 0, 		brightness: 0.85, alpha: 1).CGColor
  	contentView.layer.addSublayer(placeholderLayer)
}
```

在 `layoutSubviews()`, 加载`placeholderLayer`,修改后的方法如下：

```swift
override func layoutSubviews() {
  	super.layoutSubviews()
  	placeholderLayer?.frame = bounds
}
```

编译运行，看起来是这样：
![](http://www.raywenderlich.com/wp-content/uploads/2014/10/IMG_0003.png)

普通的`CALayers`单独使用，没有与view关联的时候，当你改变frame的时候它们会有一个隐式的动画，所以你应该会看到当那个layer加载出来的时候有一个缩放的动画，为了修改这个问题，我们重写`layoutSubviews`方法如下：

```swift
override func layoutSubviews() {
  super.layoutSubviews()
  CATransaction.begin()
  CATransaction.setValue(kCFBooleanTrue,
  forKey:kCATransactionDisableActions)
  placeholderLayer?.frame = bounds
  CATransaction.commit()
}
```

重新编译运行，你会发现刚才的问题解决了。

## Your First Node
我们要重构这个app的第一步就是添加一个node，在这一部分，我们将要处理以下几个任务：<br>

* 创建，布局，添加 node 到 cell 中
* 重用 cell 以及其中的 node 和 layer
* 对 image node 做 blur 处理 

接下来，在 `Layers-Bridging-Header.h`中导入`AsyncDisplayKit`:

```swift
#import <AsyncDisplayKit/AsyncDisplayKit.h>
```

### 项目结构梳理
* View Controller : `RainforestViewController` 实际上不做什么事情，只是简单的获得数据源并实现`UICollectionView`的代理。
* Data Source : view controller 生成并重用cell，通过调用`configureCellDisplayWithCardInfo(cardInfo:)`配置cell。
* Cell : 在 `configureCellDisplayWithCardInfo(cardInfo:)`方法中，cell生成node并添加到cell上面，然后布局nodes。

这意味着每一次重用cell的时候，都会重复这些动作 。
如果你是用views代替nodes，那这样绝对不是最好的做法。但是现在用的是nodes，因为nodes的生成，布局以及填充，这些步骤都是可以放在异步线程做的，所以目前还不是问题，当然我们后续还会优化。唯一的问题是这样做的话，你不能很方便的取消这些异步操作或者是在重用cell的时候删除nodes。

>
>Note : 在实际的开发中，你可以选择使用ASRangeController来缓存nodes，这样你就不需要在每次重用cell的时候去重新生成nodes。


### Adding the Background Image Node
打开 `RainforestCardCell.swift` 然后替换 `configureCellDisplayWithCardInfo(cardInfo:)` 为如下:

```swift
func configureCellDisplayWithCardInfo(cardInfo: RainforestCardInfo) {
  	//MARK: Image Size Section
  	let image = UIImage(named: cardInfo.imageName)!
  	featureImageSizeOptional = image.size
  	//MARK: Node Creation Section
  	let backgroundImageNode = ASImageNode()
  	backgroundImageNode.image = image
  	backgroundImageNode.contentMode = .ScaleAspectFill
 }
```

`ASImageNode`是`AsyncDisplayKit`中用来做展示用的众多nodes中的一种，等价于`UIKit`中的`UIImageView`，只是`ASImageNode`默认情况下的图片解码操作是异步的。

在`configureCellDisplayWithCardInfo(cardInfo:)`的末尾添加下面一行代码：

```swift
backgroundImageNode.layerBacked = true
```

Nodes有两种模式，一般情况下当需要处理某些事件的时候比如`touch event`，我们采用`view-backed`模式，反之如果只是纯粹的展示则采用`layer-backed`模式，`layer-backed`模式相对而言是轻量级的，会有更好一点的性能。由于我们这个项目中不需要处理事件，所以`backgroundImageNode`采用`layer-backed`模式。

在`configureCellDisplayWithCardInfo(cardInfo:)`的末尾继续添加下面一行代码：

```swift
//MARK: Node Layout Section
backgroundImageNode.frame = FrameCalculator.frameForContainer(featureImageSize: image.size)
```
`FrameCalculator`是个辅助类，封装了cell的布局处理，返回每一个node的frame。如果你要适配多个设备尺寸，这里你要谨慎处理，你可以看到这里没有使用约束，因为`AsyncDisplayKit`目前版本并不支持约束，希望后续支持吧。

在`configureCellDisplayWithCardInfo(cardInfo:)`的末尾继续添加下面一行代码：

```swift
//MARK: Node Layer and Wrap Up Section
self.contentView.layer.addSublayer(backgroundImageNode.layer)
```

上面已经提到了，`AsyncDisplayKit`会为`backgroundImageNode`创建一个layer，但是你仍然需要把这个node添加到`layer tree`中，它才可以在屏幕上显示。另外由于node的绘制是异步的，所以在绘制完成之前它是不会显示的，尽管你已经把它添加到`layer tree`中了，这一点需要注意。当node的图片绘制完成之后，node的layer的content就会被更新，这个时候cell的 `content view`的layer会有两个sublayer.

你应该会注意到cell每次被取出重用的时候，`configureCellDisplayWithCardInfo(cardInfo:)`都会被调用，所以每次该方法调用的时候，我们都会添加一个新的layer到cell的contentview的layer上面，不过别担心，我们马上就会解决这个问题。

在`RainforestCardCell.swift`中新添加一个变量`backgroundImageNode`像这样：

```swift
class RainforestCardCell: UICollectionViewCell {
  var featureImageSizeOptional: CGSize?
  var placeholderLayer: CALayer!
  var backgroundImageNode: ASImageNode? ///< ADD THIS LINE
  ...
}
```

添加这个属性我们可以持有这个node，是因为在`ARC`环境下，某些时候它会被释放，这样就不会显示在屏幕上了。node是持有它的layer的引用的，但是它的layer并不持有node，所以我们需要持有这个node。

在`configureCellDisplayWithCardInfo(cardInfo:)`的末尾继续添加下面一行代码：

```swift
self.backgroundImageNode = backgroundImageNode
```

好了，目前为止，`configureCellDisplayWithCardInfo(cardInfo:)`是这样子的：

```swift
func configureCellDisplayWithCardInfo(cardInfo: RainforestCardInfo) {
  //MARK: Image Size Section
  let image = UIImage(named: cardInfo.imageName)!
  featureImageSizeOptional = image.size
  //MARK: Node Creation Section
  let backgroundImageNode = ASImageNode()
  backgroundImageNode.image = image
  backgroundImageNode.contentMode = .ScaleAspectFill
  backgroundImageNode.layerBacked = true
  //MARK: Node Layout Section
  backgroundImageNode.frame = FrameCalculator.frameForContainer(featureImageSize: image.size)
  //MARK: Node Layer and Wrap Up Section
  self.contentView.layer.addSublayer(backgroundImageNode.layer)
  self.backgroundImageNode = backgroundImageNode
}
```
编译运行，你可以观察到`backgroundImageNode`的图片的异步呈现，感觉一下效果吧。
![](http://www.raywenderlich.com/wp-content/uploads/2014/10/IMG_0006.png)

如果你运行在一个老一点的设备上，你会发现那些cell上的图片像爆米花一样一个个跳出来了，这并不是我们的理想结果，这个问题我们会放在最后解决。

正如上面我已经提到的，每次cell重用的时候，都会有一个新的layer被加上去，你可以快速滚动页面，然后打个断点在cell里面，会发现很有多layer在上面，接下来我们就来处理这个问题。


### Handling Cell Reuse
首先继续在`RainforestCardCell.swift`, 添加个`contentLayer`，像这样：

```swift
class RainforestCardCell: UICollectionViewCell {  
		var featureImageSizeOptional: CGSize?
  		var placeholderLayer: CALayer!
  		var backgroundImageNode: ASImageNode?
  		var contentLayer: CALayer? ///< ADD THIS LINE
  		...
}
```

在`configureCellDisplayWithCardInfo(cardInfo:)`的末尾继续添加下面一行代码：

```swift
self.contentLayer = backgroundImageNode.layer
```

然后替换 `prepareForReuse()`方法如下：

```swift
override func prepareForReuse() {
  	super.prepareForReuse()
  	backgroundImageNode?.preventOrCancelDisplay = true
}
```

由于`AsyncDisplayKit`可以异步绘制，nodes可以让你阻止任何正在进行的绘制，当你需要取消或者停止绘制的时候只需要设置它的`preventOrCancelDisplay`为`true`即可，这样你就可以在cell重用之前停止之前的所有绘制，是不是很赞？

接下来，在`prepareForReuse()`中再添加几行代码，如下：

```swift
contentLayer?.removeFromSuperlayer()
contentLayer = nil
backgroundImageNode = nil
```

这几行代码简单易懂，就是在cell重用之前，把相应的layer和node都删除，并确保被释放掉，这样就解决了上面提到的layer堆积的问题。
编译运行，这一次就不会有layer堆积了，并且在cell滚出屏幕可视范围后，取消不必要的绘制。


### Blurring the Image
为了添加blur效果，需要我们在imagenode的展示过程中加入一些步骤。在`RainforestCardCell.swift`文件的`configureCellDisplayWithCardInfo(cardInfo:)`方法中做点修改，在`backgroundImageNode.layerBacked = true`后面添加如下代码：

```swift
backgroundImageNode.imageModificationBlock = { input in
  if input == nil {
    return input
  }
  if let blurredImage = input.applyBlurWithRadius(
    30,
    tintColor: UIColor(white: 0.5, alpha: 0.3),
    saturationDeltaFactor: 1.8,
    maskImage: nil, 
    didCancel:{ return false }) {
      return blurredImage
  } else {
    return image
  }
}
```

`ASImageNode`的`imageModificationBlock `给了我们一个机会在展示原始图片之前可以做一些其他的操作，比如滤镜操作，模糊处理等。

上面的代码，使用`imageModificationBlock `，给cell的背景图加上了模糊效果。最关键的一点是imagenode把绘制动作和这个闭包操作放在了子线程，这样就使主线程运行顺畅，这个闭包把原始图片作为参数然后返回处理后的图片。

这个模糊处理是使用了系统的方法，`UIImage` 的 `blurring category`，它主要是使用`Accelerate framework` 基于`CPU`来做的模糊操作。由于这个模糊处理会消耗内存同时也比较耗时，所以就支持了取消机制，`didCancel`闭包会被多次调用来监测是否应该取消模糊操作。目前为止，上面的代码只是简单返回了`false`，稍后我们就会来实际修改`didCancel`。

```swift
Note : 还记得一开始的时候你滑动页面的时候是什么感觉么？那个模糊处理严重的阻塞了主线程，通过AsyncDisplayKit把这个操作放入子线程，现在大大提升了collection view滚动时候的体验。这简直就是一个天上，一个地下啊，有木有？
```

运行看看现在的效果吧：
![](http://www.raywenderlich.com/wp-content/uploads/2014/10/IMG_0009.png)

现在你可以看到`collection view`滚动起来是有多么的顺滑。

当`collection view`从重用队列取出一个cell重用的时候就开始了一个模糊处理操作，所以当你快速滚动的时候，`collection view`就会重用每一个cell很多次，理所当然的就启动了很多模糊处理的操作。这个当然不合理了😏😏，所以我们的目标应该是当一个cell开始重用的时候，去取消之前的模糊处理操作。

在前面我们已经可以在`prepareForReuse()`中取消node的绘制，所以一旦我们有机会在合适的时候取消模糊操作的话，那就毫不犹豫的去取消吧。


### Canceling the Blur
为了取消那些正在进行的blur操作，我们需要重新实现blur方法中的`didCancel`闭包。添加如下代码到`imageModificationBlock`中：

```swift
backgroundImageNode.imageModificationBlock = { [weak backgroundImageNode] input in
  if input == nil {
    return input
  }
  // ADD FROM HERE...
  let didCancelBlur: () -> Bool = {
    var isCancelled = true
    // 1
    if let strongBackgroundImageNode = backgroundImageNode {
      // 2
      let isCancelledClosure = {
        isCancelled = strongBackgroundImageNode.preventOrCancelDisplay
      }
      // 3
      if NSThread.isMainThread() {
        isCancelledClosure()
      } else {
        dispatch_sync(dispatch_get_main_queue(), isCancelledClosure)
      }
    }
    return isCancelled
  }
  // ...TO HERE
  ...
}
```

很明显，我们这里需要使用一个`weak reference`来避免`closure`和`backgroundImageNode`的循环引用。我们就使用`backgroundImageNode`来决定是否需要取消模糊处理。

上面的代码完成了如下几个功能：

* 首先获得`backgroundImageNode`的一个强引用，为我们接下来的操作做准备。如果当这个闭包运行的时候`backgroundImageNode`不存在了，那么`isCancelled`就会为`true`，blur操作就会被取消，那我们就别提做什么blur操作了。
* 你会有疑问，在这个地方为什么取消blur的这个闭包中的代码会要求放在主线程来操作，这是因为一旦node创建了它的`layer`或者`view`之后，你就只能在主线程来访问node的属性了（这一点很重要）。由于我们需要使用node的`preventOrCancelDisplay`这个属性，而此时`backgroundImageNode`的layer已经创建过了，所以我们必须把这个监测放在主线程中。
* 由于我们需要确保`isCancelledClosure`会在主线程来被调用，所以如果是在主线程就直接访问`preventOrCancelDisplay`，否则的话就使用`dispatch_sync`来在主线程访问。你又有疑问么😏，这里又为什么使用`dispatch_sync`来同步执行，是因为我们必须在`didCancelBlur`闭包返回之前给一个明确的结果，即返回一个确切的`isCancelled`值。

最后，在调用`applyBlurWithRadius(...)`方法的地方，把刚才我们定义好的闭包作为值传给`didCancel`这个参数，所以代码看起来像下面这样：

```swift
if let blurredImage = input.applyBlurWithRadius(
  30,
  tintColor: UIColor(white: 0.5, alpha: 0.3),
  saturationDeltaFactor: 1.8,
  maskImage: nil,
  didCancel: didCancelBlur) {
  ...
}
```
编译运行，你会发现有很大不同，现在当cell滚出屏幕后，它相应的blur操作也会被取消，这样我们会节省很多内存开销，同时减少了不必要的CPU时间片占用。你将会看到巨大的性能提升，尤其是在配置低一点的设备上。

当然了，也不可能把所有的操作都搬到子线程，我们的卡片还需要其他的数据展示，在接下来的文章中，我们还将学习以下几点：

* 创建一个node容器来绘制其他内容并添加到一个单一的`CALayer`上面；
* 自定义`ASDisplayNode`；
* 在子线程来创建node层级并布局sub nodes

所以接下来，敬请期待吧😊😊😊