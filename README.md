# [![SPF][]](https://youtube.github.io/spfjs/)

[![Version][]](https://badge.fury.io/js/spf)
[![Status][]](https://travis-ci.org/youtube/spfjs)
[![InlineDocs][]](https://inch-ci.org/github/youtube/spfjs)

结构化页面片段 （或简称 SPF）是来自 YouTube 的为快速导航和
页面更新而生的轻量级的 JS 框架。

使用渐进增强和 HTML5，SPF 通过更新页面在导航中改变的部分
而不是整个页面更新，来与你的网站交互来实现更快，更流畅的
用户体验。SPF 提供一个响应格式来发送文档片段，它有一个强大
的脚本和样式管理系统，一个内存级缓存，即时处理机制和其他
更多功能。

**在 [youtube.github.io/spfjs][] 深入学习**


## 概述

SPF 允许你利用静态初始页面的优势，同时又获得动态页面加载的
性能和用户体验的好处：

**用户体验**
1. 获得最快的初始页面加载。
2. 在导航期间保持响应式持久接口。

**性能**
1. 利用现有的静态渲染技术。
2. 每次导航加载小块响应和更少的资源文件。

**开发**
1. 使用任何服务端语言和模板系统。
2. 有效的通过使用相同的代码来实现静态和动态渲染。

## 下载

通过 [npm][] 安装:

```sh
npm install spf
```

通过 [Bower][] 安装:

```sh
bower install spf
```

或者，查看下载页面 [download page][] 来下载最新的发布和
最小化 JS 的 CDN 链接：
> [Download SPF](https://youtube.github.io/spfjs/download/)


## 开始

SPF 客户端库是一个 ~10K [UMD][] 的 JS 文件，没有任何依赖。
它可能是异步延迟加载的。所有函数通过 `spf` 对象公开。

**开启 SPF**

要添加 SPF 到你的网站，引入 JS 文件并运行 `spf.init()` 。

```html
<script>
  spf.init();
</script>
```

**发送请求**

SPF 不会自动改变你网站的导航，而是使用渐进增强来开启特定
链接的动态导航。只需添加一个 `spf-link` 类到一个 `<a>`
标签并激活 SPF 。

从静态导航触发：

```html
<a href="/destination">Go!</a>
```

到动态导航：

```html
<!-- Link enabled: a SPF request will be sent -->
<a class="spf-link" href="/destination">Go!</a>
```

**返回响应**

在静态导航中，会发送一个完整的HTML页面。在动态
导航，只发送片段，使用JSON作为传输。
当 SPF 向服务器发送请求时，它会追加一个
可配置的标识符到 URL ，以便您的服务器可以
正确处理请求。 (默认，会使用`?spf=navigate`.)

在下面的例子中， 中间内容和下部页脚。在动态导航，只有中间
内容的片段被发送， 因为标头和页脚不改变。

从静态导航出发：

`GET /destination`

```html
<html>
  <head>
    <!-- Styles -->
  </head>
  <body>
    <div id="masthead">...</div>
    <div id="content">
      <!-- Content -->
    </div>
    <div id="footer">...</div>
    <!-- Scripts -->
  </body>
</html>
```

到动态导航：

`GET /destination?spf=navigate`

```json
{
  "head": "<!-- Styles -->",
  "body": {
    "content":
        "<!-- Content -->",
  },
  "foot": "<!-- Scripts -->"
}
```

要了解全面的信息请查看文档 ：

01. [开始](https://github.com/TonyGao/spfjs/blob/master/doc/documentation/start.md)
02. [响应](https://github.com/TonyGao/spfjs/blob/master/doc/documentation/responses.md)
03. [事件](https://github.com/TonyGao/spfjs/blob/master/doc/documentation/events.md)
04. [资源](https://github.com/TonyGao/spfjs/blob/master/doc/documentation/resources.md)
05. [版本化](https://github.com/TonyGao/spfjs/blob/master/doc/documentation/versioning.md)
06. [缓存化](https://github.com/TonyGao/spfjs/blob/master/doc/documentation/caching.md)
07. [预取化](https://github.com/TonyGao/spfjs/blob/master/doc/documentation/prefetching.md)

## 浏览器支持

要使用动态导航，SPF 需要 HTML5 历史记录 API。对此所有
当前的浏览器都有广泛的支持，包括 Chrome 5+，Firefox 4+
和IE 10+。查看完整的浏览器兼容列表，请看 [Can I Use][] 。
基础功能，如 AJAX 样式页面更新和脚本/样式加载，有更
广泛地支持，如 IE 8+。



## 获取帮助

想针对 SPF 给予反馈，评论或提问，发送至 <spfjs@googlegroups.com>.

文件 Bug 或特性需求，上  [GitHub][].

加入我们的邮件列表 [mailing list][] 并在 Twitter 上粉我们 [@spfjs][]
以了解最新消息。


## 协议

MIT  
Copyright 2012-2017 Google, Inc.



[youtube.github.io/spfjs]: https://youtube.github.io/spfjs/
[npm]: https://www.npmjs.com/
[Bower]: http://bower.io/
[download page]: https://youtube.github.io/spfjs/download/
[UMD]: https://github.com/umdjs/umd
[documentation]: https://youtube.github.io/spfjs/documentation/
[Can I Use]: http://caniuse.com/#feat=history
[GitHub]: https://github.com/youtube/spfjs/issues
[mailing list]: https://groups.google.com/group/spfjs
[@spfjs]: https://twitter.com/spfjs

[SPF]: https://youtube.github.io/spfjs/assets/images/banner-728x388.jpg
[Version]: https://badge.fury.io/js/spf.svg
[Status]: https://secure.travis-ci.org/youtube/spfjs.svg?branch=master
[InlineDocs]: https://inch-ci.org/github/youtube/spfjs.svg?branch=master
