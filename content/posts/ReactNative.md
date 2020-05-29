---
title: React Native的技术分享
date: "2015-05-14"
template: "post"
draft: false
slug: "ReactNative"
category: "Tech"
tags:
  - "React"
  - "iOS"
description: "一个多月前准备的React Native分享，昨天终于给大家分享完毕。M组最近事情很多，还伴随着固定人员离职，还是要求自己不能停啊。课件PDF与实例代码已经放到了Github上。"
#socialImage: "/media/2015Summary.jpg"
---

一个多月前准备的React Native分享，昨天终于给大家分享完毕。M组最近事情很多，还伴随着固定人员离职，还是要求自己不能停啊。

课件PDF与实例代码已经放到了Github上，[看这里](https://github.com/hkongm/ReactNativeGuide)。

下面是自述文件搬运哈。

## React Native Guide 入门
ReactNative开发入门，制作了一个ListView实现的原生列表App，数据使用fetch从http url获取。

用于部门内部技术分享的程序demo和讲课pdf。

代码内含详细改动注释。

### 文件介绍
1. `0original.js` ReactNative init生成的原始`index.ios.js`文件
2. `1course.js` 简单的文本改动
3. `2singleRow.js` 单行列表，字符串常量
4. `3singleRowWithData.js`单行列表，数据变量
5. `4multiRowWithArray.js` 多行列表，引入`List View`，使用数组数据
6. `5multiRowWithFetch.js` 多行列表，使用`fetch`获取URL数据接口
7. `6multiRowWithFetchUE.js` 用户体验优化，在加载完成前加入loading的View。

