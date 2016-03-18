title: WKWebView(入门篇)
date: 2016-3-17
toc: false
tags: Hybrid
categories: iOS开发
author: 宏伟
---

`WKWebView` 是 iOS8 中引入的新组件，苹果将 UIWebViewDelegate 与 UIWebView 重构成了 14 个类和 3 个协议并引入了不少新的功能和接口。由于一直以来苹果对于WebView内核封锁的程度是令人发指的，WKWebView 的引入无疑是另广大开发者兴奋的事。

<!--more-->

## 与传统UIWebView的优劣对比

* 优点
    * 大幅降低网页加载时所占用的内存
    * 大幅增加网页加载速度
    * 支持高达 60 fps 的滚动刷新率，内置了手势探测
    * 提供更多Web新功能和接口
    * 支持了更多的HTML5特性
    * 更优雅的与JS的交互方式

* 缺点
    * WkWebView不再支持页面缓存
    * WkWebView不可以实现NSURLProtocol拦截

看到它相比于UIWebView有着那么多的优点，是不是已经心动了呢？那么接下来与笔者一起走入WkWebView的奇妙世界吧~


## 新增的属性

### 1.estimatedProgress

以往WebView的加载进度条的具体进度值都是假的数据，而`estimatedProgress`则可以用来显示真实的进度值变化。`estimatedProgress`是通过KVO的监控来进行使用的。

```objc
[webView addObserver:self forKeyPath:@"estimatedProgress" options:NSKeyValueObservingOptionNew context:NULL];
```

```objc
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context {
    if ([keyPath isEqualToString:@"estimatedProgress"]) {
        if (object == webView) {
            double progress = webView.estimatedProgress;
            
            /** 用当前获取的进度值去处理进度条控件 */
            ... ....
        } else {
            [super observeValueForKeyPath:keyPath ofObject:object change:change context:context];
        }
    }
}
```


### 2.title

与`estimatedProgress`的使用方式类似，可以获取到html页面`title`标签下设置的值。

```objc
[webView addObserver:self forKeyPath:@"title" options:NSKeyValueObservingOptionNew context:NULL];
```

```objc
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context {
   	if ([keyPath isEqualToString:@"title"]) {
        if (object == webView) {
            NSString *title = webView.title;
            // ... ...
        } else {
            [super observeValueForKeyPath:keyPath ofObject:object change:change context:context];
        }
    }
}
```

### 3.WKWebViewConfiguration

`WKWebViewConfiguration`可以设置偏好设置，与网页交互的配置，注入对象，注入js等。

```objc
WKWebViewConfiguration *config =[[WKWebViewConfiguration alloc]init];
    
/** WebView的偏好设置 */
    
config.preferences.minimumFontSize = 10;
    
config.preferences.javaScriptEnabled = YES;
    
/** 默认是不能通过JS自动打开窗口的，必须通过用户交互才能打开 */
    
config.preferences.javaScriptCanOpenWindowsAutomatically = NO;
    
/**  添加JS到到HTML中  */
    
NSString *js = @"window.alert('欢迎体验WkWebView!!');";
    
WKUserScript *script = [[WKUserScript alloc]initWithSource:js injectionTime:WKUserScriptInjectionTimeAtDocumentEnd forMainFrameOnly:YES];
    
WKWebViewConfiguration *config =[[WKWebViewConfiguration alloc]init];
    
[config.userContentController addUserScript:script];
    
/** 用WkWebView的 - (instancetype)initWithFrame:(CGRect)frame configuration:(WKWebViewConfiguration *)configuration 方法初始化webView */
    
webView = [[WKWebView alloc]initWithFrame:self.view.bounds configuration:config];
```

## 新的代理方法

WkWebView中提供了新的三种代理方法，分别是`WKNavigationDelegate`、`WKUIDelegate`、`WKScriptMessageHandler`，下文将对其作用的场景进行一一介绍。

### 1.WKNavigationDelegate

