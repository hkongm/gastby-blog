---
title: 给Jekyll博客重新装修
date: "2015-01-20"
template: "post"
draft: false
slug: "ThemeBlog"
category: "Tech"
tags:
  - "杂谈"
  - "Jekyll"
description: "过了个春节，好久没更新文章了，节前+节后综合症，状态始终不对，慢慢调整回来吧。"
#socialImage: "/media/2015Summary.jpg"
---

觉得Jekyll默认的样式不爽，自己搞了一套。顺便熟悉下自定义站点的一些环境变量和liquid的用法。

还需要细节再优化。

* date变量`{ { post.date | date: "%Y年%m月%d日" } }`
* `site.categories`的不遍历，试了`item.name`不行，`item.title`也不行，SO搜了下居然是`item.first`有意思
* 文章页正文定了650px的宽，for UX。右边加了同类别的文章列表
* 使用`site.related_posts`列出了相关文章
* 使用循环后if`post.categories == page.categories`，列出了同类下的文章
* [建立分类列表](http://stackoverflow.com/questions/17118551/generating-a-list-of-pages-not-posts-in-a-given-category)
* 使用了[lunar eclipse配色](http://www.colourlovers.com/palette/3622259/Lunar_Eclipse)
* 使用 `limit` 限制循环数量，使用 `倒序` 排列 `for post in site.related_posts reversed limit:4`

20日更新内容：

* 感谢dribbble上[牛逼的设计](https://dribbble.com/shots/1091493-You-know-his-usual-malarky)提供配色。
* 文章页排版修改
* `{ %assign tags = post.tag | split: ' '% }`用来拆分字符串问数组
* 首页加入了文章meta，标签反白显示
