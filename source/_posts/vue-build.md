---
title: vue2.0新手上路-环境搭建到发布
date: 2017-08-03
comments: true
categories: vue
description: vue2.0新手上路-环境搭建到发布
---

### vue2.0简介

Vue.js（读音 /vjuː/，类似于 view） 是一套构建用户界面的渐进式框架。
与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。Vue 的核心库只关注视图层，它不仅易于上手，还便于与第三方库或既有项目整合。
另一方面，当与单文件组件和 Vue 生态系统支持的库结合使用时，Vue 也完全能够为复杂的单页应用程序提供驱动。

详情见: https://cn.vuejs.org/



### 依赖环境

1.nodejs
2.安装淘宝镜像(非必选) [npm install -g cnpm --registry=https://registry.npm.taobao.org]
3.安装webpack   [npm(cnpm) install webpack -g]
4.安装vue脚手架  [npm(cnpm) install vue-cli -g]



### (铛铛铛…终于到了)根据模板创建vue项目

#### 1.vue init webpack projectname [工程名字<工程名字不能用中文>]

会有一些初始化的设置，如下输入:

Target directory exists. Continue? (Y/n) 直接回车默认(然后会下载 vue2.0模板，这里可能需要连代理)

Project name (vue-test) 直接回车默认

Project description (A Vue.js project) 直接回车默认

Author 写你自己的名字

#### 2.cd projectname [cd 命令进入创建的工程目录]

#### 3.安装项目依赖

npm install [一定要从官方仓库安装，npm 服务器在国外所以这一步安装速度会很慢]
或者用淘宝镜像安装 cnpm install [ps:可能会出现依赖缺失的情况，如果报错了就根据提示把缺失的依赖重装一遍就ok啦~]

#### 4.安装路由模块和网络请求模块
npm(cnpm) install vue-router    --save
npm(cnpm) install vue-resource  --save

#### 5.项目启动
npm run dev

#### 6.项目打包
npm run build

#### 7.附加
npm(cnpm) install jquery
npm(cnpm) install vue-file-upload
npm(cnpm) install node-sass --save-dev
npm(cnpm) install sass-loader --save-dev
npm(cnpm) install style-loader --save-dev
npm(cnpm) install css-loader --save-dev
npm(cnpm) install file-loader --save-dev



### 相关资料参考地址
http://www.runoob.com/w3cnote/vue2-start-coding.html
