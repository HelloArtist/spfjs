---
title: Prefetching
description: Prefetch responses before they're requested.
---
SPF 旨在实现快速导航，提速一个导航请求的最好的方法是
不要全部做请求。预取允许你在请求它们之前取得响应并存储
它们到本地缓存里 [local cache][] 直到它们被需要时。你也可以
预先获取脚本和样式 [scripts and styles][resources] 来提前告知
浏览器缓存。


## 预取请求

通过调用 [spf.prefetch][] 函数来与获取响应，其行为几乎等同于 
[spf.navigate][] ，并且当传递一个实现自 [spf.RequestOptions][]
接口对象时它会接收同样的回调和退出信号 [callbacks and cancellations][] ：
回调列表如下：

| Callback    | State                         | Cancel |
|:------------|:------------------------------|:-------|
| `onRequest` | Started; Sending Prefetch     | Abort  |
| `onProcess` | Processing; Response Received | Abort  |
| `onDone`    | Done                          |        |

当 SPF 发出预取请求到服务器时，它会以相同的方式将可配置的
标识符附加到 URL 作为导航，默认情况下，这个值为 `?spf=prefetch`。

当接收到预取的响应时，SPF 将放置它到本地缓存作为“新”导航的
首选。使用之后，这个缓存的响应将只作为“历史”导航的首选。
然而，如果 `cache-unified` 配置值设定为 `true`，那么
预取响应可用于所有导航，如其他缓存响应。详见 [when the cache is used][] 。


## 预取资源

当 SPF 处理一个预取响应，它将预取任何资源 [resources][] 以确保
浏览器缓存被初始化。在 JS 和 CSS 文件被需要之前预取它们以使
未来的导航更快。

要手动预取资源，会用到下边的 API 函数：

**`spf.script.prefetch(urls)`**  
预取一个或多个脚本，此脚本将被请求而不会被加载。查看此
API 手册 [spf.script.prefetch][] 。

**`spf.style.prefetch(urls)`**  
预取一个或多个样式，此样式将被请求但不会被加载。查看此
API 手册 [spf.style.prefetch][] 。


[local cache]: ./caching.md
[resources]: ./resources.md
[spf.prefetch]: ../api.md#spf.prefetch
[spf.navigate]: ../api.md#spf.navigate
[callbacks and cancellations]: ./events.md#callbacks-and—cancellations
[spf.RequestOptions]: ../api.md#spf.requestoptions
[when the cache is used]: ./caching.md#when-the-cache-is-used
[spf.script.prefetch]: ../api.md#spf.script.prefetch
[spf.style.prefetch]: ../api.md#spf.style.prefetch
