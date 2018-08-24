---
title: 你还在用第三方组件做Base64的编码和解码么？
date: 2018-08-24 16:16:43
tags: [javascript]
categories: [javascript]
---

清早看到一篇张老师的文章，才发现自己傻傻的用了很多年的第三方组件。
早期web端要做base64编码和解码的时候最常用的就是去找个开源的组件base64.js。嗯，现在都还是这样的，觉得还满完美的，看了才发现实际浏览器原生提供了Base64编码解码的方法，啊，孤陋寡闻！！！
嗯，我按照自己的理解简单介绍一下，btoa和atob的方法。来，跟我学着在浏览器的控制器上测试一下。
```
window.btoa('MJPiero');     //编码结果："TUpQaWVybw=="

window.atob("TUpQaWVybw==");    //解码结果 "MJPiero"
```
是不是很简单！还有一个要注意的地方。如果你如下输入：
```
window.btoa('博客');
```
你会发现页面会报错：
`Uncaught DOMException: Failed to execute 'btoa' on 'Window': The string to be encoded contains characters outside of the Latin1 range.`

这是因为不支持中文汉字的缘故，但是我们加一层转码就可以了。
```
window.btoa(window.encodeURIComponent('博客'));   //编码结果："JUU1JThEJTlBJUU1JUFFJUEy"

window.decodeURIComponent(window.atob('JUU1JThEJTlBJUU1JUFFJUEy'));     //解码结果 "博客"
```

此外这个还有很多可以拓展的地方，这里还是让大家去[张鑫旭](https://www.zhangxinxu.com/wordpress/2018/08/js-base64-atob-btoa-encode-decode/)老师的文章里面去看看：
- IE8/IE9的polyfill。由于这个方法支持IE10+的浏览器，所以有向下兼容的方法。
- 任意文件Base64编码。