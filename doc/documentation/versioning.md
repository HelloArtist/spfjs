---
title: Versioning
description: Automatically update script and style versions.
---

使用动态导航时，SPF会将您的网站从短暂的页面转变成长周期的
应用程序。构建一个长周期应用程序，一个重要的关注点是如何推
新代码 —— 您不希望应用于旧 HTML 的旧 JS 与新 HTML 和新 CSS
产生交互。SPF 可以自动更新您的脚本和样式版本，以确保它们与
您的内容保持同步。

> **注意：** 自动化版本仅适用于 [managed resources][].

As an example, consider a user currently on your site who
started with yesterday's code.  If you push updated scripts and
content, the user will transition to the new content and needs
the updated scripts as well.

举个例子，假设当前您网站上的用户从昨天的代码开始。如果你推
更新的脚本和内容，用户将需要过度到新内容和最新的脚本。


## 为每个版本使用唯一的 URL

你可以通过给它一个 `name` 属性来指示 SPF 来管理资源
[manage a resource][managed resources]

```html
<!-- external script -->
<script name="common" src="common-library.js"></script>
<!-- inline script -->
<script name="page">/* Page-specific script. */</script>
<!-- external style -->
<link name="common" rel="stylesheet" href="common-styles.css">
<!-- inline style -->
<style name="page">/* Page-specific style. */</style>
```

当在页面之间导航时，一个命名的资源只会是加载一次。要检测
更新，SPF 会跟踪外部 URL 或与每个资源名称相关联的内联文本。
如果什么时候发现处理响应时有已更改的 URL 或文本，SPF 将卸载
现有资源并加载新的资源。

为保证您的脚本和样式旧版和新版之间的 SPF 切换，每次都使用
唯一的 URL。例如，对于当前在您的网站上使用昨天代码的用户，
他们可能已经提供 HTML 如下：

```html
<!-- old version -->
<script name="common" src="common-library-v1.js"></script>
```

然后，当你推最新的脚本和内容时，也更新这个 URL：

```html
<!-- new version -->
<script name="common" src="common-library-v2.js"></script>
```

> **注意：** 当管理的内联脚本和样式的内容更新时，SPF 将
> 通过跟踪一个快速文本内容哈希来判断。当计算此哈希时空格
> 将被忽略，因此缩进和格式变动不会触发更新。


## 资源事件

当判断出来一个新的版本，如果需要处理已卸载资源
（例如，配置事件监听器等），SPF 将在资源被删除
之前和之后调用事件。与 [Events][] 文档里的详细说明
一样，这些事件作为 [spf.Event][] 定义在了  [API][] 。
这些事件以及描述如下：

**`spfcssbeforeunload`**  
在一个被管理的样式资源上传之前触发。当一个给定名称的最新样式被
发现时触发。

**`spfcssunload`**  
上传一个被管理的样式资源过程中触发。如果此样式是作为版本
切换的一部分来上传的，当新样式被加载后会卸载老的样式来避免
无样式化的内容出现。

**`spfjssbeforeunload`**  
在一个被管理的脚本资源上传之前触发。当一个给定名称的最新脚本
被发现时触发。

**`spfjsunload`**  
当卸载一个管理状态的脚本资源时触发。如果此脚本是作为
切换版本的一部分而卸载的，那么在新脚本加载后卸载老脚本，
与央视加载一致。


[managed resources]:  ./resources.md#managed-resources
[Events]: ./events.md
[API]: ../api.md
[spf.Event]: ../api.md#spf.event
