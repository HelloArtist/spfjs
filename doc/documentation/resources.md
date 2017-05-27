---
title: Resources
description: Manage script and style loading.
---

在 [Responses overview][responses] 中简要提到了，当 SPF 处理
一个响应时，它会从 `head` 片段，任意 `body`片段和 `foot`
片段 安装 JS 和 CSS。但是 SPF 有两种处理脚本和样式的方法：
非管理和管理的。两者都在下面详述。


## 非管理资源

SPF分析脚本和样式的每个片段，提取它们，并通过将它们附加
到 `<head>` 来执行它们。例如，给出以下响应：

```json
{
  "head": "
    <style>.foo { color: blue }</style>
    <link href=\"file.css\" rel=\"stylesheet\">
  ",
  "body": {
    "element-id-a": "Lorem ipsum dolor sit amet",
    "element-id-b": "consectetur adipisicing elit"
  },
  "foot": "
    <script src=\"file.js\"></script>
    <script>alert('hello');</script>
  "
}
```

然后，当SPF处理此响应时，它将执行以下操作步骤：

1.  添加 `<style>.foo { color: blue }</style>` 到文档的
    `<head>` 来评估 CSS 。
2.  添加 `<link href="file.css" rel="stylesheet">` 到文档的
     `<head>` 来加载 CSS 文件。
3.   用 HTML 代码 `Lorem ipsum dolor sit amet` 来更新 DOM id 为
`element-id-a` 的元素
4.  用 HTML 代码 `consectetur adipisicing elit` 来更新 DOM id 为
`element-id-b`的元素。
5.  添加 `<script src="file.js"></script>` 到文档 `<head>` 
 来加载 JS 文件 **并等待它执行完成**。
6.  添加 `<script>alert('hello');</script>` 到文档`<head>`
    来评估 JS。

> **注意:** 在处理之前，SPF 将等待脚本完成加载和执行。这符合浏览器行
> 为并保证了脚本的执行顺序。(见上文第4步)。要是不等待脚本执行，
> 添加 `async` 属性：
>
> ```html
> <script src="file.js" async></script>
> ```

无论你往页面或从页面发送这个响应，每次都会重复这些步骤。


## 管理的资源

However, a significant benefit of SPF is that only sections of
the page are updated with each navigation instead of the browser
performing a full reload.  That means — almost certainly — not
every script and style needs to be executed or loaded during
every navigation.

Consider the following common pattern where two scripts are
loaded per page: one containing common library code (e.g.
jQuery) and a second containing page-specific code.   For
example, a search page and an item page:

然而，SPF 的一个重要的优势是对每次导航只更新页面的部分
而不是让浏览器进行全部重载。这意味着几乎可以肯定在每次
导航时不需要对所有脚本和样式进行执行或加载。

考虑下边每个页面加载两个脚本的一般模式：一个包含普通的
库的代码（如 jQuery），第二个包含特定于页面的代码。例如，
一个搜索页面和一个子项页面：

搜索页面:

```html
<!-- common-library.js provides utility functions -->
<script src="common-library.js"></script>
<!-- search-page.js provides functions for the search page -->
<script src="search-page.js"></script>
```

子项页面:

```html
<!-- common-library.js provides utility functions -->
<script src="common-library.js"></script>
<!-- item-page.js provides functions for the item page -->
<script src="item-page.js"></script>
```

当用户从搜索页​​面导航到子项页面时，`common-library.js`文件
不需要重新加载，因为它已经在页面中。你可以指示 SPF 通过给它一个
`name`属性来管理这个脚本。然后，当它再次遇到脚本，它就不会重新
加载它了：

```html
<script name="common" src="common-library.js"></script>
```

By applying this to all the scripts, a user can navigate back
and forth between the two pages and only ever load a given file
once:

通过将它应用到所有脚本，用户可以在两个页面间前进和返回，
并且只加载给定的文件一次：

搜索页面:

```html
<script name="common" src="common-library.js"></script>
<script name="search" src="search-page.js"></script>
```

子项页面:

```html
<script name="common" src="common-library.js"></script>
<script name="item" src="item-page.js"></script>
```

现在，执行下边的导航过程：

    [ 搜索 ]--->[ 子项 ]--->[ 搜索 ]--->[ 子项 ]

然后，当SPF处理每个页面的响应时流程，将执行以下步骤：

1.  **导航到搜索页面。**
2. 加载 `common-library.js` JS 文件并等待它完成再继续。 
3.  加载 `search-page.js` JS 文件并等待它完成再继续。
4.  **导航到子项页面。**
5.  _跳过重载 `common-library.js`。_
6.  加载 `item-page.js` JS 文件并等待它完成再继续。
7.  **导航到搜索页面。**
8.  _跳过重载 `common-library.js`._
9.  _跳过重载 `search-page.js`._
10. **导航到子项页面。**
11. _跳过重载 `common-library.js`._
12. _跳过重载 `item-page.js`._

现在在这两个页面之间的导航避免了不必要的重载。

> **注意：** 请参阅 [Events][events] 文档来在导航期间适当处理
> 初始化和页面处理来避免内存泄漏和过期的事件监听器。

> **注意：** 请参阅 [Versioning][versioning] 文档来为无缝发布进行
> 脚本和样式版本间的自动切换。



[responses]: ./responses.md
[events]: ./events.md
[versioning]: ./versioning.md
