title: iOS通用下载管理器-HJMURLDownloader
date: 2016-3-13
tags: 网络
categories: iOS开发
author: 东子
---

HJMURLDownloader 是我们内部使用的 iOS 下载模块，基于 NSURLSession开发，已经在两个项目中使用，还算是比较稳定，在大家的努力下终于已经在 Github 上开源了，见[这里](https://github.com/HujiangTechnology/HJMURLDownloader)。

<!--more-->

## 为什么造轮子

在我们内部有不少项目需要下载功能，之前因为种种原因，并没有单独设计一套下载模块，而且原有项目中的方案还是基于 NSURLConnection，缺失了不少 NSURLSession 提供的新特性，失去了抽取并独立为模块的意义，为此我们在一次新的项目开发中重新开始设计。在前期 我们在 github 上也参考了不少下载模块开源项目，但综合看了一遍，并没有太完美的方案，主要参考和借鉴了 [HWIFileDownload](https://github.com/Heikowi/HWIFileDownload) 和 [TCBlobDownload](https://github.com/thibaultCha/TCBlobDownload) 这两个不错的项目，基于我们的需求重新搞了一套，

总结了下，提供了如下特性：

- 基于 NSURLSession，支持多任务、后台下载、断点续传
- 采用 CoreData 存储下载数据，并提供了一套下载管理界面，显示下载中和已下载的任务
- 使用 Protocol 扩展下载信息，提供了 Delegate 和 Block 两种回调方式，便于替换到现有项目
- 处理了较多的异常情况，例如用户手动把后台的 app 杀掉

说几点开发下载模块时遇到的问题和注意点:

1. BackgroudSession 创建的 identifier 需要一一对应，和默认提供的单例方法有冲突，需要提供单例和创建实例两种方式，这在一开始设计的时候并没有觉得是多大的问题，直到下载模块被越来越多的其他模块依赖就需要提供多个后台下载。

2. 断点续传有时候会在 app 第二次启动恢复下载时失效，需要重新开始下载。解决方法是需要检查 `NSURLSessionDownloadTask` 生成的 resumeData 的合法性，resumeData 存的内容为 plist 文件，一般为 4kb 大小，里面的 tmp 文件路径在 iOS 9 和 iOS 9 之前的存储是有差异的，需要区分对待，并且第二次启动后需要矫正 tmp 文件路径，有时也可能出现 tmp 文件不存在的情况也需要考虑到。

3. 后台下载在 app 退到后台或者遇到 crash 时也会正常继续下载，除非用户主动把 app 在后台杀死，此时更好的设计是提供一套后台蒙版，显示后台正在进行下载中...

4. 用户后台主动杀死app会按照取消逻辑处理，第二次启动后，逻辑需要修正，此时可以通过 ```- (void)URLSession:(NSURLSession *)session task:(NSURLSessionTask *)task didCompleteWithError:(NSError *)error``` 返回的 error 区分是否为上次后台杀死的任务，检查 error.userInfo 的 key NSURLErrorBackgroundTaskCancelledReasonKey 

5. 正确理解 NSURLSession ```- (void)getTasksWithCompletionHandler:(void (^)(NSArray<NSURLSessionDataTask *> *dataTasks, NSArray<NSURLSessionUploadTask *> *uploadTasks, NSArray<NSURLSessionDownloadTask *> *downloadTasks))completionHandler;``` 的用法，此回调主要是用来处理上次 app 未结束的任务，处理的任务有下载完成，下载取消，下载进行中几种可能，下载中的任务需要重新 和 UI 绑定，下载完成的任务需要正确处理好解压或者路径移动的问题，下载取消的任务状态要调整为暂停并且确保 UI 正确显示

6. 处理 ```- (void)application:(UIApplication *)anApplication handleEventsForBackgroundURLSession:(NSString *)aBackgroundURLSessionIdentifier completionHandler:(void (^)())aCompletionHandler``` 方法，此时的 aCompletionHandler的回调需要正确保存和执行，可以让退到后台的应用刷新后台截图，或者触发新的下载任务，适合一个接一个下载。

7. 当 WIFI 状态切换时，需要重新创建 NSURLSession，NSURLSession的销毁回调是异步的，期间调用下载可能导致闪退问题，暂时没有更好的处理方法，需要处理好逻辑。

8. NSURLSession 后下载无法控制下载的数目，系统会自动优化，所以需要控制队列，通过 NSOperationQueue 方式可能更好，目前的方式是数组维护，逻辑处理不完善时会存在触发多个下载的问题，在这个地方没有抽象好是一个遗憾。

9. 判断系统剩余空间暂时没有提供更好的方法，这个点其实目前还没有处理好，因为 URLSession 接口并没有类似 URLConnection 的方法 ```- (void)connection:(NSURLConnection*)connection didReceiveResponse:(NSURLResponse *)response```, 可以在下载开始后取到文件的大小从而判断是否符合当前空间大小，目前只是在下载之前提供回调接口判断，调用方需要在下载之前就可以取得下载文件的大小。

10. 采用 CoreData 的存储方案是一个很有意思的点，之所以用这个有点偏重的方案是因为因为 CoreData 的一个特性 ```NSFetchedResultsController```，可以自动检测数据变化从而方便的属性 UI，所以在做下载管理界面的时候就特别合适处理多个任务的UI刷新，不需要设计一套复杂的逻辑，所以的刷新东西触发都由 CoreData 主动发起。

下载模块的设计很多时候是为了更好的给业务线使用，如果够用了就可以了，所以很多地方的设计并不是很通用，很多情况也没有太好的处理妥当，后续有机会我们会结合实际项目聊聊下载模块里面的一些设计思想和始终为处理好的疑难杂症，并根据项目使用继续维护好下载模块~