---
title: Get Started
description:
    "Use SPF on your site: send requests, return responses."
---


## 获取代码

在开始之前，首先下载代码 **[download the code][]**.


## _选项:_ 运行Demo

如果你从 Github 克隆的项目或下载的源代码，你可以运行包含在内的 demo 程序来测试看看我们的框架是怎么工作的：

```sh
cd spfjs
npm install
npm start
```

然后你可以在浏览器打开 <http://localhost:8080/> 来查看 Demo 。
> **注意:** 你这里需要用 `npm` 来安装开发依赖
> 以及需要 `python` 来运行 demo 程序。
> 你可以通过下边的命令来检查是否安装了它们：
>
> ```sh
> npm --version
> python --version
> ```
>
> 如果你需要安装 `npm`, 进入 <https://nodejs.org/> 并
> 点击 "Install" 来获得安装器。 如果你需要安装
> `python`, 进入 <https://www.python.org/> 并进入
> "Downloads" 来下载安装程序。


## 开启 SPF

要添加 SPF 到你的网站，你需要包含这个 JS 文件，并运行
`spf.init()` 来开启新功能。

当 [下载][] 代码后，拷贝 `spf.js` 文件到存放 JS 文件的地方，
添加脚本到你的网页，并初始化 SPF:

```html
<script src="PATH-TO-YOUR-JS/spf.js"></script>
<script>
  spf.init();
</script>
```


## 发送请求

SPF不会自动更改您的网站的导航 而是使用渐进增强来开启
动态某些链接的导航。 只需要添加一个 `spf-link` 类到一个
`<a>` 标签来激活 SPF。

从静态导航出发：

```html
<a href="/destination">Go!</a>
```

到动态导航：
```html
<!-- Link enabled: a SPF request will be sent -->
<a class="spf-link" href="/destination">Go!</a>
```


## 返回响应

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



[download the code]: ../download.md
[downloading]: ../download.md
