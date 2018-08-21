---
title: 解决国内Ruby Gem Install 失败问题
tags: [Ruby, 环境搭建]
categories: [环境搭建]
date: 2016-10-21 14:55:48
---

做一个国内的程序员真的是很辛苦很辛苦，各种被墙，科技还怎么进步捏。好在上有政策下有对策，国内大神还是很良心的给我们提供了很多不少解决方案。

淘宝团队提供了国内可以快速访问的镜像地址，官方地址：https://ruby.taobao.org/

官方有详细的替换的方法，这里我也按照我的流程过一遍。
# 安装Ruby
首先，我的系统是win7。

在window上安装Ruby，可以通过下载RubyInstaller工具（ http://rubyinstaller.org/ ）快速安装：

![](/images/QQ截图20160413155659.png)
# 替换成taobao镜像
之后 `win+R` 键打开运行窗口，输入cmd 快速打开命令行程序。
```
$ gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
$ gem sources -l
*** CURRENT SOURCES ***

https://ruby.taobao.org
# 请确保只有 ruby.taobao.org
$ gem install rails
```
以上是淘宝官方的方法。很简单，然而现实总是没这么顺利…

和我一样安装遇到SSL证书错误的请看这里：[解决gem install SSL 证书错误](http://blog2.pierrothall.com/2016/10/21/%E8%A7%A3%E5%86%B3gem-install-SSL-%E8%AF%81%E4%B9%A6%E9%94%99%E8%AF%AF/)