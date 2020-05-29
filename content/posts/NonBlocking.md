---
title: 非阻塞渲染加载CSS的技巧
date: "2015-04-13"
template: "post"
draft: false
slug: "NonBlocking"
category: "Tech"
tags:
  - "CSS"
  - "翻译"
description: "这篇文章介绍了一个“防止‘样式表加载行为导致页面渲染被阻塞’的异步加载样式表”的技术。"
#socialImage: "/media/2015Summary.jpg"
---

*忙劲儿过去了，目前压力还好，今天抽空翻了一篇文章，表明我还活着～*

> 原文链接：[Loading CSS without blocking render](http://keithclark.co.uk/articles/loading-css-without-blocking-render/)

**作者提示：当前，不要将此技巧用于产品化，主要是兼容性问题。**

> 这篇文章介绍了一个“防止‘样式表加载行为导致页面渲染被阻塞’的异步加载样式表”的技术。

这项技术的基本原则并不是新知识。例如Fliamentgroup发布过的[loadCSS](https://github.com/filamentgroup/loadCSS)和[fonts](http://www.filamentgroup.com/lab/font-loading.html)。我的这篇文章用来阐述我的**如何实现非阻塞的加载资源**的想法。

<!-- more -->

异步下载样式表这个技巧，源自于使用`<link>`元素的同时给他设置一个不合法的`media`属性（我自己在使用`media="none"`，但其他的任何值都是可以的）的方法。当媒体查询判断这个值为假时，浏览器仍旧会下载样式表文件，但当整个页面加载渲染完毕时，这部分样式内容不会被应用到页面中。

```html
<link rel="stylesheet" href="css.css" media="none">
```

一旦，样式表被下载完成，`media`属性必须被设置为一个合法的值，这样页面才会应用这些样式定义。`onload`事件可以用来完成这项任务，将`media`属性设置为`all`。

```html
<link rel="stylesheet" href="css.css" media="none" onload="if(media!='all')media='all'">
```

使用这种方法加载CSS，会比一般的方式更快速的为用户呈现出页面的内容。重要CSS仍然可以使用通常的阻塞加载方式（或者将这些样式使用inline的方式写在页面中以获得更好的性能表现），非重要的样式部分可以渐进下载，在稍后进行转换/渲染的过程。

这项技术使用了Javascript，在不支持Javascript的浏览器中，你可以在外面套个`<noscript>`标签来实现。

```html
<link rel="stylesheet" href="css.css" media="none" onload="if(media!='all')media='all'">
<noscript><link rel="stylesheet" href="css.css"></noscript>
```

这项技术有个副作用。一旦无阻塞样式文件下载完成，文档中所有被影响的元素会进行重绘操作。在页面中插入新的样式会触发页面内容的重排操作。当页面第一次被加载时，这真的是个问题。你需要考虑到这种情况，对潜在的性能影响有个预判。

### 使用非阻塞CSS技术加载字体

字体是个页面首次渲染的性能影响点，他们属于阻塞资源，并且当字体在下载时，浏览器已经在隐形（invisible）渲染他们了。使用上面介绍的无阻塞技术的话，让“在后台下载包含字体数据的样式表”这件事成为可能，实现非阻塞页面渲染：

```html
<link rel="stylesheet" href="main.css">
<link rel="stylesheet" href="font.css" media="none" onload="if(media!='all')media='all'">
```

`font.css`包含了一套由base64编码的WOFF版本的[Merriweather](http://www.google.com/fonts/specimen/Merriweather)字体。

```css
@font-face {
  font-family: Merriweather;
  font-style: normal;
  font-weight: 400;
  src: local('Merriweather'), url('data:application/x-font-woff;charset=utf-8;base64,...')
}
```

`main.css` 包含了站点所需要的所有样式规则。下面是关于字体的声明：

```css
body {
  font-family: Merriweather, "Lucida Grande", ...;
}
```

当`font.css`正在下载时，第一个匹配的fallback字体（在本例中是Lucida Grande）被用来渲染页面内容。当字体样式表下载完成并被应用时，才会使用Merriweather来渲染。注意：字体的变化会导致页面触发重排操作。

3G网络下，我在Chrome中使用[Google Analytics Debugger站点](https://keithclark.github.io/gadebugger/)进行阻塞和非阻塞方式的测试。结果显示，使用无阻塞方式的页面`DOMContentLoaded`事件的触发时间比使用传统阻塞方式的页面要快450ms：

![](/assets/nonblocking/non-blocking-vs-blocking-graph.png)

*模拟3G网络下的瀑布图，上面为阻塞字体加载，下面为非阻塞字体加载*

将页面部署到线上，使用[webpagetest](http://www.webpagetest.org/)测试3G连接的加载时间线如下：

![](/assets/nonblocking/non-blocking-vs-blocking-timeline1.png)

*3G网络下的时间线，上面为阻塞字体加载，下面为非阻塞字体加载*

两种方法都在2.8秒内完成了整个页面，但非阻塞方法比传统阻塞方法提前1秒触发了页面渲染。我们把`main.css`的内容通过inline的方式写到页面中再次进行同一个测试，结果显示：我们获得了0.7秒的提升。

![](/assets/nonblocking/non-blocking-vs-blocking-timeline2.png)

*3G网络下的时间线（主样式inline），上面为阻塞字体加载，下面为非阻塞字体加载*

这项技术不能很好的适应字体（译者注：作者指的是浏览器兼容性差），但我建议严密关注新的CSS字体加载模块，它会大幅提升字体加载性能。

### 总结

字体加载只是非阻塞加载技术的其中一个例子，该技术还可以应用到其他方面，比如从核心CSS中分离出Javascript增强样式。
