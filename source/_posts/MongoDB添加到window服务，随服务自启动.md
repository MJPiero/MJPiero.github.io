---
title: MongoDB添加到window服务，随系统自启动
date: 2016-12-28 11:38:01
tags: [mongodb, 环境搭建]
categories: [mongodb]
---
最近在window上安转MongoDB，记录一下遇到的一些的问题。
# windows上安装MongoDB
首先去官网下载Windows安装包：https://www.mongodb.org/downloads
安装路径这里假设是安装在 `D:/soft/mongodb/`。
安装好之后，我们可以随便在一个目录下，比如在 `D:/` 根目录下创建一个目录 `D:\mongodb\`，进入该目录，新建data和logs两个目录。
然后打开控制台命令窗口（CMD），输入`D:\soft\mongodb\bin` 进入到安装目录下。
再执行：`mongod --dbpath d:/mongodb/data`，开启MongoDB服务，并将运行目录指向之前创建好的 `D:\mongodb\data` 下。
打开浏览器，进入：`http://127.0.0.1:27017`（window服务默认端口27017），这时你会看到以下提示语：
```
You are trying to access MongoDB on the native driver port. For http diagnostic access, add 1000 to the port number
```
这说明MongoDB服务已经启动了。
# 添加MongoDB服务到windows本地服务中
上面的方法要保证MongoDB服务运行，必须每次都要重复输入命令才能开启。为了方便在windows上开启MongoDB服务，我们需要将MongoDB服务到windows本地服务中，并且设置随系统启动开启。
继续回到CMD中，在安装目录中（D:\soft\mongodb\bin）执行：
```
mongod.exe --logpath d:/soft/mongodb/logs/mongodb.log --logappend --dbpath d:/soft/mongodb/data --directoryperdb --serviceName MongoDB -install --auth
```
> 此处注意 "--auth" 是将服务开启权限认证，这样别人需要账户和密码才能去访问你的数据库。
> 如果要开启认证，需要在前期运行时要在MongoDB服务中设置好账户密码。
> 详细方法可参考：[MongoDB如何开启认证权限功能](http://www.mjpiero.cc/2016/12/28/MongoDB%E5%A6%82%E4%BD%95%E5%BC%80%E5%90%AF%E8%AE%A4%E8%AF%81%E6%9D%83%E9%99%90%E5%8A%9F%E8%83%BD/)
> 如果不需要，或则选择后期再设置，可以不使用。

上面执行完毕之后，会在windows服务下创建一个名为MongoDB的服务。
执行 `net start MongoDB` 便开启MongoDB服务了。
可以在windows的服务窗口中看见MongoDB的服务状态。
![](/images/QQ截图20161228122836.png)
# 删除MongoDB服务
如果要删除MongoDB服务，首先先停止当前的MongoDB服务，这个可以去服务窗口停止。
然后在CMD中执行：`sc delete MongoDB`
这样之前安装的服务就会被删除。
# 安装时遇到的一些问题
在安装时可能遇到的一些问题：
## Windows不能在本地计算机启动MongoDB，错误代码 100
__解决办法：__ MongoDB安装目录\data\将此文件夹下的mongod.lock、storage.bson删除
## 连接数据库时发生错误 failed to execute listdatabases command
__解决办法：__ 在安装服务的时候开启权限认证，确定一下账户密码是否有误。
如果是在添加服务的时候使用了"--auth"命令，则需要进入MongoDB服务中重新添加新的账户和密码。
详细方法可参考：[MongoDB如何开启认证权限功能](http://www.mjpiero.cc/2016/12/28/MongoDB%E5%A6%82%E4%BD%95%E5%BC%80%E5%90%AF%E8%AE%A4%E8%AF%81%E6%9D%83%E9%99%90%E5%8A%9F%E8%83%BD/)