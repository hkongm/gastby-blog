---
title: 为GHPages添加了自己的域名
date: "2015-02-09"
template: "post"
draft: false
slug: "GHPagesAddDomainName"
category: "Tech"
tags:
  - "GHPages"
  - "博客"
description: "如何把自己的域名配到 GHPages 上。"
#socialImage: "/media/2015Summary.jpg"
---
参考了[这里](https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider/)

现在GHPages的项目里添加一个`CNAME`文件，**注意要全大写**。

其中只写上你要制定的域名`abc.com`。

然后去你的域名服务商，登陆域名后台，添加几条A记录。

A记录`@`和`www`指向到`192.30.252.153`和`192.30.252.154`两个IP，注意，每个域名要添加两条A记录，因为是两个IP哦。

随后用`dig`命令验证解析的结果。

```bash
dig example.com +nostats +nocomments +nocmd
example.com.   73  IN  A 192.30.252.153
example.com.   73  IN  A 192.30.252.154
```

稍等，访问你自己的域名就可以正常访问GHPages服务啦。
