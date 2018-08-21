---
title: 包与NPM
tags: [nodeJs]
categories: [nodeJs]
date: 2016-12-08 10:39:20
---

在说到NPM之前，应该先提及CommonJS的包规范。

CommonJS包规范定义很简单，它是由包结构和包描述文件两个部分组成。

# 包结构

包结构是用于组织包中的各种文件。完全符合CommonJS规范的包目录应该包含如下文件。

* package.json ———— 包描述文件。
* bin ———— 用于存放可执行二进制文件的目录。
* lib ———— 用于存放js代码的目录。
* doc ———— 用于存放文档的目录。
* test ———— 用于存放单元测试用例的代码。

# 包描述文件

包描述文件用于表达非代码相关的信息，它是一个JSON格式的文件（package.json），位于根目录下。

这里我们就只介绍 NPM 的 package.json 文件定义了哪些必需的字段：

* name ———— 项目名称。
* version ———— 版本。在 http://semver.org/ 上有详细的定义，通常为 major.minor.revision 格式。
* author ———— 作者。
* description ———— 项目简介。
* keywords ———— 关键词数组。用于NPM中做分类搜索的。
* repository ———— 托管源代码的位置列表。示例：`{ "type": "git", "url": "https://package/path" }`
* license ———— 当前包所使用的许可证列表。示例：`[{ "type": "GPLv2", "url": "http://www.example.com/licenses/gpl.html", }]`
* engines ———— 指明该项目所需要的nodejs版本。
* bugs ———— 返回bug的网页地址或则邮箱地址。
* contributors ———— 贡献者列表。
* scripts ———— 指定了运行脚本命令的npm命令行缩写。可以自行写好运行脚本。
* dependencies ———— 指定了项目运行所依赖的模块。`npm install XX --save`
* devDependencies ———— 指定项目开发所需要的模块。`npm install XX --save-dev`
> * __波浪号（tilde）+指定版本：__比如~1.2.2，表示安装1.2.x的最新版本（不低于1.2.2），但是不安装1.3.x，也就是说安装时不改变大版本号和次要版本号。
> * __插入号（caret）+指定版本：__比如ˆ1.2.2，表示安装1.x.x的最新版本（不低于1.2.2），但是不安装2.x.x，也就是说安装时不改变大版本号。需要注意的是，如果大版本号为0，则插入号的行为与波浪号相同，这是因为此时处于开发阶段，即使是次要版本号变动，也可能带来程序的不兼容。
> * __latest：__安装最新版本。
* peerDependencies ———— 用来供插件指定其所需要的主工具的版本（从npm 3.0版开始，peerDependencies不再会默认安装了）。
* bin ———— 用来指定各个内部命令对应的可执行文件的位置。
* main ———— 指定加载的入口文件。
* config ———— 用于向环境变量输出值。示例：`{ "port" : "8080" }`，则在`server.js`脚本就可以直接引用config里的值 `http.createServer(...).listen(process.env.npm_package_config_port)`

下面是express项目的package.json文件，可以参考下：
```
{
	"name": "express",
	"description": "Sinatra inspired web development framework",
	"version": "3.3.4",
	"author": "TJ Holowaychuk <tj@vision-media.ca>",
	"contributors": [
		{
			"name": "TJ Holowaychuk",
			"email": "tj@vision-media.ca"
		},
		{
			"name": "Aaron Heckmann",
			"email": "aaron.heckmann+github@gmail.com"
		},
		{
			"name": "Ciaran Jessup",
			"email": "ciaranj@gmail.com"
		},
		{
			"name": "Guillermo Rauch",
			"email": "rauchg@gmail.com"
		}
	],
	"dependencies": {
		"connect": "2.8.4",
		"commander": "1.2.0",
		"range-parser": "0.0.4",
		"mkdirp": "0.3.5",
		"cookie": "0.1.0",
		"buffer-crc32": "0.2.1",
		"fresh": "0.1.0",
		"methods": "0.0.1",
		"send": "0.1.3",
		"cookie-signature": "1.0.1",
		"debug": "*"
	},
	"devDependencies": {
		"ejs": "*",
		"mocha": "*",
		"jade": "0.30.0",
		"hjs": "*",
		"stylus": "*",
		"should": "*",
		"connect-redis": "*",
		"marked": "*",
		"supertest": "0.6.0"
	},
	"keywords": [
		"express",
		"framework",
		"sinatra",
		"web",
		"rest",
		"restful",
		"router",
		"app",
		"api"
	],
	"repository": "git://github.com/visionmedia/express",
	"main": "index",
	"bin": {
		"express": "./bin/express"
	},
	"scripts": {
		"prepublish": "npm prune",
		"test": "make test"
	},
	"engines": {
	"node": "*"
	}
}
```
> __参考文献：__
> - 《深入浅出Node.js》朴灵
> - [http://javascript.ruanyifeng.com/nodejs/packagejson.html](http://javascript.ruanyifeng.com/nodejs/packagejson.html)