该代理方法主要是用来追踪webview的加载过程，和传统的UIWebView比较相似，分别对页面开始加载、加载完成、加载失败等几个过程进行事件捕捉和追踪，另外在页面的跳转时也可对其进行拦截处理、如过滤某些特定网页等业务操作。

```objc
/** 页面开始加载时调用 */
- (void)webView:(WKWebView *)webView didStartProvisionalNavigation:(WKNavigation *)navigation;

/** 当内容开始返回时调用 */
- (void)webView:(WKWebView *)webView didCommitNavigation:(WKNavigation *)navigation;

/** 页面加载完成之后调用 */
- (void)webView:(WKWebView *)webView didFinishNavigation:(WKNavigation *)navigation;

/** 页面加载失败时调用 */
- (void)webView:(WKWebView *)webView didFailProvisionalNavigation:(WKNavigation *)navigation;

/** 接收到服务器跳转请求之后调用 */
- (void)webView:(WKWebView *)webView didReceiveServerRedirectForProvisionalNavigation:(WKNavigation *)navigation;
```

以上4个代理委托是不是很眼熟？没错，其实就是对应UIWebView中的那几个代理方法。

```objc
/** 在收到响应后，决定是否跳转 */
- (void)webView:(WKWebView *)webView decidePolicyForNavigationResponse:(WKNavigationResponse *)navigationResponse decisionHandler:(void (^)(WKNavigationResponsePolicy))decisionHandler {
    
    // 如果响应的地址是百度，则允许跳转
    if ([navigationResponse.response.URL.host.lowercaseString isEqual:@"www.baidu.com"]) {
        decisionHandler(WKNavigationResponsePolicyAllow);
        return;
    }
    
    //否则不允许跳转
    decisionHandler(WKNavigationResponsePolicyCancel);
}
```

```objc
/** 在发送请求之前，决定是否跳转 */
- (void)webView:(WKWebView *)webView decidePolicyForNavigationAction:(WKNavigationAction *)navigationAction decisionHandler:(void (^)(WKNavigationActionPolicy))decisionHandler {
    
    // 如果响应的地址是百度，则允许跳转
    if ([navigationResponse.response.URL.host.lowercaseString isEqual:@"www.baidu.com"]) {
        decisionHandler(WKNavigationResponsePolicyAllow);
        
        return;
    }
    
    //否则不允许跳转
    decisionHandler(WKNavigationResponsePolicyCancel);
}
```

### 2.WKUIDelegate

这个代理中包含3个针对于web界面的三种提示框（警告框、确认框、输入框）的弹出事件捕捉

```objc
/**  确认框 */
- (void)webView:(WKWebView *)webView runJavaScriptAlertPanelWithMessage:(NSString *)message initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(void))completionHandler {
    [[[UIAlertView alloc] initWithTitle:@"标题" message:message delegate:nil cancelButtonTitle:@"确认" otherButtonTitles: nil] show];
    completionHandler();
}

/**  警告框 */
- (void)webView:(WKWebView *)webView runJavaScriptTextInputPanelWithPrompt:(NSString *)prompt defaultText:(nullable NSString *)defaultText initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(NSString * __nullable result))completionHandler;

/**  输入框 */
- (void)webView:(WKWebView *)webView runJavaScriptTextInputPanelWithPrompt:(NSString *)prompt defaultText:(nullable NSString *)defaultText initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(NSString * __nullable result))completionHandler;

/** webview关闭事件 */
- (void)webViewDidClose:(WKWebView *)webView;
```


### 3.WKScriptMessageHandler

这个协议只有一个方法，它是APP与Web进行交互的关键，在从web中接收到一个脚本时调用，在下文中会详细进行说明。
```objc
- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message;
```

## 更优雅的与Web交互

在讲WKWebView之前，让我们先来看一下UIWebView是如何与Web进行交互的。

### UIWebView -- APP 调 Web

```objc
NSString * result = [webView stringByEvaluatingJavaScriptFromString:@"func()"];
```

