---
title: 分享个Karabiner的配置文件，cmd／opt兑换位置＋翻转鼠标滚轮
date: "2015-09-22"
template: "post"
draft: false
slug: "Karabiner"
category: "Tech"
tags:
  - "Mac"
description: "买了机械键盘后，在系统设置里无法更改修饰键（樱桃键盘是可以的……我支持了*便宜的*国产……），最后只能求助于Karabiner。"
#socialImage: "/media/2015Summary.jpg"
---

买了机械键盘后，在系统设置里无法更改修饰键（樱桃键盘是可以的……我支持了*便宜的*国产……），最后只能求助于Karabiner。

最开始的时候是直接使用预制的opt、cmd键的设置在用，在用本子自己的键盘时要切换模式，太麻烦了。

后来发现这软件可以还可以修改鼠标的行为，就想自己改改`private.xml`来配置下。于是乎，下面这段xml就出来了，功能：

1. 对换外接键盘左侧的cmd和opt键。
2. 翻转外接鼠标的滚轮方向。

这样一来，Reverse Scroll这个App也可以请出去了（据说这个App会导致一些莫名其妙的Bug）。

下面是正文。

```xml
<?xml version="1.0"?>
<root>
  <item>
    <name>Reverse Vertical Scrolling for Mouse</name>
    <identifier>private.reverse_vertical_scrolling_mouse</identifier>
    <device_not>
      DeviceVendor::APPLE_COMPUTER,
      DeviceProduct::ANY
    </device_not>
    <autogen>
      __FlipScrollWheel__
      <!-- reverse vertical scrolling -->
      Option::FLIPSCROLLWHEEL_VERTICAL
    </autogen>
  </item>
  <item>
    <name>Toggle Left Cmd and Opt for Other Keyboard</name>
    <identifier>private.toggle_left_cmd_and_opt</identifier>
    <device_not>
    DeviceVendor::RawValue::0x05ac,
    DeviceProduct::RawValue::0x0259
    </device_not>
    <autogen>
      __KeyToKey__ KeyCode::COMMAND_L, KeyCode::OPTION_L
    </autogen>
    <autogen>
      __KeyToKey__ KeyCode::OPTION_L, KeyCode::COMMAND_L
    </autogen>
  </item>

</root>
```
