---
title: PointerEvent 来了
date: "2017-01-11"
template: "post"
draft: false
slug: "PointerEvent"
category: "Tech"
tags:
  - "Javascript"
  - "浏览器"
description: "今天看了下搁置好久的 github 通知，watching 的 Zepto 通知中有这么一条。tap double trigger on chrome 55。其中大意是说在 Chrome55 中，Double Tap 事件异常。下面的 @mbyor（不知道为啥这家伙才注册11天？小号也不可能啊）给了个比较满意的解答，而且很详尽，改你的 touch module 就行啦，blabla。"
#socialImage: "/media/2015Summary.jpg"
---

今天看了下搁置好久的 github 通知，watching 的 Zepto 通知中有这么一条。[tap double trigger on chrome 55](https://github.com/madrobby/zepto/issues/1249)。其中大意是说在 Chrome55 中，`Double Tap` 事件异常。下面的 [@mbyor](https://github.com/mbyor) （不知道为啥这家伙才注册11天？小号也不可能啊）给了个比较满意的解答，而且很详尽，改你的 `touch module` 就行啦，blabla。

其中提到了 `pointerdown` 这个事件，这是啥？貌似之前在哪儿听说过，但是没细研究过，马上 Chrome 打开 Console 输入以下代码：

```javascript
monitorEvents(document.body, 'pointerdown')
```

之后鼠标点了下页面，嘿！有东西出来了啊：

```javascript
pointerdown PointerEvent {isTrusted: true, pointerId: 1, width: 1, height: 1, pressure: 0.5…}
```

回去 issue ，顺藤摸瓜，搭梯子访问了下 [https://developers.google.com/web/updates/2016/10/pointer-events](https://developers.google.com/web/updates/2016/10/pointer-events) 

里面介绍了这次的主角 `PointerEvent` 的由来：

> We’ve had touch events for a while to help us with that, but they’re an entirely separate API specifically for touch, forcing you to code two separate event models if you want to support both mouse and touch. Chrome 55 ships with a newer standard that unifies both models: pointer events.

大意就是我们很早以前就支持 touch 事件啦，但是后来发现开发者们为了兼容 PC 和 M 经常要监听同类型的两个事件以应付鼠标和触屏的触摸，所以呢，这次我们实现了 `PointerEvent` 标准，来解救你们来啦。

```javascript
node.addEventListener('mousedown', handle)
node.addEventListener('touchstart', handle)
```

每次这么写是不是挺恶心的？以后直接监听 `pointerdown` 就行啦～而且这次的 Pointer 事件比 Mouse 和 Touch 设计的更精细、全面，如下：

* pointerover The pointer has entered the bounding box of the element. This happens immediately for devices that support hover, or before a pointerdown event for devices that do not.
* pointerenter  Similar to pointerover, but does not bubble and handles descendants differently. Details on the spec.
* pointerdown The pointer has entered the active button state, with either a button being pressed or contact being established, depending on the semantics of the input device.
* pointermove The pointer has changed position.
* pointerup The pointer has left the active button state.
* pointercancel Something has happened that means it’s unlikely the pointer will emit any more events. This means you should cancel any in-progress actions and go back to a neutral input state.
* pointerout  The pointer has left the bounding box of the element or screen. Also after a pointerup, if the device does not support hover.
* pointerleave  Similar to pointerout, but does not bubble and handles descendants differently. Details on the spec.
* gotpointercapture Element has received pointer capture.
* lostpointercapture  Pointer which was being captured has been released.

够用了吧？期待不久的将来（为什么是将来？因为标准普及到浏览器需要时间，值得庆幸的是，普及的时间越来越短了），我们能全面用上 Pointer 事件，告别事件绑定的尴尬境地。

新年来了，想说些啥，也实在不知道说啥，公司还好，团队还好，业务尚可，明年可以期待可以上更多的新技术，机会越来越多需要抓住。

在年前做个总结会吧，总结&激励&愿景，邀请其他部门的老大们过来听听，要不是UI老大想找我听听FE团队的总结，还真没重视这个事儿。

Vue看了一段时间，基本上算是会用了，也做了一些用得到Vuex，10个组件左右的不算复杂的小App，也都是练手学习用的，不过始终停留在如何使用上，还没细看过API里面的实现，怀疑自己是否会去看，心态有点变了呢。

今天就这样了，祝大家新年快乐，万事顺意，身体健康！
