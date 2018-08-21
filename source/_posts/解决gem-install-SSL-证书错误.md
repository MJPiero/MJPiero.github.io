---
title: 解决gem install SSL 证书错误
tags: [环境搭建, Ruby]
categories: [环境搭建]
date: 2016-10-21 16:40:10
---

这个问题我是在替换gem镜像路径的时候遇到的。

详情见：[解决国内Ruby Gem Install 失败问题](http://blog2.pierrothall.com/2016/10/21/%E8%A7%A3%E5%86%B3%E5%9B%BD%E5%86%85Ruby-Gem-Install-%E5%A4%B1%E8%B4%A5%E9%97%AE%E9%A2%98/)

这个其实也折腾了我一个多小时在网上找解决办法。
# 问题
相信有部分的人在按照上面方法安装的时候出现了和我一样的问题：

![](/images/QQ截图20160413163537.png)

在网上搜索一圈之后，解释是：
> ruby 没有包含 SSL 证书，所以 https 的链接被服务器拒绝。

本来这个情况下，只要改用http路径就好了，偏偏淘宝已经停止基于HTTP协议的镜像服务了。

于是我继续在网上搜了一圈，找到了如下的解决办法（来源：https://gist.github.com/fnichol/867550 ）:
# 解决
先下载证书 http://curl.haxx.se/ca/cacert.pem ，然后再环境变量里设置 SSL_CERT_FILE 这个环境变量，并指向 cacert.pem 文件。

![](/images/QQ截图20160413165101.png)

之后再在`cmd.exe`中输入命令：

```
set SSL_CERT_FILE=C:\path\to\cacert.pem
```
之后再按照上面的方法来操作一遍~~~

![](/images/QQ截图20160413165633.png)

Perfect！！