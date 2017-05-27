---
title: Events
description: Handle SPF events and the navigation life cycle.
---

SPF 旨在导航期间为开发者提供足够的灵活性，来控制应用程序
逻辑并提供UI更新，如进度条。


## 导航生命周期

基本事件流程图如下，更详细解释如下：


                   导航生命周期            事件已委派
                        +                                   
                        |                                   
                        |                                   
                        +---------------------->[ spfready ]
                        |                                   
                        |                                   
    +-------------------+                                   
    |                   |                                   
    |                   |                                   
    |     +----------------------------+                    
    |     |             |              |                    
    |  api|navigate  dom|click  history|popstate            
    |     |             |              |                    
    |     |             +------+-------+                    
    |     |                    |           +--->[ spfclick ]
    |     |                    +-----------+                
    |     |                    |           +->[ spfhistory ]
    |     +-------------+------+                            
    |                   |                                   
    |                   |                                   
    |                   +-------------------->[ spfrequest ]
    |                   |                                   
    |               send|request                            
    |                   |                                   
    |                   +--------------+                    
    |                   |              |                    
    |                   |       history|pushstate           
    |                   |              |                    
    |                   +--------------+                    
    |                   |                                   
    |                   |                                   
    |            receive|response                           
    |                   |                                   
    |                   +-------------------->[ spfprocess ]
    |                   |                                   
    |                   |                                   
    |            process|response                           
    |                   |                                   
    |                   +----------------------->[ spfdone ]
    |                   |                                   
    |                   |                                   
    +-------------------+                                   
                        |                                   
                        |                                   
                        v                                   


## 事件描述

[API][] 里的所有API都被定义在 [spf.Event][] 对象里。
事件列表及其描述如下：

**`spfclick`**  
当处理一个有效链接的点击（例如一个
有效的 `spf-link` 类或父元素）。尽早使用显示的说明会
发生一个导航，或提供的元素级UI回调。

**`spfhistory`**  
Fired when handling a `popstate` history event, indicating the
user has gone backward or forward; similar to `spfclick`.

**`spfhistory`** 
在处理 `popstate` 历史事件时发出，说明用户已经向后或向前；
类似于`spfclick`。

**`spfrequest`**  
在发送导航请求之前触发。用于处理导航的开始并提供全局级别的UI回调
（即开始一个进度条）。此事件在一个请求被发送之前被触发，
此请求是发送所有类型导航的请求：点击，后退/跳转和 API 调用。 
（请注意，即使从缓存中获取响应，也没有实际的网络请求，此事件也被触发）。

**`spfprocess`**  
从网络或从缓存中接收到响应时触发和处理。用于更新 UI 回调（即预热进度条）
并在内容更新之前处理事件监听器。

**`spfdone`**  
响应处理完成后触发。用于完成 UI 回执（即完成进度条）并初始化内容
更新后的事件监听器。


## 回执和取消

如果用 [spf.navigate][] 手动开始导航，那么取代处理事件的是你可能会在
一个实现自 [spf.RequestOptions][]  接口对象里传递一个回调。几乎所有
事件和回调都可以通过调用 `preventDefault` 或返回 `false` 来
取消。这些事件和它们对应的回调和它们的取消操作的列表如下：

| Event        | Callback    | State                         | Cancel |
|:-------------|:------------|:------------------------------|:-------|
| `spfclick`   |             | Link Clicked                  | Ignore |
| `spfhistory` |             | Back/Forward Clicked          | Ignore |
| `spfrequest` | `onRequest` | Started; Sending Request      | Reload |
| `spfprocess` | `onProcess` | Processing; Response Received | Reload |
| `spfdone`    | `onDone`    | Done                          |        |



[API]: ../api.md
[spf.Event]: ../api.md#spf.event
[spf.navigate]: ../api.md#spf.navigate
[spf.RequestOptions]: ../api.md#spf.requestoptions
