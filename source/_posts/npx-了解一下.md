---
title: npx 了解一下
date: 2018-09-05 15:44:54
tags: [nodejs,npm]
categories: [nodejs]
---

在看vue-cli 3的时候才看到有关[npx](https://github.com/zkat/npx)的指令。npm更新至5.2.0会自动安装的一条指令，主要是为了方面开发者使用包内提供的命令行工具。
比如说：我们要用一个 `create-app-cli` 工具包去执行里面的某个指令，使用npm命令的话
```
npm i -g create-app-cli
create-app-cli my-app
```
使用npx命令：
```
npx create-app-cli my-app
```
该命令会临时安装 `create-app-cli` 包，并且执行包内指令，完成后坏删除create-app-cli包，下次再执行需要重新安装。嗯，这个对于强迫症患者有奇效。

再者，npx 会帮你执行依赖包里的二进制文件。
```
npm i -D webpack
./node_modules/.bin/webpack -v
# 也可以写成
# `npm bin`/webpack -v   // bash命令写法
```
npx命令：
```
npm i -D webpack
npx webpack -v
```
npx 会自动查找当前依赖包中的可执行文件，如果找不到，就会去 PATH 里找。如果依然找不到，就会帮你安装。

甚至还支持运行远程仓库的可执行文件：
```
npx github:piuccio/cowsay hello
```

最有意思的是 npx http-server 可以一句话帮你开启一个静态服务器，初次运行会稍微慢一些。
```
npx http-server
```
还可以指定不同的node版本去运行npm script：
```
npx -p node@6 npm run dev
```
可以省去使用nvm指令了。
可以尝试使用看看。