---
title: 如何使用Yeoman来创建你的下一个Web项目
date: "2015-01-20"
template: "post"
draft: false
slug: "YeomanGuide"
category: "Tech"
tags:
  - "翻译"
description: "Yeoman这个概念（是个概念，若干工具的集合）已经被提出很久了，很久以前也有一些文章在介绍了，这里是15年新鲜出炉的一篇介绍文章，里面配图较多，非常直观，量不大，就稍微翻译了下。"
#socialImage: "/media/2015Summary.jpg"
---

> Yeoman这个概念（是个概念，若干工具的集合）已经被提出很久了，很久以前也有一些文章在介绍了，这里是15年新鲜出炉的一篇介绍文章，里面配图较多，非常直观，量不大，就稍微翻译了下。

> 原文链接：[How to Use Yeoman to Scaffold Your Next Web App](http://wes.is/2015/01/13/how-to-use-yeoman-to-scaffold-your-next-web-app/)

<!-- more -->

![yeoman](http://wes.is/content/images/2015/01/yeoman-logo.png)

现在你决定要做一个牛逼的Web App，但你不知道从哪里下手。

你需要考虑：客户端段依赖（bower）、服务端依赖（npm/node）、测试用例、CSS框架/CSS预处理器、HTML模板引擎、JS框架、构建工具、语法验证器、文件压缩方案等等。当然，还要在这些基础上，规划如此多的文件该如何存放，以保证它们能正常工作。

这些无效的工作要花去我们若干个小时，苦逼啊。

## 让Yeoman来拯救你！

你是否有过，明明商品躺在你的购物车里直接去结账就OK了，但你还在拿着购物清单继续满世界找你的欲购商品的二逼经历？

这就是Yeoman要帮你完成的。它提供了一个地方让你可以选择所有的工具来构建你完美的开发环境。你所做的只是选择你需要的部分，Yeoman会帮你把工具文件组织妥当，让你随时可以开始开发工作。Yeoman提供一个名叫“yo”的“生成器（generators）”命令来搭建你的App。

### 我怎么开始？

如果你还没有安装node/npm，你需要先安装好它们。打开你的终端，输入下面的命令来查看是否已经安装妥当。

    node -v && npm -v

如果没有安装，访问[node官方站点](http://nodejs.org/)来获得和安装它，安装Node的同时，[npm](https://www.npmjs.com/)也会一起安装。

一旦你安装好了node / npm 你需要在全局环境安装几个命令行工具，尤其是接下来你需要使用的 **Yeoman**，**Bower**和作为构建工具的**Grunt**或者**Gulp**。我推荐你两个都安装，看看哪个更适合你。下面的命令，将会帮你安装上面这些工具。注意，如果安装过程中出现了错误，你需要在命令前加入`sudo`来启用管理员权限执行这行命令：

    npm install -g yo bower grunt-cli gulp  

一切准备就绪，让我们开始吧。

### 使用生成器

![生成器](http://wes.is/content/images/2015/01/kickstarting-nodejs-projects-with-yeoman-5-638.jpg)

让我事先说明一件事情，这里有[大量的Yeoman生成器](http://yeoman.io/generators/)用以创建你想要的任意工具组合。下面的例子中，我将向你展示如何构建一个包含JQuery、Bootstrap（使用Stylus CSS预处理器）、Jade模板引擎、Jasmin / Mocha测试框架并使用Gulp构建工具的Angular App。

首先，让我们安装Angular和Gulp的Yeoman生成器：

```bash
    npm install -g generator-gulp-angular
```

接下来，我们建立一个新目录，并进入到这个目录中：

```bash
    mkdir myapp && cd myapp  
```

输入`yo`。这会激活Yeoman问你想要做什么。

![激活Yeoman](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-28-37-AM-1.png)

在上面的截图中，几个选项被显示出来。这些是不同的Yeoman生成器。在我们这个例子中，我们去选择Gulp Angular，当前的选择会被绿色高亮，敲回车。

下一步，选择你要使用的Angular的版本。

![Angular版本](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-32-42-AM.png)

选择你的Angular模块。

![Angular模块](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-33-00-AM.png)

选择项目中你欲使用的jQuery版本。

![jQuery版本](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-33-34-AM.png)

选择REST资源库。

![REST资源库](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-36-53-AM.png)

选择一个路由器。

![路由器](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-39-04-AM.png)

选择你喜欢的UI框架。

![UI框架](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-39-53-AM.png)

选择UI框架组件。

![UI框架组件](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-40-50-AM.png)

选择你的CSS预处理器。而我偏爱Stylus :)

![CSS预处理器](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-41-52-AM.png)

选择Javascript预处理器。

![Javascript预处理器](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-42-56-AM.png)

选择一个HTML模板引擎。

![HTML模板引擎](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-44-20-AM.png)

到这里就OK了。让Yeoman处理省下的事情吧。

从这里开始，Yeoman会调用`bower install`和`npm install`来安装所有的依赖，最终创建好你的App。过程中如果遇到了访问错误（access error），你可能需要自己运行`sudo npm install`。

下面进入到了真正有意思的部分。一旦你的这些依赖安装完成，在你的App的主目录，输入下面的命令来启用你的App。

```bash
    gulp serve
```

你的终端应该显示的如下图。敲入命令后，gulp会创建一个支持监控修改（watch）的服务器，使用localhost的3000端口即可访问。

![gulp启动](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-12-52-32-AM-1.png)

程序会自动调用默认浏览器访问这个gulp监控服务器，浏览器会呈现一个临时的前端页面，里面包含了你安装的所有依赖和工具的简要介绍和链接。

![自动浏览器](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-1-05-41-AM-1.png)

用你常用的文本编辑器打开当前项目目录。

![打开目录](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-1-08-08-AM.png)

我们来看看文件结构。

![查看文件结构](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-1-10-09-AM-1.png)

### Gulp的所见即所得

当我们使用`gulp serve`命令启动了一个支持修改监控的服务器后，当我们修改文件并保存后，浏览器会立即刷新，不用我们自己去手动刷新才能看到效果。

比如，我们修改了Stylus的源文件从：

![stylus从](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-1-19-55-AM.png)

改为：

![stylus到](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-1-19-55-AM.png)

当保存后，浏览器会自动刷新成下面这样：

![自动刷新](http://wes.is/content/images/2015/01/Screen-Shot-2015-01-13-at-1-22-23-AM.png)

## 嘿！Yeoman！

如果你想加快你的项目构建速度，我强烈建议你把Yeoman加入到你的开发工作流中。你可以因此节省许多时间，远离烦恼。把更多的精力用在有意义的事情上。
