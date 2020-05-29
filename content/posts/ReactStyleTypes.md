---
title: React 组件样式方案对比-CSS Modules & Styled Components
date: "2018-07-25"
template: "post"
draft: false
slug: "ReactStyleTypes"
category: "Tech"
tags:
  - "React"
  - "CSS Modules"
  - "Styled Components"
description: "如何给 React 组件应用样式，是个有争议性的话题。使用 JavaScript 将 CSS 内置的 *CSS in JS* 与传统的导入外置的 CSS 文件来应用样式，哪种方案才是最好的方法？两种方案一直存在着争议。在本文中，让我们看看这些方法的不同点，进而讨论它们的优缺点。文章的最后，你会对每种技术方案都有所了解，进而找到最适合你自己的方案并应用到你自己的项目中。"
#socialImage: "/media/2015Summary.jpg"
---

如何给 React 组件应用样式，是个有争议性的话题。使用 JavaScript 将 CSS 内置的 *CSS in JS* 与传统的导入外置的 CSS 文件来应用样式，哪种方案才是最好的方法？两种方案一直存在着争议。

在本文中，让我们看看这些方法的不同点，进而讨论它们的优缺点。文章的最后，你会对每种技术方案都有所了解，进而找到最适合你自己的方案并应用到你自己的项目中。

## Vanilla CSS

先让我们尝试使用引用外置的 CSS 样式表文件的方式，当我们开发 React 应用时，会像下面这样：

```js
import React from "react";

import "./box.css";

const Box = () => (
  <div className="box">
    <p className="box-text">Hello, World</p>
  </div>
);

export default Box;
```

样式部分由一个完全分离的 `box.css` 文件导入进来：

```css
.box {
  border: 1px solid #f7f7f7;
  border-radius: 5px;
  padding: 20px;
}

.box-text {
  font-size: 15px;
  text-align: center;
}
```

