---
title: Caching
description:
    Custom response caching for greater application flexibility.
---

当构建一个复杂的 Web 应用程序时，有时你想要比标准 HTTP 基础的
缓存有更多的灵活性，以此来让你的程序逻辑与性能达到最佳平衡。
SPF 提供了一个本地的可配置响应缓存，你可根据需求进行调整。


## 当使用缓存时

在发出请求之前，SPF 检查本地缓存以查看是否存在有效的缓存
响应可用于请求的 URL。如果找到一个，那么它将被使用而不是
通过网络发送一个请求。如果没找到，收到响应后，SPF
将该响应放在本地缓存中以备将来使用。

默认情况下，SPF 在浏览器里的后退-前进缓存里使用与之匹配的
缓存模型：“新”导航将不使用之前的混存的响应，而“历史”导航会。
这意味着您的服务器将接收每个点击链接的请求，例如，但不使用
后退按钮。然而，`cache-unified`的配置值如果设定为 `true`，
那么 SPF 将为所有的导航使用一个可得缓存响应。这意味着你的
服务器不会接收到点击链接到你之前访问过的页面的请求，就如同
后退按钮被按下一样。

当预取 [prefetching][] 响应时, 默认情况下, SPF 将一个预取响应
放在本地的缓存作为作为一个“新”导航的备选。 之后使用，缓存
的响应将只针对“历史”导航有效。但是，如果 `cache-unified`
的配置值被设定为  `true`，那么预期响应将针对所有导航可得，
就像其他缓存的响应。


## 配置缓存

您可以使用参数配置缓存来控制缓存中条目的总数和生命周期。
增加这些设置将增加找到有效缓存的机会，缓存响应也在浏览器
中消耗更多的内存。相反，减少这些设置会降低找到有效的缓存
响应的可能，并且在浏览器中消耗更少的内存。其配置参数列表
和它们的描述如下：

**`cache-lifetime`**  
缓存条目的最长时间（毫秒）被认为是有效的。默认为 `600000`
(10分钟)。

**`cache-max`**  
总缓存条目的最大数量。默认为 `50`。

**`cache-unified`**  
用它来决定是否同意所有缓存响应，而不是分离它们
（如 历史，预取）。默认为 `false`。

## 自动垃圾收集

缓存通过两次删除条目来执行自动垃圾收集：

1. 如果发现请求的条目，但是根据配置的生命周期它已过期
，该条目将从缓存中删除而不是被使用。

2. 每次添加新条目时，缓存会执行异步垃圾收集。这垃圾
收集首先删除所有过期的条目，然后如果需要的话，以最近
最少使用（LRU）政策删除缓存中超过配置中最大值的条目。


## 手动调整缓存

有时您可能需要手动从缓存中删除条目。例如，用户可以在页面
上执行操作改变它，你希望在执行下一个到服务器时未来的请求
反映那个操作，响应将会保持同步。

每个 [spfdone][] 事件都在 [response object][] 设定一个 `cacheKey`
属性。如果你调用用于操作缓存的 API 函数时可以使用 `cacheKey` 
关联一个特定缓存条目。API 函数列表和它们的描述如下：

**`spf.cache.remove(key)`**  
从缓存删除一个条目。从一个响应对象传一个 `cacheKey`来关联
你想要删除的条目。查看 API 手册 [spf.cache.remove][] 。

**`spf.cache.clear()`**  
Removes all entries from the cache.  See also the API reference
for [spf.cache.clear][].
从缓存删除所有条目。查看 API 手册 [spf.cache.clear][] 。



[prefetching]: ./prefetching.md
[spfdone]: ../events.md#event-descriptions
[response object]: ../../api.md#spf.singleresponse
[spf.cache.remove]: ../../api.md#spf.cache.remove
[spf.cache.clear]: ../../api.md#spf.cache.clear
