---
title: js模块化规范总结
date: 2018-08-29 10:17:23
tags: [javascript,模块化]
categories: [javascript]
---
早期，我们在使用js开发的时候，并没有类的概念，更不用说模块（Module）了，当然现在es2015|2016在语言标准的层面上，已经实现了模块的功能，而且实现的很简单，用意也是为了尽可能成为浏览器和服务器通用的模块解决方案。
那么，作为一个模块化的系统，需要具备的是以下能力：
1. 定义封装的模块。
2. 定义新模块对其他模块的依赖。
3. 可对其他模块的引入支持。

让我们回忆一下广为人知的几个不同的模块化规范。

## commonJS
在es2015之前，js没有模块化的规范，Nodejs的CommonJS率先制定了js的模块化标准，当然也是仅仅限制在Nodejs的服务器环境下使用。
- __模块化定义：__ 
```
// hello.js
// exports是作为模块文件唯一出口的对象
function hello(){
	console.log('hello');
}
exports.hello = hello;
```

```
// module为全局的对象，其exports属性等同于全局的exports对象
module.exports = {
	sayName(){
		console.log('my name is Luo Xia');
	}
	...
}
```
>注：这里需要注意的是一旦使用了module.exports，最终出口将会忽略在全局的exports对象上添加的属性和方法。

```
//Module.js
function a(){
	console.log('a');
} 
exports.a = a;
module.exports = {
	b(){
		console.log('b');
	}
};

//testM.js
let m = require("./Module");
console.log(typeof m.a); //undefined
console.log(typeof m.b); //function
```
- __导入：__ require('路径')
有关require导入的路径规范，可以了解一下NodeJS里npm指令，这里要提到的是模块会优先从缓存加载，会优先加载核心模块（比如npm安装好的依赖包下的模块），之后才会加载文件模块。

## AMD
AMD中译是“异步模块定义”的意思。它是一个浏览器环境下的模块化规范，可以采用同步和异步地加载方式加载模块文件。
- __模块化定义：__ 全局的 define(id?, dependencies?, factory);
id为模块标识符，dependencies为模块依赖的其他模块，factory为依赖加载完毕后执行的回调函数。

```
// 独立加载模块，不依赖其他模块，省略dependencies
define(function(){
	return {
		sayHello(){
			console.log('hello');
		}
	}
});

// 依赖其他模块
define(['jquery'],function(){
	return {
		...
	}
});
```
- __导入：__ require(['模块名称'], function ('模块变量引用'){// 代码});
```
// a.js
define(function (){
　　return {
　　　a:'hello world'
　　}
});
// b.js
require(['./a.js'], function (moduleA){
    console.log(moduleA.a); // 打印出：hello world
});
```
- __应用：__ [requireJS](https://requirejs.org/)

## CMD
CMD是在AMD基础上改进的一种规范，和AMD不同在于对依赖模块的执行时机处理不同，CMD是就近依赖，而AMD是前置依赖。也就是说AMD要在一开始就加载所有的依赖，而CMD是一级一级的加载。
- __定义模块：__ define(function(require, exports, module) {});
像AMD和CommonJS的整合版，和NodeJS兼容性会比较好，后期requireJS

```
define(function(require,exports,module){
    require('...');
	...
});
```
- __导入：__ 

```
// a.js
define(function (require, exports, module){
　　exports.a = 'hello world';
});
// b.js
define(function (require, exports, module){
    var moduleA = require('./a.js');  // 同步加载
    // requie.async(id,callback?);  异步加载
    console.log(moduleA.a); // 打印出：hello world
});
```
比起AMD默认一个当多个用，CMD则是按照API严格区分，AMD中require分全局和局部加载，但是CMD是没有全局require的，每个API都简单纯粹。
- __应用：__ [Seajs](https://seajs.github.io/seajs/)

## UMD
兼容AMD和CommonJS的规范，还兼容全局引用的方式。因此在浏览器和服务器的环境都可以应用此规范。
实际上就是一个兼容性的写法，详细可看：[UMD兼容多种模块规范](http://www.mjpiero.cc/2016/12/08/UMD%E5%85%BC%E5%AE%B9%E5%A4%9A%E7%A7%8D%E6%A8%A1%E5%9D%97%E8%A7%84%E8%8C%83/)

## ES2015|ES2016 Module规范
ES2015出来之后，终于有了自己的模块规范。下面我们直接从es6开始说。
- __定义模块化：__ 

```
// test.js
let a = 'hello';
let b = 'word';
let year = 2018;

// 和CommonJS不同的是，这里export后面跟着的不是对象，而是一系列代表出口的值
// 同样可以用as设置别名
// 例如：export{a as aName}

// 这类
export {a, b};
```
>注：export后面可以直接跟一个函数，但是不能直接跟对象，对象都需要用`{}`框起来（export default输出不同）。

- __导入：__ 
我们都知道CommonJS的模块是一个对象，es6的模块不是对象，是编译时执行的，使得编译时就能够确定依赖关系和输入输出的变量。
比如说：

```
// 从fs里面加载3个方法，其他方法不加载，这叫”编译时加载“，ES6可以在编译时就完成模块加载。
import { stat, exists, readFile } from 'fs';
```
引用的时候我们没法引用模块本身，因为它不是对象。
```
import {a, b, c} from './test';

import {a as name, b, c} from './test';

import * as ALLName from './test';
```

- __默认输出：__
当我们并不知道模块的API，可以提供默认输出：

```
export default function () {
  console.log('foo');
}
```
这时通过import导出则不需要{}，因为只有一个变量：

```
import name from './export-default';

// 例如我们在使用一些库
// import $ from 'jquery';
```
>注：es6 Module和CommonJS有个本质的区别，CommonJS加载的模块是对值的拷贝，而ES6 Module是对值的引用，这样模块定义文件内部变量的变化会实时反映到依赖文件，而CommonJS不同。

## 总结
自从es2015出来之后，AMD和CMD规范用的越来越少了，但是并不代表这些完全失去了可用性，考虑到浏览器的兼容性，虽然现在浏览器环境对es6的支持越来越完善了，但考虑对一些低版本的浏览器支持，很多在写es6的时候还是会使用babel对es6做一层转换，比如结合webpack配置去打包一个es6语法的js组件，打包出来会是UMD规范的，这是兼容性比较强的一种规范。
当然，实际上只是要大家尽可能养成模块化开发的思想，灵活的在不同环境下去运用好这些规范。