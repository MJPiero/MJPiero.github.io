---
title: MongoDB如何开启认证权限功能
date: 2016-12-28 14:04:52
tags: [mongodb]
categories: [mongodb]
---
MongoDB预设不会开启认证权限功能（Authentication），所以任何人都可以随意连接到MongoDB的数据库。于是我们要去创建一个管理员的账号，并为其添加权限。
# 创建新用户
在开启MongoDB服务的情况下。执行`mongod`或则`mongod --dbpath <path to data directory>` 进入MongoDB服务。
进入服务之后，执行：
```
use admin
db.addUser("账户名称", "密码")
// 如果希望此账号只有读取的权限，则修改为
// db.addUser("账户名称", "密码", true)
```
执行完成之后，先停止MongoDB服务，在执行下面指令重新开启MongoDB服务：
```
mongod --auth
```
# 进入库
启动完成之后，在连接上MongoDB Shell，在其中执行以下命令才可进入到admin库中：
```
use admin 
db.auth("账户名称", "密码")
```
