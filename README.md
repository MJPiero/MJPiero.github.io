# MJPiero.github.io

Mob 前端组博客。[Hexo](https://hexo.io/zh-cn/index.html) + [github Page](https://pages.github.com/) 搭建的静态博客。优点在于简单方便维护，依赖Github平台，无限量流量访问。

访问地址：http://www.mjpiero.cc/

## 第一步、安装环境

### 基础环境

本机先安装好 Node 环境和 git。此处省略一万字的安装说明，聪明的你一定懂得如何google。

### 安装Hexo

Hexo是一个是一个快速、简洁且高效的博客框架。官网地址放这里，有兴趣可以了解一下，闲来没事可以写个主题或者插件玩玩。

--->>> [Hexo官网](https://hexo.io/zh-cn/index.html)

全局安装Hexo-Cli：

```
npm install hexo-cli -g
```

## 第二步、安装主题包

代码拉下来直接运行是会报错的，因为这里是在第三方的主题包 [indigo](https://github.com/yscoder/hexo-theme-indigo) 上修改的主题。
由于主题包是从github上拉下来的，是个子库，push的时候没法将更新的主题配置上传上去，回导致主题包内容为空。
所以这里从我的github上Fork了一下，配置主题先把代码拉下来，进入项目根目录：
```
cd your-hexo-folder

git clone https://github.com/MJPiero/hexo-theme-indigo.git themes/indigo_my

cp -R themes/indigo_my/_source/* source/
```

这里如果更改了配置暂时由我（团子）这边来更新，如果后期他人维护可以自行建立个仓库去Fork这个主题。


## 第三步、本机运行

代码拉下来之后先：

```
npm i
```

下载好依赖。

> 这里要提一下，在package中有一个`hexo-deployer-git`插件，这个不是hexo-cli中的依赖，是个第三方插件，Hexo默认deploy的方法是ftp方法部署，这个组件的作用是为Hexo deploy添加git上传提交的功能。


本机上运行：

```
hexo server
```

启动本机服务，默认访问地址：http://localhost:4000/



## 第四步、添加新文章

创建新的md文件：

```
hexo new [layout] <title>
```

例如：hexo new 'Hello Word'

Layout 是文章布局，不写的话默认`post`布局（普通文章布局）。

> 实际上，不用指令创建的话，直接到目录`source/_post/`下新建一个md文件也是可以的。但是指令创建会默认给你添加好头文件，也就是layout当前页配置。

创建完成之后，打开文件可以看到默认已经添加好了配置信息（自己在目录底下创建的，则手动添加）：

```
---
title: Hello Word
date: 2018-08-22 00:55:08
tags:
---
```

上面配置的信息，官网文档有，这里简单列举一下

```

```

| 参数     | 描述                 | 默认值       |
| -------- | -------------------- | ------------ |
| title    | 标题                 |              |
| date     | 建立日期             | 文件建立日期 |
| tags     | 标签（不适用于分页） |              |
| comments | 开启文章的评论功能   | true         |

## 保存草稿（非必须）

这步不是必要的，如果要存储草稿的话，在新建文章的时候更改一下布局：

```
hexo new draft <title>
```

或者直接在`source/_draft/`下新建个md文件也是一样的，上面的操作实际上就是对应在不同的目录下创建文件。

草稿写好要发布的话，输入指令：

```
hexo publish [layout] <title>
```

同样layout不填默认`post`。



## 第五步、发布文章

文章全部写好之后，要发布出去。直接输入指令：

```
hexo d -g
```

如果遇到发布异常的问题，可以先输入指令：

```
hexo clean
```

清空一下生成的静态文件，再重新部署一遍。

> 注：由于连的github账户，所以可以先配置一下自己的公钥。

