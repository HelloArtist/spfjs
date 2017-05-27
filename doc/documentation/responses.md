---
title: Responses
description:
    An overview of SPF responses and how they are processed.
---


在动态导航中，SPF 以响应的内容来更新页面。 
SPF将以下边的顺序来进行处理：

1. `title` — 更新页面标题
2. `url` — 更新页面 URL
3. `head` — 安装之前的 JS 和 CSS
4. `attr` — 设定元素属性
5. `body` — 设定元素内容并安装 JS 和 CSS
6. `foot` — 安装之后的 JS 和 CSS

> **注意:** 所有字段都是可选的，通常是都需要
> 响应值是 `title`, `head`, `body`, 和 `foot`.


响应通常采用以下格式：

```json
{
  "title": "Page Title",
  "head":
      "<style>CSS Text</style>
       <!-- and/or -->
       <link href=\"CSS URL\" rel=\"stylesheet\">
       ...",
  "body": {
    "DOM ID 1": "HTML Text...",
    "DOM ID 2": "..."
  },
  "foot":
      "<script>JS Text</script>
       <!-- and/or -->
       <script src=\"JS URL\"></script>
       ..."
}
```

这个模板遵循一般的最佳实践“样式在头部，脚本在 body 底部”。
"foot" 字段表示“ body 结束” 的部分，而无需开发者创建一个
显示的元素。

要更新特定元素属性，响应格式为如下：

```json
{
  "attr": {
    "DOM ID 1": {
      "Name 1": "Value 1",
      "Name 2": "Value 2"
    },
    "DOM ID 2":  {
      "...": "..."
    }
  }
}
```
