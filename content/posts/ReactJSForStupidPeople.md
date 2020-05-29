---
title: 给新手（笨笨）准备的ReactJS介绍
date: "2014-11-01"
template: "post"
draft: false
slug: "ReactJSForStupidPeople"
category: "Tech"
tags:
  - "React"
  - 翻译
description: "我挣扎了很久，试图理解React是个啥，它整个的架构是个啥。这篇文章正好满足了我的这个愿望。"
#socialImage: "/media/2015Summary.jpg"
---

> 原文地址：[http://blog.andrewray.me/reactjs-for-stupid-people/](http://blog.andrewray.me/reactjs-for-stupid-people/)


> 篇幅过长，没有阅读（[TL;DR](http://zhidao.baidu.com/link?url=J-CJXq3dC4oPgQacI5pukD_S4-9HagnMMGB4YJT9hUFufaHxH7enybUz7e6vBEpW4IyV8wOZqNLvl_CMEYs6Jq)）。我挣扎了很久，试图理解React是个啥，它整个的架构是个啥。这篇文章正好满足了我的这个愿望。

## React是什么

React与Angular，Ember，Backbone这类框架相比到底有啥区别？怎么处理数据？怎么与服务器通信？JSX又是TMD啥东东？“组件（component）”又应该怎么理解？


停


立马停止

### React只是一个视图层的东西

<!-- more -->

近来，在讨论JS框架的时候，React经常会被提及，但“React对比Angular”这种说法根本就不合理，因为他们就不是一种东西（框架）。Angular是一个完整的框架（包括视图），React则不是这么一种完备的框架。这就是为什么React这么难以理解，虽然目前正在形成一个基于React框架的完整的生态系统，但它本质上只是一个视图（view）。

*译者注：View layer指的是MVC的V，这里翻译成视图层*


React提供给我们一套模板语言和一些功能函数，让我们任意操作和渲染我们的HTML。HTML就是React输出的所有内容了。HTML/Javascript这样的组合，我们称之为“组件（Components）”，而这些组件又可以在内存中保存自己的一些状态（比如在tab选择组件中保存当前选择的哪个tab），但是说到底，这些依然只是HTML。

你绝对**不可能单独使用React来创建一个功能健全的动态应用程序**。下面就让我们来看下为什么。

## 优点


在使用React一段时间后，我发现它3个很重要的优点（benefits surface：优势面）。

#### 1. 你可以只观察一个源文件就知道组件会如何渲染页面


这也许是所有优势中最重要的一个，他和Angular提供的模板完全不同。让我们用一个真实的例子来说明下。


在你没有使用Javascript MVC框架的情况下，你想在用户登陆后改变页面头部的状态，你可能会这么做：

```html
<header>
  <div class="name"></div>
</header>  
```

```javascript
$.post('/login', credentials, function( user ) {
  // Modify the DOM here
  // 在这里修改DOM
  $('header .name').show().text( user.name );
});
```


我可以凭经验告诉你，这段代码足以葬送你还有你朋友们的一生。你怎么调试输出结果？谁来更新头部？还有谁在使用头部的HTML？谁来保证当用户名显示出来时的实际效果？这个DOM操作就**像GOTO跳转语句一样烂**皆因你写的这段程序。


下面是用React重写的版本：


```javascript
render: function() {  
  return <header>
    { this.state.name ? <div>this.state.name</div> : null }
  </header>;
}
```


我们可以马上告诉这个组件将怎样渲染。如果你知道当前的状态，你就可以知道渲染输出的结果。你不必调试你的程序流。当我们在开发一个复杂的应用程序时，特别是团队合作的项目是，这是非常重要（critically致命）的一点。

#### 2. 使用包含Javascript和HTML的JSX来制作组件，使理解起来更容易


混和着HTML和Javascript的这怪异的一锅粥（soup：汤）可能使你退缩。从我们成为一名程序员那时起，就习惯了不在DOM中写Javascript（比如 `onClick` 处理函数）。你要相信我，写JSX组件是非常棒的体验。


一般来讲，我们要隔离视图（HTML）与功能（Javascript）。这就导致了我们大量的Javascript文件包含了一个“页面”的所有功能，而后，你不得这复杂的环境中（JS到HTML再到JS）调试，追踪错误等。


一般的情况下，直接编写（单一）功能并打包为一个随处可用的标签，这样的自包含“组件”会让这个编码过程变得更快乐、更干净。既然你的Javascript代码和HTML是密不可分的，那么就让他们幸福滴在一起吧。

### 你可以在服务端渲染React


如果你正在构建一个公开站点或者App，而同时又遵循着“所有渲染应该发生在客户端”这一原则的话，那么你就错了。为什么 [Soundcloud](https://soundcloud.com/) 感觉很慢，而 [Stack Overflow](http://stackoverflow.com/) 感觉非常快？皆因前者是单客户端渲染机制。你可以[在服务端渲染React](https://www.npmjs.org/package/react-server-example)，而且你也应该尝试下。


Angular和其他的框架鼓励你做一些：使用PhantomJS渲染你的页面；基于UA为了搜索引擎爬虫做服务或者[为类似的服务付钱](https://prerender.io/pricing)，诸如此类恶心的事情。

## 缺点


不要忘了React**只是个视图**。

#### 1. 你不会从React中找到下面任何一个功能

* 事件系统（（即使是）与vanilla DOM 事件相比）
* AJAX功能
* 数据层（Model）的表单
* Promises
* 完整应用程序框架
* 任何基于上面功能的想法

*译者注：Vanilla JS，世界上最轻量级的Javascript框架。*


React在它自己的**对真实世界是毫无用处**上（这句好难翻，貌似少个谓语动词，很不通，大意就是比较烂）。更糟糕的是，正如我们所看到的，这促使每个人重新发明轮子。

#### 2.官方文档超级烂


重提一句，本篇博客是为新手（笨笨）写的。看看[官方文档页面](http://facebook.github.io/react/docs)侧栏的第一部分吧：

![Quick Start](http://blog.andrewray.me/content/images/2014/Sep/Screen-Shot-2014-09-22-at-12-08-18-AM.png)


快速开始向导这里只有3个部分。即便是我在没喝醉的情况下，我依然崩溃（万念俱灰）了。更下面的部分简直可以称得上是噩梦，这些章节很明显不应该出现在这里，比如“更多的参考”和“PureRenderMixin”*（译者注：这个词专有名词不翻）*。

#### 3. React库代码量大


更新：React并不是我之前文章写的144kb大，如下图，在**gzip**后，大约是**35kb**。

*译者注：上面一段是博客原文评论区有个网友回复提出来的观点，后作者对本文做了上面一段的更正。*

![瀑布图](http://blog.andrewray.me/content/images/2014/Oct/react-size.png)


这是在**不加载任何React插件库**的情况下的结果！


这是在**不加载ES5 Shim库**的情况下，不用考虑支持IE8浏览器的情况下的结果！


这是在**不加载**任何应用程序库（上面提到的Angular/Backbone等等）的情况下的结果。

*译者注：作者连说了三遍，表示对这种情况已经怒不可遏了*


相对于Angular这个完整的应用程序框架，React本身大小已经堪比Angular了。React还老老实实地提供了许多你会用到的小功能使自己变得臃肿，其希望我们在开发中能提升效率。

## 不要再谈论Flux了


也许，“Flux”是使用React进行开发时，被争执最多的一个部分了。它远比React自身更加的难理解。“Flux”这个名字就已经很装逼了。


**根本没有Flux这个东西。** Flux是一个概念，不是库。好吧，这里算有这么个[库](https://github.com/facebook/flux)


> “Flux比起库更像个模式”


呃。最烂的部分就是这个名字：React并没有重新发明（颠覆）过去40年来的UI架构知识体系也并没有带来数据管理的新理念。


**"Flux"概念简单得如下**，你的视图（*译者注：MVC中的V*）触发一个事件，这个事件更新了一个模型（*译者注：MVC中的M*），然后这个模型再触发一个事件，这个视图（*译者注：MVC中的M*）响应刚刚这个由模型触发的事件，使用最新的数据再渲染。**仅此而已**。


这种单向数据流 / 解耦的观察者模式被设计成可以保证你有效的数据一直存在你的存储器或者模型中。这是一个好事。


**Flux不好的一面**就是每个人都需要重新发明它。至今依然没有一个事件库，模型层，AJAX功能层或者其他的任何功能，[这里](https://www.npmjs.org/package/compose-flux-dispatcher) [有](https://www.npmjs.org/package/flux-action) [一些](https://www.npmjs.org/package/react-flux) [不同](https://www.npmjs.org/package/fluxxor)的“[Flux](https://www.npmjs.org/package/reflux)”实现，但它们彼此之间完全不兼容（冲突）。

## 那么我该用React么？


简单的回答：应该。


较长的回答：不幸是，可以用，在大多数地方。


**这里是为什么**你应该使用React：

* 对团队协作很有益，尤其是用户界面构建工作上（直译：大力实施UI和工作流模式）
* UI代码是易读且易维护的，组件化的用户界面是web开发的未来趋势，你需要现在开始做起来


在你切换React前，**你应慎重的再度思考如下问题**：

* 改用React，在最开始会极大地拖慢你的速度，理解属性、状态和组件之间的通信机制不是一件容易的事情，再加上官方文档那么烂。在你的整个团队都搞定前，理论上，这种情况将会持续下去。
* React不支持IE8及以下浏览器，未来也不会支持。
* 如果你的程序/网站并没有非常多的动态页面更新，你将要为微小的提升编写大量的实现代码。
* 你要重新发明许多轮子。React还年轻，还没有正式的事件/组件通信机制，你不得不从零开始，建立你的组件库。你的应用里有下拉菜单、更改窗口大小或者轮播（灯箱）效果么？你很可能不得不重新开始写这些功能了。

## 就是这些！


我的下面一篇文章[给新手（笨笨）准备的Flux介绍](http://blog.andrewray.me/flux-for-stupid-people/)。


我希望这篇文章可以帮助一些向我一样笨的朋友更好的理解React。如果这篇文章能让你的生活更简单，那么在Twitter上follow我吧。
