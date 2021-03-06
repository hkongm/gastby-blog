---
title: React入门&实战
date: "2015-01-07"
template: "post"
draft: false
slug: "ReactGuide"
category: "Tech"
tags:
  - "React"
  - "翻译"
description: "下面内容是前几天在公司内部做ReactJS分享的提纲，DEMO和PDF"
#socialImage: "/media/2015Summary.jpg"
---

*下面内容是前几天在公司内部做ReactJS分享的提纲，DEMO和PDF，可以看[这里](http://github.com/hkongm/reactguide)。*

## 零、介绍
2014年最火热的三个前端技术之一

其他备选还有AngularJS和另外一个

作者：Facebook

相关：Bigpipe、模块开发、模块间的组合、相关

## 一、最简例子

1. React.render() 输出 hello world. `1hello.html`

```javascript
React.render(
  <div>Hello!</div>,
  document.body
);
```

2. React.render() 第二个参数，选择需要父节点`1append.html`

```javascript
React.render(
  <div>Hello World!</div>,
  document.getElementById('result')
);
```

## 二、JSX

JSX是一个XML语法的预处理器。使用React时可以不必使用JSX，但JSX已经基本上成为标配了。由于是XML，所以是**大小写敏感**的！`3jsx.html`

```html
<div className="red">Children Text</div>
<MyCounter count={3 + 5} />

var gameScores = {
  player1: 2,
  player2: 5
};
<DashboardUnit data-index="2">
  <h1>Scores</h1>
  <Scoreboard className="results" scores={gameScores} />
</DashboardUnit>
```

就像XML一样，JSX的标签包括一个标签名，若干属性，还有子节点。
双引号包起来的是字符串，花括号包起来的是JS表达式。

[JSX官方文档](http://facebook.github.io/react/docs/jsx-in-depth.html)
[JSX在线编译器](http://facebook.github.io/react/jsx-compiler.html)

## 三、组件 Components

**组件是React最最核心的概念！**

```javascript
// 这里定义了组件
var MessageComponent = React.createClass({
  render: function() {
    return (
      <div>{this.props.message}</div>
    );
  }
});
// 这里使用了组件（渲染到）
React.render(
  <MessageComponent message="Hello!" />,
  document.body
);
```

使用`React.createClass`来创建一个组件，创建组件只有一个要求，暨需要实现`render`方法。该方法定义组件将被怎样渲染（指的是HTML结构）。

### 组件的属性 Props
组件中一个重要的概念就是属性。`4props.html`

实际上，前面的例子中已经出现了属性。比如XML例子中的`className`和`scores`，都是组件的属性。这些属性，在组件被render后，可以使用`this.props.`来直接访问到。

## 四、事件 Event

首先，创建个组件，后我们要使用行内事件处理器（inline event handlers）进行事件处理。众所周知，`onclick`是个很差的事件处理方案，但是在React中并不是这样。很快我将告诉你为什么，在此之前，先看来学习下React怎么使用事件。`5event.html`

```javascript
var BannerAd = React.createClass({
  onBannerClick: function(evt) {
    // codez to make the moneys
  },

  render: function() {
    return <div onClick={this.onBannerClick}>Click Me!</div>;
  }
});
```

就是这样，你在节点中添加了`onXXX`方法，同时属性值说明了执行函数。

虽然在创建组件过程中，看上去貌似使用的`onClick={this.handler}`的方式来绑定事件函数，但实际上，最终的事件绑定默认是在document上，默认使用事件代理的方式，通过dom节点上的`data-reactid`属性代理到触发事件的元素。最终完成整个事件处理。

所以React的事件写法，看上去很古老低效率，但实际上他会帮你优化到极致。如此设计当然是还是模块化，事件跟组件一体而不必去访问组件外的元素。

**注意1：组件中`onClick`是大小写敏感的。XML**

**注意2：React中有更高级的事件使用方法，支持冒泡和事件代理，本节课不讲。**

## 五、状态 State

到现在为止，这里展示了React的静态渲染引擎，现在我们要介绍状态(`state`)，好让React变得更加的动态。

状态(`state`)与属性(`props`)最大的区别在于：状态是组件内部且被组件自行修改的，而属性是可以通过外部注入或者修改的。让我们看个例子：

```javascript
var StateDemo = React.createClass({
  // 这个方法用来初始化状态
  getInitialState: function() {
    return {
      clicks: 0
    }
  },

  // 这里是事件
  onBtnClick: function() {
    this.setState({
      clicks: this.state.clicks + 1
    })
  },

  render: function() {
    return (
      <div>
        <div>点击数：<strong>{this.state.clicks}</strong></div>
        <button onClick={this.onBtnClick}>点我增加点击数</button>
      </div>
    )
  }
});

React.render(
  <StateDemo />,
  document.body
);
```

### API

##### getInitialState
该接口返回组件状态的初始化值，键-值对象类型。

```javascript
getInitialState: function() {
  return {
    clicks: 0
  };
}
```

##### this.state
访问一个组件的状态，使用`this.state`，就像使用`this.props`一样。

##### this.setState
更新一个组件的状态，传入一个键值组合。

```javascript
this.setState({
  // Notice how we access the current state here
  clicks: this.state.clicks + 1
})
```

当组件的一个状态变化时，渲染器将使用新的状态值与UI重新渲染组件。
这是React实现的核心。我们将在下一个章节中着重讲述。

至此，组件、属性和状态，三个React核心介绍完毕，你可以自己动手使用React了！

## 六、综合例子

* 组件的组合
* 列表（循环）
