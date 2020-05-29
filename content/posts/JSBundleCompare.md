---
title: JS打包工具效果对比事件
date: "2016-08-31"
template: "post"
draft: false
slug: "JSBundleCompare"
category: "Tech"
tags:
  - "Javascript"
  - 构建
description: "文中对比了知名的以及不怎么知名的模块打包工具的效果，打包后大小，gzip后大小，加载以及执行效率，结果让人大跌眼镜。webpack 和 browserify 远远不如 rollup 和 closure。nolan分别在安卓，iOS，PC（Chrome52）中分别测试，横向对比结果没有改变，依然是rollup closure大幅领先。"
#socialImage: "/media/2015Summary.jpg"
---

在浏览器中打开很久一直放着没看的一篇文章[The cost of small modules](https://nolanlawson.com/2016/08/15/the-cost-of-small-modules/)，来自[@nolanlawson](https://github.com/nolanlawson)的大作。

其中对比了知名的以及不怎么知名的模块打包工具的效果，打包后大小，gzip后大小，加载以及执行效率，结果让人大跌眼镜。**webpack** 和 browserify 远远不如 rollup 和 closure。nolan分别在安卓，iOS，PC（Chrome52）中分别测试，横向对比结果没有改变，依然是rollup closure大幅领先。

且可以预计的未来 Webpack2 也不会有特别大的性能提升在这方面。（原文：Unfortunately I’m not sure Webpack 2 will solve any of these problems, because although they’ll be borrowing some ideas from Rollup, they seem to be more focused on the tree-shaking aspects and not the scope-hoisting aspects.）

众所周知 Webpack2 一大升级就是 TreeShaking，但这次测试不涉及 TreeShaking，所以作者才敢这么说吧。

文章发出来后，有人在 Webpack Repo 里 建了个 [issue/2873](https://github.com/webpack/webpack/issues/2873) 同样在激烈讨论着。有兴趣的朋友们可以去看看。

评论区中有提到，webpack 的 production 模式如何？结论是，毫无影响，希望 Webpack2 正式版出来能解决这个问题吧。

最后说一句，rollup确实不错哦～

好久没发博客了，好懒，好忙，最近膝盖滑膜炎了，累啊～
