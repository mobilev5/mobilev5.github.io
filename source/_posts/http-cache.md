
title: HTTP缓存策略
date: 2016-4-1
tags: 网络
categories: iOS开发
author: Lanvige
---

通过网络获取内容既缓慢，成本又高，有些响应需要在客户端和服务器之间进行多次往返通信，这拖延了浏览器可以使用和处理内容的时间，同时也增加了访问者的数据成本。因此，缓存和重用以前获取的资源的能力成为优化性能很关键的一个方面。

HTTP 协议中定义了一些数据缓存的策略，每个浏览器都进行了实现。我们所要做的就是，确保每个服务器响应都提供正确的 HTTP 头，以指导浏览器何时可以缓存响应以及可以缓存多久。

<!--more-->

## 缓存方式

对于 HTTP Cache，我们最常用的有以下方式：

- Last-Modified / If-Modified-Since (HTTP 1.0)
- ETag / If-None-Match (HTTP 1.1)
- Cache-Control (HTTP 1.1)

### 1、Last-Modified / If-Modified-Since (HTTP 1.0)

*不能减少请求，但是可以减少应用服务的重复计算，并减少返回数据流量。*

``` text 
# response header
Last-Modified: Mon, 03 Jan 2016 17:45:57 GMT

# request header
If-Modified-Since: Mon, 03 Jan 2016 17:45:57 GMT
```

如果服务器每次都重新发送请求，获取完整的数据。这样做效率较低，因为如果资源未被更改过，就没有理由再去下载与缓存中已有的完全相同的字节。

原理是：浏览器第二次请求一个资源，会带上上次请求返回的一些 HTTP 头： If-Modified-Since，服务端判断浏览器所请求资源在此期间是否发生了变化，如未变化，直接返回 304 和空body。

### 2、 ETag / If-None-Match (HTTP 1.1)

*(Entity Tag) 和Last-Modified功能一致。*

不同于 Last-Modified，ETag 使用的是一个令牌值，这对于一些无法用时间来标识的资源为更方便。

##### Request Headers

```
If-None-Match:"1edec-3e3073913b100"
```

##### Response 

```
# General
Request URL:https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html
Request Method:GET
Status Code:304 Not Modified

# Headers
ETag:"1edec-3e3073913b100"
```

在请求中，多了一个 `If-None-Match`，服务器端返回的 `Status Code` 为 `304 Not Modified`。

### 3、Cache-Control (HTTP 1.1)

*真正减少请求数量。指定客户端什么要缓存，什么不要。而且可以指定中间设备是否应该缓存（路由器，CDN等）。*

Cache-Directive 有很多值，可以用来配置缓存的策略。

-  no-cache / no-store：
   - no-cache：这个很容易让人产生误解，使人误以为是响应不被缓存。实际上 no-cache 是会被缓存的，只不过每次在向客户端（浏览器）提供响应数据时，缓存都要向服务器评估缓存响应的有效性。？？？？如果有了 etag，还要 no-cache 干啥？
   - no-store：这个才是响应不被缓存的意思。每个请求必须获取完整的响应。
- public / private：
	- public 意思是中间服务设备（路由器，CDN）能缓存内容。
   - private 是指只有用户浏览器才可以缓存。
- must-revalidation / proxy-revalidation 
	- 如果缓存的内容失效，请求必须发送到服务器/代理以进行重新验证。如果失效，本身就是要重新验证的。。。为什么还要 must-revalidation?
- max-age
	- max-age=xxx (xxx 为数值) 缓存的内容将在 xxx 秒后失效。


### 4、HTTP/1.1 Protocol

HTTP/1.1 规范中定义了新的规范，取代了之前用来定义响应缓存策略的头.

- ETag / If-None-Match (HTTP 1.1)
- Cache-Control (HTTP 1.1)

当前的所有浏览器都支持 Cache-Control，因此，使用它就够了。新的规范会重写旧的值（Cache-Control 会重写 Expires 的规则）。

## 最优策略

![](http-cache-decision-tree.png)


## 参见：

- [HTTP 缓存](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=zh-cn)
- [Increasing Application Performance with HTTP Cache Headers](https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers)