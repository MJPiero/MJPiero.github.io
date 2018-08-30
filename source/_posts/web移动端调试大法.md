---
title: web移动端调试大法
date: 2016-12-29 15:02:38
tags: [环境搭建,web移动端]
categories: [环境搭建]
---
在做移动端web开发的时候，头痛遇到移动端调试的问题，虽然现在很多PC浏览器的开发工具都自带移动端调试功能，但是显然和真机测试还是有一定差距，因为移动端不同的设备会出现不同的问题，在PC模拟器上显示正常的换到真机上测试就是会出问题。
在这里整理一些常见的移动端调试方法。

__先从一般的页面调试方法开始。__
# console方式
这个是最常见的一种调试方法，通过console在浏览器控制台打印出每步所需的回调数据。
详细可以参考[Web API接口](https://developer.mozilla.org/zh-CN/docs/Web/API/Console)
# 浏览器自带的移动端模拟器调试
这个现在也比较常见了。最常用的是chrome的模拟器，当然现在流行的浏览器基本上都有开发者的模式，也都携有移动端的模拟器。比如火狐浏览器、360浏览器等。
基本上浏览器开启开发者模式的方法都统一了，在windows环境下按`F12`进入开发者攻击界面，mac环境则是下按`option+command+i`。
![](/images/QQ截图20161229151926.png)
# UC浏览器测试
UC浏览器提供了开发版方便开发者们连接测试。详见：[UC浏览器开发者版](http://plus.uc.cn/document/webapp/doc5.html)
# 第三方平台在线模拟器调试
第三方开发的平台比较方便，功能也相当强大，对于一些需要完善测试的，其实使用第三方的平台还是比较方便的。
这里我就推荐几个比较有名的第三方平台：
- [BrowserStack](https://www.browserstack.com/)
- [Keynote](http://www.keynote.com/)
- [BrowserShots](http://browsershots.org/)
- [Browsera](http://www.browsera.com/)
- [Ghostlab](http://www.vanamco.com/ghostlab/)
等等...

__下面介绍一些我比较喜欢的一些远程调试工具。__
# Weinre
之前微信开发工具中的远程调试也是基于这个开发的。
安装方法很简单快捷。
安装 Weinre：
```
npm install -g weinre
```
安装完成之后，输入指令启动：
```
weinre --httpPort 8081 --boundHost -all-
```
显示如下则启动成功。
![](/images/QQ截图20161229154152.png)
此时我们访问地址：`http://localhost:8081/` 会显示下图：
![](/images/QQ截图20161229154809.png)
在页面中也有提供几种方案，在你的项目中添加对应的js。例如：`<script src="http://192.168.55.23:8081/target/target-script-min.js#anonymous"></script>`
随后便可以通过PC访问`http://192.168.55.23:8081/client/#anonymous`就可以看到手机上访问的项目页面了。
![](/images/QQ截图20161229160034.png)
# 代理应用
这类代理应用实际上是用于抓包的，但是比起Weinre是基于开发者自身开发测试的。如果我们想要去获取其他网站的一些资源，例如http请求，或则一些js源码等。这时候使用代理工具比较合适。
mac下常使用的是[Charles](https://www.charlesproxy.com/),windows环境下可以用[Fiddler](http://www.telerik.com/fiddler)
默认开启的是8888端口。在手机上设置一个网络代理即可，这个不多详细介绍了。
# [BrowserSync](https://browsersync.io/)
这是一个很强大的多终端测试工具，它可以跨设备同步操作行为，还可以监听你的文件，在文件修改时自动刷新所有设备中页面。
[BrowserSync官方网站](https://browsersync.io/)
也可以通过npm全局安装
```
npm install -g browser-sync
```
随后进入项目根目录下
```
browser-sync start --files "*.*"
// 意思为监听当前目录下所有改动
```
终端中若显示如下，则启动成功：
![](http://7xoxxe.com1.z0.glb.clouddn.com/bs.png)
终端中显示默认访问地址：`http://localhost:3001` 则可直接进入到控制台界面。
之后和weinre类似，也是添加一段js在自己的项目底下。
BrowserSync还可以和gulp以及webpack搭配使用，详细的可以看官方文档。
# 真机测试
真机测试是一概而论的说法。由于像ios系统本身就提供了一些对前端十分友好的开发者工具。
例如，Safari浏览器自带的开发者工具，或则xcode也有测试的方法，这里我局限性比较高，有这方面条件的是可以去搜罗一下相关的方法。
再者，PhoneGap也是有调试工具的，只不过需要安装它测试app。
这也是一种方法，不过个人还是偏爱Weinre或则BrowserSync的测试工具。

以上。是我整理的一些移动端测试的方法。欢迎补充~