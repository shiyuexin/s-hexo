---
title: vue-cli打包及常见问题解决
date: 2017-12-05
comments: true
categories: vue
description: vue-cli打包及常见问题
---

### vue-cli

之前，楼楼自己总结了下关于vue-cli的使用方式，对于刚入手要学习的伙伴们可以先去看
《vue2.0新手上路-环境搭建到发布》，地址:https://shiyuexin.github.io/vue-build.

这次这篇博文是针对使用这个脚手架，然后出现关于端口冲突及打包发布的相关问题的解答。希望对大家有帮助~

<!-- more -->

### 关于端口号

相信用过vue-cli的朋友都知道，项目运行直接在cmd或者gitbash执行 npm run dev
这个项目可以根据webpack的服务启动一个本地开发的页面地址
例如: http://localhost:8000/#/ （8000是项目默认的端口号）
或者: http://127.0.0.1:8888/#/
可是对于本地有服务的小伙伴们，可能会出现异常情况。
就我而言，我本地有nginx服务，在配置文件nginx.conf中配置了多个端口来开发项目
如果我nginx配置文件中已经出现了8000端口的设置，这个端口会发生冲突，vue的项目在浏览器中页面会异常

### 更改默认端口号

config文件下 index.js文件

```
 // Various Dev Server settings
    host: 'localhost', // can be overwritten by process.env.HOST
    port: 8000, // can be overwritten by process.env.PORT, if port is in use, a free one will be determined
    autoOpenBrowser: false,
    errorOverlay: true,
    notifyOnErrors: true,
    poll: false, // https://webpack.js.org/configuration/dev-server/#devserver-watchoptions-
```
port对应的8000端口号，改成你想设置的，然后重新启动下服务，就可以了。
so easy

### 关于项目打包

如果项目完成之后，我们需要打包发布。执行 npm run build  项目中会自动生成一个dist文件夹
这个文件夹下的所有内容就是我们需要发布的内容
但是你会发现你执行完命令之后有一个警告
```
  Build complete.

  Tip: built files are meant to be served over an HTTP server.
  Opening index.html over file:// won't work.

```
也就是说
提示：建立文件是放在一个HTTP服务器。打开index.html文件：/ /不工作。
当直接使用浏览器打开文件时，浏览器控制台会报错。

原因呢，就是很多资源都加载失败，仔细看一下路径，绝对路径，
本地盘下哪有static文件夹，那就要将打包的路径改为相对路径，
找到config\index.js文件中的build对象，将assetsPublicPath中的“/”，改为“./”即可，就在前面加个点就可以了，
并在build\build.js将这两句的提示信息删掉或注释掉，再打包直接用浏览器直接运行就好了

#尾声
到这里，这几个坑就都填完了。
最后给大家推荐一个关于vue的谷歌拓展，vue Devtools，一个vue的调试工具。
如果安装之后没有成功出现vue图标，可以尝试重启浏览器，在这个工具下面可以看到你vue项目的结构，清晰明了~~




