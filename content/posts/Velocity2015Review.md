---
title: Velocity 2015 Review
date: "2015-08-18"
template: "post"
draft: false
slug: "Velocity2015Review"
category: "Tech"
tags:
  - "技术大会"
description: "参加了日前在京举办的 Velocity 技术大会，整理下主要内容做个记录。"
#socialImage: "/media/2015Summary.jpg"
---

> 参加了日前在京举办的 Velocity 技术大会，整理下主要内容做个记录，总结＋截图，给团队的小伙伴们分享的，同时这里发一份。

## 总体感想

性能优化开始从前后端代码／技术栈方面，全转向了距离硬件更近的领域（运维），比如：

* HTTP2/SPDY（TCP）
* DNS lookup
* CDN
* 各种方法降低Latency / TTFB

又比如，更贴近客户体验端的优化，比如：

* Loading 中启用动画
* Page Block级别的Placeholder

对于JS库的态度：众多的jQuery及其插件真的有用么？不如考虑使用原生JS+Polyfill

Amazon还在支持IE6 ：给IE6用户非华丽的体验，实际是对用户也是好的*（这点很有深意，请细心体会，当然产品经理们更需要细心体会）*

作为前端，本次大会，新的技术点很少，Relay算一个，但只是提了一句，毕竟刚刚开源

## 参与制定W3C标准

未标准（未来）贡献你的微薄之力，有什么好点子尽量拿出来讨论。参与标准定制原则：

> 毋以善小而不为

### URLs
* [Specifiction Discourse](http://discourse.wicg.io)
* [The Extensible Web Manifesto](https://extensiblewebmanifesto.org)
* [WEB PLATFORM INCUBATOR COMMUNITY GROUP](https://www.w3.org/community/wicg/)
* [webcomponents](http://webcomponents.org)
* [Test the Web Forward](http://testthewebforward.org)

## Yahoo的React&Flux在S／C端的实践

### 模型
Server side: Node.js + Express

Client side: Webpack + Polyfills

Middle area: Fluxible + React + CommonJS

![图](http://dl.caichao.me/assets/velocity2015/IMG_1848.JPG)

### 目前已经实装该架构的站点
* [Yahoo台湾M站](https://tw.mobi.yahoo.com)
* [Sports Daily Fantasy App](https://sports.yahoo.com/dailyfantasy)
* [Beauty digital magazines](https://www.yahoo.com/beauty)

### 技术点

* Flux实现，使用[fluxible](http://fluxible.io)
* YUI团队开源的（也许有用的）React组件
    * [react i13n](https://github.com/yahoo/react-i13n)
    * [react intl](https://github.com/yahoo/react-intl)


## 前端性能 Alva Cheung (Google)

### 要点：
1. 数据显示：页面加载时间会随着带宽增加越来越不明显；但会跟随Latency的降低而持续减低 ![图](http://dl.caichao.me/assets/velocity2015/IMG_1859.JPG)

2. 使用DNS Prefetch

3. 谨慎使用 Prerender，**举例：google结果页的前两个结果会启用prerender，给用户造成结果秒开的体验**

2. Javascript中的代码细节：![图](http://dl.caichao.me/assets/velocity2015/IMG_1860.JPG)


    ```javascript
    function Dog(name) {
        this.name = name;
    }

    var dog1 = new Dog('Jim');
    var dog2 = new Dog('Bin'); // dog1和dog2公用一个hidden class

    dog2.breed = 'new name'; //此时内存中会新创建hidden class一个
    ```

3. Javascript中的代码细节：compiler（特制closure compiler，参考[这里](https://developers.google.com/closure/compiler/docs/js-for-compiler)和[这里是中文的](http://blog.csdn.net/zj831007/article/details/6801110)）会认识JS原文件中的注解(annotation)，有针对性的进行优化**（这是新知识点）**

    ```javascript
    /**
     * Returns a new Point which is the original point translated by distance in
     * both the x and y dimensions.
     * @param {example.Point} point
     * @param {number} distance
     * @return {example.Point}
     */
    example.translate = function(point, distance) {
      return {
        x: point.x + distance,
        y: point.y + distance
      };
    };
    ```

3. 在真实的设备上测试FPS。（Timeline工具的使用）![图](http://dl.caichao.me/assets/velocity2015/IMG_1861.JPG)

4. 事件节流技术（老生常谈的 throttling）![图](http://dl.caichao.me/assets/velocity2015/IMG_1862.JPG)


## 功能实验模型
如何在真实环境上测试N个功能？看每个新功能的实际效果。![图](http://dl.caichao.me/assets/velocity2015/IMG_1871.JPG)

### 代码方案：

1. 在JS中if（功能开关）；在SCSS中if（功能开关）![图](http://dl.caichao.me/assets/velocity2015/IMG_1872.JPG)
2. 在JS中原型链覆盖；在SCSS中class覆盖![图](http://dl.caichao.me/assets/velocity2015/IMG_1873.JPG)

### 资源方案：
1. 独立：NONONONONO ![图](http://dl.caichao.me/assets/velocity2015/IMG_1874.JPG)
2. HTTP2的Server Push ![图](http://dl.caichao.me/assets/velocity2015/IMG_1875.JPG)
3. 不变的＋可变的combo ![图](http://dl.caichao.me/assets/velocity2015/IMG_1876.JPG)

## Yahoo React & Flux 实践
开场大量篇幅讲了React基础和Flux概念

重要实践填坑内容，因为被叫出去说事，没听到……

* Flux方案的升级版本 ![图](http://dl.caichao.me/assets/velocity2015/IMG_1879.JPG)

* 启用了Apache的ESI组件，进行页面拼装，降低TTFB ![图](http://dl.caichao.me/assets/velocity2015/IMG_1881.JPG)

* 服务器React渲染的问题和技巧 ![图](http://dl.caichao.me/assets/velocity2015/IMG_1882.JPG)

* Flux下通过Store管理提炼数据 ![图](http://dl.caichao.me/assets/velocity2015/IMG_1885.JPG) (超注：我们广告这边动辄20M的JSON……噩梦）

* 服务端渲染（传统站点） VS 客户端渲染（后市场M站） VS Preload（咱们有么……） 三种渲染方式对比，适应场景 ![图](http://dl.caichao.me/assets/velocity2015/IMG_1887.JPG)

* 如果你用了lodash&&lodash.merge，记得一定要`require('lodash/object/merge')`**性能巨坑** ![图](http://dl.caichao.me/assets/velocity2015/IMG_1888.JPG)

* CSS的处理，在用[Atomic CSS
](http://acss.io/)

* 中检测要做S／C端的区别，比如：服务器端的那接口用`request`，客户端要封装一层`ajax`，统一对外暴露一套API供组件使用

## 淘宝天猫M

* 使用CDN！阿里云有强大的CDN！

* 在移动端，连接数代价远远高于代码量（file size）

* 前端能做的：域名收敛（降低其他域名出现的次数）

* 淘宝首页用one-request打包，听着类似于Webpack，Webpack未来两周内，准备好会给大家分享

* 自己写Scroll，因为ios safari 原生 scroll触发问题，不能在其中做任何操作

* 降低`composition layers`，提升FPS，会产生`composition layers`的方法 ![图](http://dl.caichao.me/assets/velocity2015/IMG_1894.JPG)

* 对图片的处理，50质量压缩＋150%的锐化（CDN自动完成）