构建工具会收集所引入的全部外置 CSS 文件，合并后最终链接到你的 HTML 文件中。（比如使用 [create-react-app](https://github.com/facebook/create-react-app) ）。

### 优点

因为是 Vanilla CSS ，所以你可以无限制地使用所有的 CSS 功能，比如：

* 媒体查询（Media queries）
* 帧动画（Keyframe animations）
* 伪元素（Pseudo-elements）(eg. `:before`, `:after`)
* 伪类（Pseudo-selectors）(eg. `:hover`, `:nth-child`)

你正在使用纯 CSS，Web 页面结构中最最基础的一部分。这意味着，你不用在项目中添加任何的依赖。你同时也不用担心 CSS 部分在不远的将来将会淘汰，就像本文下面介绍的那些方案。

如果你喜欢，你还可以使用你喜爱的 CSS 预处理器，比如 Sass 或者 Stylus，这些预处理器可以为你提供更强大的诸如 mixins、变量等高级功能，只要在构建过程中稍微添加下配置即可。

### 缺点

首先，你需要确认你的构建过程可以识别 CSS 的引用。幸运的是，如果你正在使用 `create-react-app`，这部分功能已经集成完备了。

另一方面，与下面其他几种方案相比，Vanilla CSS 方案无法使用 JavaScript 基于组件变量和属性来直接定制样式。这种样式方案，你需要采取一个别扭的方法（比如条件语句）来应用不同的样式类，比如下面的例子：

```js
const Button = props => {
  const classNames = ["button"];
  if (props.large) classNames.push("button-large");
  if (props.rounded) classNames.push("button-rounded");
  if (props.color) classNames.push(`button-${props.color}`);

  return <button className={classNames.join(" ")} />;
};
```

在这个示例中，我不得不添加了一些条件来更改组件的 `className` 属性，再通过 prop 传入组件。稍后我们还要回来看下这个例子，演示下相对于其他 React 样式方案，本例还可以是什么样。

但最重要的问题是，CSS 本身不是基于组件架构而设计的。CSS 是基于页面文档和 Web 页来设计的，在这些环境下，CSS 的全局命名空间和层叠书写方式（cascade）是有力的工具。但在基于组件化的应用中，全局命名空间反而成了累赘和经常发生问题的地方。

在 React 应用程序中直接使用 CSS 文件的这种方式，会让我们遇到诸如类名冲突、样式错乱等问题，都可以归咎于全局样式污染。使用 [BEM](http://getbem.com/introduction/) 命名方案，可以一定程度上缓解这种问题，但你还是无法保证第三方的（样式）代码不会影响你自己的样式。

更好的解决方案是可以直接作用于组件内的样式。这不仅可以解决全局命名空间问题，而且可以让开发者专注于组件，进而让我们可以干净整齐地打包样式代码，不必担心影响到其他组件。

（译者注：原文下方评论区有人提到了 [classnames](https://www.npmjs.com/package/classnames) 包来处理 `className`，作者也做了相应解释）

## Inline styles

下一个赋予 React 组件样式的方式是使用 Inline styles。

在组件中，通过 `style` 属性，传入使用驼峰风格的 JavasScript 代码设置好的样式规则，如下：

```js
import React from "react";

const boxStyle = {
  border: "1px solid #f7f7f7",
  borderRadius: "5px",
  padding: "20px"
};

const boxTextStyle = {
  fontSize: "15px",
  textAlign: "center"
};

const Box = () => (
  <div style={boxStyle}>
    <p style={boxTextStyle}>Hello, World</p>
  </div>
);

export default Box;
```

### 优点

这种方式的优点是，你不需要任何额外的依赖和构建步骤，你仅仅就是在用 React。这种方式可以最快速的开始项目，更适合小项目或者可以表现作者的意图的示例代码（小 Demo）。你的样式声明仅在组件内部，所以不用担心全局样式污染或者层级错误导致的问题。

因为使用 Javascript，我们便可以加入一些功能和逻辑到我们的样式中。尝试用 inline styles 这种新的方式改写上面一段 Vanilla CSS 版本的按钮例子，如下：

```js
const styles = props => ({
  fontSize: props.large ? "20px" : "14px",
  borderRadius: props.rounded ? "10px" : "0",
  background: props.color
});

const Button = props => {
  return <button style={styles(props)} />;
};
```

你可以看到，这个版本比上一个纯 CSS 版本更具描述性，而不是类似循环的一步步应用某些样式，所需要的样式可以直接作用于组件。

### 缺点

在实际项目中，我是不会推荐使用 inline styles 方式的。这是因为通过 JS 来实现 CSS ，会使我们失去许多 CSS 最好用最强大的部分：

* 我们无法做到媒体查询
* 我们失去了帧动画功能
* 我们失去了伪元素功能（比如 :before, :after），你不得不把这些内容写到你的 JSX 中
* 我们失去了伪类功能（比如 :hover, :nth-child），你需要使用 `mouseover` 和 `mouseleave` 事件来模拟 `:hover` 行为

缺失任何一个重点都有可能是严重的问题，而且你还会失去在构建步骤中转换 CSS 的福利，比如自动添加厂商前缀的功能。主观地将，这方案让人感觉到笨拙且不爽。

## CSS Modules

下一个方案，让我们来关注 [CSS Modules](https://github.com/css-modules/css-modules)。CSS Modules 就像是第一个 Vanilla CSS 方案的改进型，其将所有的类与动画名都固定在自己的作用域内。这意味着，你完全可以规避全局命名空间导致的那些问题。

```js
import React from "react";
import styles from "./box.css";

const Box = () => (
  <div className={styles.box}>
    <p className={styles.boxText}>Hello, World</p>
  </div>
);

export default Box;
```

你引入了一个 `box.css` 外部文件，就像上面第一个 Vanilla CSS 方案一样。但这一次的引入，我们给他赋予了 `styles` 名字，此模块名作为一个属性 key 在组件中的 `className` 处使用，用以应用不同的类名。而引入的 CSS 文件就是一个普通的样式文件。

```css
.box {
  border: 1px solid #f7f7f7;
  border-radius: 5px;
  padding: 20px;
}

.boxText {
  font-size: 15px;
  text-align: center;
}
```

### 优点

CSS Modules 更改了 CSS 文件中的类名并将他们变为了唯一名，这样将有效的将样式作用域化，使它们更适合基于组件的 Web 应用开发。CSS Modules 的使用，在保持了 Vanilla CSS 方案所有优点的同时，更帮助我们完全地规避了最大的（全局命名空间）问题。你仍旧可以完全控制 CSS 文件中所有的内容，而且还可以进一步的使用 CSS 预处理器（这需要升级构建步骤来支持）。

因为你的代码库内仍然包含若干纯 CSS 文件，所以哪怕是添加构建步骤，并小幅度地更改导入和应用样式的方式，对项目代码的修改量也不大。这意味着如果你将来需要抛弃 CSS Modules 方式，这种过渡也是影响最小的。

### 缺点

CSS modules 使用作用域类解决了全局 CSS 的问题，但是它还是没有解决纯 CSS 所遇到的同样的问题。

* 你需要设置你的构建步骤来支持 CSS modules （如果你同时还在使用 Sass / Stylus）。`create-react-app` 已经支持了 CSS modules。
* 像 CSS 一样，你无法通过 JavaScript 加入任何的逻辑判断

## Styled Components

[Styled Components](https://github.com/styled-components/styled-components) 是一个用于  React 的 **CSS in JS** 库，它允许你在 JS 中添加本地组件范围的样式。但区别于 inline styles，styled components 最终会将 JavaScript 编译到 CSS 中。这个库使用 [template tag literal](https://www.styled-components.com/docs/advanced#tagged-template-literals) 语法在组件中应用样式，就像下面这样：

```js
import React from "react";
import styled from "styled-components";

const BoxWrapper = styled.div`
  border: 1px solid #f7f7f7;
  border-radius: 5px;
  padding: 20px;
`;

const BoxText = styled.p`
  border: 1px solid #f7f7f7;
  border-radius: 5px;
  padding: 20px;
`;

const Box = () => (
  <BoxWrapper>
    <BoxText>Hello, World</BoxText>
  </BoxWrapper>
);

export default Box;
```

### 优点

因为最终 JavaScript 会转换成实际的 CSS，所以你可以在 styled components 中无限制的使用 CSS 任何能力。这包括媒体查询、伪元素、动画等等。相较于（第二种 Inline styles 方式）驼峰属性，我们使用的就是常规的样式写法，styled components 得以让你书写普通的 CSS，这对于熟悉 CSS 工程师来说，这种方式真的极易上手。

因为我们在使用一种 CSS in JS 方案，所以我们可以使用 JavaScript 语言的全部特性，可基于传入属性（props）来应用不同的样式。组件的传入属性（props）实际上被组件的 `styled` 的调用，这点非常强大：

```js
import React from "react";
import styled from "styled-components";

const Button = styled.button`
  background: ${props => props.color};
  font-size: ${props => props.large ? "18px" : "14px"};
  border-radius: ${props => props.rounded ? "10px" : "0"};
`;

export default props => <Button {...props} />;
```

与 Vanilla CSS 不同，样式与声明它们时创建的组件紧密相关，也解决了全局命名空间问题。

如果你喜欢 Styled Components 这种方式，那么还有其他可选的实现方案，你可以查看 [Glamorous](https://github.com/paypal/glamorous)，看看你喜欢哪个。
（译者注：Glamorous 已经停止维护了，作者已经转向了 [emotion](https://emotion.sh)）

### 缺点

使用 Styled Components 需要在你的项目中添加额外的依赖。CSS in JS 世界的变化实在是很快，可以遇见在不远的将来，很有可能会出现更好的库，使用 styled components 的同时，你已经给自己背负了技术债务。

还有另外一件事，当新人加入团队时，你需要让他们加速赶上技术革新的进度。除此之外，考虑下我在文章开始时提到的辩论，为什么使用 CSS in JS 不是一个好主意？

## CSS in JS是一个好主意么？（Is CSS in JS a good idea?）

在读过整篇文章后，你可能更倾向于使用 Styled Components 或者 CSS modules。Inline styles 对于大多数应用来说功能性不够，而且如果你还要 JS 路由的话 Styled Components 是个更好的选择。相反的，如果你倾向于传统的 CSS 方式，CSS Modules 给你一些 Vanilla CSS 无法实现的功能 - 像是作用域选择器。

就像我在文章开头提到的那样，使用 CSS in JS 是一个相当有争议性的话题，Styled Components 和 CSS Modules 的选择，二者孰优孰劣的讨论超出了本文的范围，如果你关心这场辩论，请移步这里 [Stop using CSS in JavaScript for web development](https://medium.com/@gajus/stop-using-css-in-javascript-for-web-development-fa32fb873dcc)。记住，凡事都有两方面，本文只是探讨了 Styled Components 的一些优势。

## 结论（The Verdict）

本文目的是简要的介绍了几种为 React 组件设置样式的方法的关键不同点。所以，我没有深入每种方法探讨它们的细节，如果你偏爱其中某种方式，你完全可以自己去做更深入的研究。

从现开始，在我自己的项目中，我准备应用 styled components 方案，我只是觉得这个方案提供更加灵活且更加符合组件化概念，使用它可以让我的代码尽可能的保持干净整洁。

> 原文：https://www.bhnywl.com/blog/styling-react-components/