iOS9默认是不允许加载http请求的，对于webview加载http网页也是不允许的。可以通过修改info.plist取消http限制。

### UIWebView -- Web 调 APP

第一步， 在APP本地先写一个方法如下：

```objc
- (void)callNativeFun:(NSString *)args {
	
}
```

第二步，在UIWebView的delegate方法中注入一段JS源码

```objc
- (void)webViewDidFinishLoad:(UIWebView *)webView {
    NSString *js = @"(function() {\
        window.JSBridge = {};\
        window.JSBridge.callFunction = function(functionName, args) {\
            var url = \"hybrid://invoke?\";\
            var callInfo = {};\
            callInfo.functionname = functionName;\
            if (args) {\
                callInfo.args = args;\
            }\
            url += JSON.stringify(callInfo);\
            var rootElm = document.documentElement;\
            var iFrame = document.createElement(\"IFRAME\");\
            iFrame.setAttribute(\"src\",url);\
            rootElm.appendChild(iFrame);\
            iFrame.parentNode.removeChild(iFrame);\
        };\
        return true;\
    })();";

    [webView stringByEvaluatingJavaScriptFromString:js];
}

```

第三步，在HTML某个事件中调用如下方法

```objc
window.JSBridge.callFunction("callNative", "so many args");
```
当html调用这段注入的JS方法后，会将`hybrid://invoke?` 以及后面拼接的内容以IFrame的方式进行加载，在加载的同时会出发APP的delegate方法

最后一步，在APP的delegate中实现对本地方法的调用，从而完成Web到App的调用

```objc
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {
    NSString *urlStr = [NSString stringWithString:url];
    
    if ([[urlStr lowercaseString] hasPrefix:@"hybrid://invoke?"]) {
         
         urlStr = [urlStr substringFromIndex:protocolPrefix.length];
         urlStr = [urlStr stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
         
         NSError *jsonError;
         NSDictionary *callInfo = [NSJSONSerialization JSONObjectWithData:[urlStr dataUsingEncoding:NSUTF8StringEncoding] options:kNilOptions error:&jsonError];
         NSString *functionName = [callInfo objectForKey:@"functionname"];
         NSString * args = [callInfo objectForKey:@"args"];
         
         if ([functionName isEqualToString:@"callNative"]) {
             /**  在此处完成对预先定义好的native方法进行调用 */
             [self callNativeFun:args];
         }
         return NO;
    }
    return YES;
}
```

如上代码所述，UIWebView是通过Html的iframe来制造页面刷新再解析自定义协议，这种做法其实是比较lower的，也是对于苹果对UIWebView内核封闭的无奈。而WKWebView则是可以直接使用已经事先注入的js代码，runtime的将js接口给 Native 层传值。

### WKWebView -- APP 调 Web

```objc
NSString *js = @"window.alert('欢迎体验WkWebView!!');";
[webView evaluateJavaScript:js completionHandler:nil];
```

### WKWebView -- Web 调 APP

第一步，在webview初始化之前先注册一个名为hybridDemo的handler对象。

```objc
WKWebViewConfiguration *config =[[WKWebViewConfiguration alloc]init];
[config.userContentController addScriptMessageHandler:self name:@"hybridDemo"];
webView = [[WKWebView alloc]initWithFrame:self.view.bounds configuration:config];
```

第二步，在HTML端调用JS来访问之前注册的handler对象，并通过调用postMessage方法把数据传到app。

```javascript
var message = {"method":"hello", "args":"let go"};

window.webkit.messageHandlers.webViewApp.postMessage(message);
```

第三步，在webview容器中实现上文提过的WKScriptMessageHandler委托，从而响应来自于JS端下发的message。

```objc
- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message {
    NSDictionary *dic = message.body;
    NSString *method = [dic objectForKey:@"method"];
    NSString *param = [dic objectForKey:@"args"];
    
    /**  doSomeThing..... */
}
```