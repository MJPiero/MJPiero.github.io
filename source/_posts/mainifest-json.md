---
title: mainifest.json
date: 2018-08-23 12:24:55
tags: [web移动端, 缓存, javascript]
categories: [javascript]
---

在看别人开发的架子的时候，看到了这个文件，网上大概搜了一下，在此mark一下。

developers.google.com 上有个简介，中译过来被叫做“网络应用清单”，很多人也将他和web离线缓存联系在一起。
实际上，开发者可以利用 mainifest.json 控制在用户想要看到应用的区域（例如移动设备主屏幕）中如何向用户显示网络应用或网站，指示用户可以启动哪些功能，以及定义其在启动时的外观。

mainifest.json 提供了将网站书签保存到设备主屏幕的功能。当网站以这种方式启动时：

- 它具有唯一的图标和名称，以便用户将其与其他网站区分开来。
- 它会在下载资源或从缓存恢复资源时向用户显示某些信息。
- 它会向浏览器提供默认显示特性，以避免网站资源可用时的过渡过于生硬。
- 它通过一个文本文件中的元数据这一简单机制完成所有这些工作。那就是网络应用清单。

>注：尽管您可以在任何网站上使用该文件，它们却是 PWA 的必备要素。

## 创建清单
下面是一个示例：
```
{
  "short_name": "AirHorner",
  "name": "Kinlan's AirHorner of Infamy",
  "icons": [                              // 自定义图标
    {
      "src": "launcher-icon-1x.png",
      "type": "image/png",
      "sizes": "48x48"
    },
    {
      "src": "launcher-icon-2x.png",
      "type": "image/png",
      "sizes": "96x96"
    },
    {
      "src": "launcher-icon-4x.png",
      "type": "image/png",
      "sizes": "192x192"
    }
  ],
  "start_url": "index.html?launcher=true",  // 设置启动网址
  "background_color": "#000000"             // 设置背景颜色
}
```
确保包括以下内容：

- 在用户主屏幕上用作文本的 short_name。
- 在网络应用安装横幅中使用的 name。

## 将清单的相关信息告知浏览器
在您创建清单且将清单添加到您的网站之后，将 link 标记添加到包含网络应用的所有页面上，如下所示：
```
<link rel="manifest" href="/manifest.json">
```

## 测试您的清单
如果您想要手动验证网络应用清单是否已正确设置，请使用 Chrome DevTools 的 Application 面板上的 Manifest 标签。

![](https://developers.google.com/web/fundamentals/web-app-manifest/images/devtools-manifest.png?hl=zh-cn)

> [google开发者原文](https://developers.google.com/web/fundamentals/web-app-manifest/?hl=zh-cn)