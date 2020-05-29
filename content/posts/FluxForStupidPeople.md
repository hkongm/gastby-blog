---
title: 给新手（笨笨）准备的Flux介绍
date: "2015-01-14"
template: "post"
draft: false
slug: "FluxForStupidPeople"
category: "Tech"
tags:
  - "React"
  - "Flux"
  - "翻译"
description: "作为一个脑袋不咋灵光的人，在我挣扎着学习Flux时，希望有人能告诉我这篇文章里的内容。这不是一篇简单的、好的文档，而且它还有许多需要改正的部分内容。这篇文章，紧接着上篇《给新手（笨笨）准备的ReactJS介绍》"
#socialImage: "/media/2015Summary.jpg"
---

> 原文地址：[http://blog.andrewray.me/flux-for-stupid-people/](http://blog.andrewray.me/flux-for-stupid-people/)

篇幅过长，没有阅读（[TL;DR](http://zhidao.baidu.com/link?url=J-CJXq3dC4oPgQacI5pukD_S4-9HagnMMGB4YJT9hUFufaHxH7enybUz7e6vBEpW4IyV8wOZqNLvl_CMEYs6Jq)）。作为一个脑袋不咋灵光的人，在我挣扎着学习Flux时，希望有人能告诉我这篇文章里的内容。这不是一篇简单的、好的文档，而且它还有许多需要改正的部分内容。

这篇文章，紧接着上篇《给新手（笨笨）准备的ReactJS介绍》

<!-- more -->

我该用Flux么？

如果你的应用要处理一些**动态数据**的话，那么答案是“**是的**”，你很可能应该用Flux。


如果你的应用只是不用共享状态的**静态页面**，且你永远不需要保存或者更新数据，那么答案是“**不**”，Flux不会给你带来任何提升。


为什么用Flux？


小幽默下，因为Flux本是个难以理解的概念，为什么你还把问题整的更加复杂？


90%的iOS应用通过**table view呈现数据**。iOS工具包把数据呈现与数据模型定义的非常好，以至于让开发过程变得很简单。


在前端，我们没有如iOS那样的便利条件，相反的**我们有个大麻烦**，那就是没人知道怎么正确的构建一个前端应用程序。说真的，我在这个行业工作了几年了，从没有一个“最佳实践”讲授这方面内容的。相对应的“库”倒是很多，jQuery？Angular？Backbone？Handlebars？这些库都避开了数据流（data flow）的处理，数据流该怎么处理始终困扰着我们。


Flux是啥？


Flux是一个描述了对指定的事件和监听器进行“单向”数据流处理的一个概念词。所以根本没有名叫Flux的这么个库，但你需要的是[Flux分配器](https://github.com/facebook/flux/blob/master/src/Dispatcher.js)（Flux Dispatcher）和任何一个Javascript[事件库](http://notes.jetienne.com/2011/03/22/microeventjs.html)。


[官方文档](http://notes.jetienne.com/2011/03/22/microeventjs.html)被写得很意识流（stream of conciousness），而且它并不适合开始学习Flux。一旦你理解了Flux，再去看文档会感觉融会贯通。


不要尝试去比较Flux和MVC架构，那会让你更加困惑。


好了，让我们开始吧，为了**解释Flux的概念**我会用例子带你了解它们。

## 1. 视图“分派了”“一个操作”


一个**分配器（dispatcher）**是基于一个事件系统使用的。它被用来广播触发的事件并注册事件的回调，而且有且**只有一个全局分配器**。你应该使用Facebook [Dispatcher Library](https://github.com/facebook/flux/blob/master/src/Dispatcher.js)。它非常容易初始化：

```javascript
var AppDispatcher = new Dispatcher();  
```

让我们做个“new”按钮，用来像列表中添加个事项。

```javascript
<button onClick={ this.createNewItem }>New Item</button>  
```

在按钮被点击时发生了什么？你的视图**派发了一个特定的事件**，事件中包含着事件名和新事项的数据，如下代码：

```javascript
createNewItem: function( evt ) {
  AppDispatcher.dispatch({
    eventName: 'new-item',
    newItem: { name: 'Marco' } // example data
  });
}
```

## 2. “存储”响应了分配器事件


跟Flux一样，“Store”（存储）也是Facebook发明的一个词。在我们的应用程序中，我们需要为列表指定一个集合来存放逻辑和数据。这就是我们的“Store”，我们暂且称之为ListStore。


一个store作为一个单例模型，这意味着你不应该使用`new`来声明它。ListStore是一个全局的对象，如下：

```javascript
// 全局的对象描述了数据和逻辑
var ListStore = {

  // 模型数据的真实数据集合
  items: [],

  // 附加的方法，我们稍后会用到
  getAll: function() {
    return this.items;
  }
};
```

你的store接下来响应了被分配的事件

```javascript
var ListStore = …

AppDispatcher.register( function( payload ) {

  switch( payload.eventName ) {

    case 'new-item':

      // 我们得到了变更后（mutate）的数据!
      ListStore.items.push( payload.newItem );
      break;

  }

  return true; // 为了实现Flux的Promise必须加的

});
```

这就是Flux如何派发回调函数的传统方式。每个payload包含了一个事件名和数据。一个switch分支条件决定具体用什么操作。


🔑 关键概念：**Store并不是模型（model），一个store包含一个或多个模型（models）**


🔑 关键概念：应用中的每个store只专注于如何更新数据。**这是Flux中最关键的部分。**我们派发的事件并不知道如何添加或删除数据元素。


如果，举个例子，你程序中另一部分需要保存一些图片和它们的元数据，你应再新建个store，起名为ImageStore。在程序中，每个store**代表一个单独的“领土”（domain）**。如果你的程序比较大，领土（domain）需要命名的很容易识别，如果你的程序较小，很可能你只需要1个store。


**只有你的store们**被允许注册到分配器的回调上。你的视图不应该调用`AppDispatcher.register`。分配器只用来从视图向store们发送消息。视图将响应不同于事件的类型。

## 3. Store触发了一个change事件


我们快接近核心了！现在数据确实改变了，我们要通知全世界。


你的store触发一个事件，但**不使用分配器**。这有点令人困惑，但这就是Flux的特殊方式。让我们给我们的store一个触发事件的能力。如果你正在用[MicroEvent.js](http://notes.jetienne.com/2011/03/22/microeventjs.html)，那么是非常简单的：

```javascript
//MicroEvent.mixin( ListStore );  

//Then let's trigger our change event:

// 让我们触发change事件：

case 'new-item':

  ListStore.items.push( payload.newItem );

  // 向世界大声喊着“我们改变了！”
  ListStore.trigger( 'change' );

  break;
```

🔑 关键概念：在`触发`change事件时，不用传最新的item值，视图只关心*什么东西*发生了改变。让我们继续跟踪数据，去理解到底是为什么。

## 4. Event视图响应了change事件


现在我们需要显示列表。列表内容改变时，我们的视图要**完全的重新渲染**。那不是个错误。


首先，我们让组件首次被创建时监听从ListStore发来的`change`事件：

```javascript
componentDidMount: function() {  
  ListStore.bind( 'change', this.listChanged );
},
```

For simplicity's sake, we'll just call `forceUpdate`, which triggers a re-render. A different approach would be to store the entire list in `state`.

为了简单起见，我们只调用`forceUpdate`方法，它触发了一个重绘操作，这是个不同的处理方法，把整个列表存储到`状态(state)`中。

```javascript
listChanged: function() {  
  // Since the list changed, trigger a new render.
  this.forceUpdate();
},
```

Don't forget to clean up your event listeners when your component "unmounts," which is when it goes back to hell:

在你的组件卸载时（unmounts），不要忘记清理事件监听：

```javascript
componentWillUnmount: function() {  
  ListStore.unbind( 'change', this.listChanged );
},
```

Now what? Let's look at our render function, which I've purposely saved for last.

现在呢？让我们看下我们的渲染函数，这是整个函数的全貌：

```javascript
render: function() {

  // 切记，ListStore是全局的
  var items = ListStore.getAll();

  // 在整个列表上使用循环创建列表项
  var itemHtml = items.map( function( item ) {

    // "key"很重要，应该是唯一的，用来标记每个列表项
    return <li key={ listItem.id }>
      { listItem.name }
    </li>;

  });

  return <div>
    <ul>
        { itemHtml }
    </ul>

    <button onClick={ this.createNewItem }>New Item</button>

  </div>;
}
```

**我们回顾下整个流程。**当你为列表新添加一项时，*view分派一个动作(action)*，store响应这个动作(action)，store随之更新，同时*触发*一个change事件，view响应这个change事件进行重绘。


但还有个问题，当列表改变时我们**重绘整个view**，这是不是效率太低了？！


当然不是。


诚然，我们再一次的调用重绘方法，自然的所有渲染方法的代码都要重新运行。但**React将只重绘你修改了的真正DOM**。`render`函数实际上生成了一个"virtual DOM"，React会对比上一次`render`的输出，如果两个virtual DOM不同，React只会更新他们之间的不同DOM点而不是整个DOM。


🔑 关键概念：当store数据改变时， **view不应该关心数据是被新添、删除或者是被修改。** 它们（数据）应完全的重绘。React的“virtual DOM”的diff算法承担了“搞清楚真正的DOM节点变化”的重任。这样让开发者更轻松（原句：让你的生活更简单，避免你的血压升高）。

## 还有一件事：Action Creator是个啥玩意？


记住，当我们点击按钮，我们分派了一个特定的事件：

```javascript
AppDispatcher.dispatch({  
  eventName: 'new-item',
  newItem: { name: 'Samantha' }
});
```

如果你的许多视图都要需要触发这个事件，那么要重复书写非常多的代码。再加上，你所有的视图需要知道特定的对象格式，这样是很别扭的。所以Flux建议一个抽象，叫做 **action creators（操作生成器）**，它只是把上面的代码抽象为一个方法。

```javascript
  ListActions = {
    add: function( item ) {
      AppDispatcher.dispatch({
        eventName: 'new-item',
        newItem: item
      });
    }
  };
```

现在视图只要简单的调用`ListActions.add({ name: '...' }); `并且不用担心被分配的对象语法了。

## 未回答的问题


Flux只告诉我们了如何去管理数据流，它并**没有回答**下面这些问题：

* 如何从服务器上读取和保存数据？
* 我应该如何处理组件和非组件之间的通信？
* 我应该用什么样的事件库呢？
* 为什么Facebook还没有通过库的方式发布Flux？
* 我是否应该在store中启用一个数据层（像Backbone）？


这些问题的统一的答案是：**随你便！**

## 就是这些了


访问Facebook提供的[Example Flux Application](https://github.com/facebook/flux/tree/master/examples/flux-todomvc)作为附加学习资源。希望在读完本文后，`js/`目录下的文件会变得更容易理解吧。


[Flux文档](http://facebook.github.io/flux/docs/overview.html)里面包含了一些非常有用的知识（原文：金块nuggets），这些知识点被埋在了撰写不易文档深处。


这篇文章帮助你理解Flux，加我的推吧。
