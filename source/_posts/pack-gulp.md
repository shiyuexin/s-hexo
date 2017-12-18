---
title: gulp自动化打包
date: 2017-12-06
comments: true
categories: gulp
description: gulp自动化打包
---

### 关于gulp

gulp是前端开发过程中对代码进行构建的工具，是自动化项目的构建利器；她不仅能对网站资源进行优化，
而且在开发过程中很多重复的任务能够使用正确的工具自动完成；使用她，我们不仅可以很愉快的编写代码，而且大大提高我们的工作效率。

<!-- more -->

gulp是基于Nodejs的自动任务运行器， 她能自动化地完成 javascript/coffee/sass/less/html/image/css 等文件的
的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后重复指定的这些步骤。
在实现上，她借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，使得在操作上非常
简单。通过本文，我们将学习如何使用Gulp来改变开发流程，从而使开发更加快速高效。



### gulp使用流程

在学习前，先谈谈大致使用gulp的流程
首先当然是安装nodejs，通过nodejs的npm全局安装和项目安装gulp，其次在项目里安装所需要的gulp插件，然后新建gulp的配置文件gulpfile.js并写好配置信息（定义gulp任务），最后通过命令提示符运行gulp任务即可。

安装nodejs -> 全局安装gulp -> 项目安装gulp以及gulp插件 -> 配置gulpfile.js -> 运行任务

### 安装nodejs

关于安装node，楼楼在这里就不过多赘述了。大家可以自行去下载一个node，本地安装就可以了。

### 全局安装gulp

全局安装gulp，也就是在终端执行下面命令，本地安装了淘宝镜像的小伙伴们cnpm也一样的。

```
npm(cnpm) install gulp -global(-g)
```

安装成功之后，查看安装gulp版本，版本可以正常查看就说明你已经开启了gulp的第一步咯~

```
gulp -version(-v)
```

### 项目安装gulp以及gulp插件

每个项目在根目录下要新建一个package.json文件
package.json是基于nodejs项目必不可少的配置文件，它是存放在项目根目录的普通json文件

这样一个json文件（json文件内是不能写注释的，复制下列内容请删除注释）：

```
{
  "private": true,
  "entry": {},
  "dependencies": {},
  "description":"This is for study gulp project !",    //项目描述（必须）
  "devDependencies": {     //项目依赖的插件
    "del": "^3.0.0",              //删除目标目录，目标文件
    "gulp": "^3.9.1",             //gulp
    "gulp-htmlmin": "^3.0.0",     //压缩html的插件
    "gulp-imagemin": "^4.0.0",    //压缩图片文件
    "gulp-minify-css": "^1.2.4",  //压缩css的插件
    "gulp-replace": "^0.6.1",     //文件替换
    "gulp-rev": "^8.1.0",         //文件名被hash
    "gulp-sequence": "^0.4.6",    //串行方式跑gulp任务的插件
    "gulp-tinypng": "^1.0.2",     //图片压缩工具 tinypng
    "gulp-uglify": "^3.0.0",      //js压缩插件
    "gulp-usemin": "^0.3.28"      //用来将HTML 文件中（或者templates/views）中没有优化的script 和stylesheets 替换为优化过的版本
  }
}
```

### 配置gulpfile.js

gulpfile.js是gulp项目的配置文件，是位于项目根目录的普通js文件

```
//导入工具包 require('node_modules里对应模块')
var gulp = require('gulp');
var plumber = require('gulp-plumber');
var del = require('del');
var gulpSequence = require('gulp-sequence');
var htmlmin = require('gulp-htmlmin');
var imagemin = require('gulp-imagemin');
var cssmin = require('gulp-minify-css');
var usemin = require('gulp-usemin');
var uglify = require('gulp-uglify');
var replace = require('gulp-replace');
var rev = require('gulp-rev');
var tinypng = require('gulp-tinypng');
var env = process.env.NODE_ENV || "development";
var agent = process.env.agent || 'yg';
var config = {
    app: "gulp-demo",
    src: "app",
    dist: "dist",
    env: env,
    tmp: ".tmp",
    entries: "app/scripts/main.js",
    agent: agent
};
//定义一个clean:dist任务（自定义任务名称）
gulp.task('clean:dist',function(){
    var dir = [config.tmp,config.dist];
    var option = {dot:true};
    return del(dir,option);
});
gulp.task('imagemin', function() {
    return gulp.src(config.src + "/images/**/*.{png,jpg,jpeg,gif}")
    .pipe(tinypng('BZ9n7RMXnXLTZfgTLAZzQ7--KDHkX4Ov'))
        .pipe(gulp.dest(config.dist + "/images"));
});
gulp.task('imagemin:mobile', function() {
    return gulp.src(config.src + "/mobile/images/**/*.{png,jpg,jpeg,gif}")
        .pipe(tinypng('BZ9n7RMXnXLTZfgTLAZzQ7--KDHkX4Ov'))
        .pipe(gulp.dest(config.dist + "/mobile/images"));
});
gulp.task('copycss', function() {
    return gulp.src(["styles/*.css","mobile/styles/*.css"], {
        cwd: config.src,
        dot: true,
        base: config.src
    })
        .pipe(gulp.dest(config.tmp));
});
gulp.task('copyjs', function() {
    return gulp.src(["scripts/*.js","mobile/scripts/*.js"], {
        cwd: config.src,
        dot: true,
        base: config.src
    })
        .pipe(gulp.dest(config.tmp));
});
gulp.task('copyimages', function() {
    return gulp.src(["images/**/*.{png,jpg,jpeg,gif,webp}","mobile/images/**/*.{png,jpg,jpeg,gif,webp}"], {
        cwd: config.src,
        dot: true,
        base: config.src
    })
        .pipe(gulp.dest(config.dist));
});
gulp.task('copywebp', function() {
    return gulp.src(["images/**/*.webp","mobile/images/**/*.webp"], {
        cwd: config.src,
        dot: true,
        base: config.src
    })
        .pipe(gulp.dest(config.dist));
});
gulp.task('copydist', function() {
    return gulp.src([
        "*.{ico,txt}",
        "mobile/*.{ico,txt}",
        "*.html",
        "mobile/*.html",
        "fonts/*",
        "mobile/fonts/*",
        "music/*",
        "mobile/music/*"
    ], {
        cwd: config.src,
        dot: true,
        base: config.src
    })
        .pipe(gulp.dest(config.dist));
});
gulp.task('replace', function() {
    var reg = /\{\n*(\s)*env:.*,\n*\s*agent:.*\s*\n*\}/;
    var replacement = "{env:'" + config.env + "',agent:'" + config.agent +"'}";
    return gulp.src(config.tmp + "/scripts/server.config.js")
        .pipe(replace(reg, replacement))
        .pipe(gulp.dest(config.tmp + "/scripts"));
});
gulp.task('replace:mobile', function() {
    var reg = /\{\n*(\s)*env:.*,\n*\s*agent:.*\s*\n*\}/;
    var replacement = "{env:'" + config.env + "',agent:'" + config.agent +"'}";
    return gulp.src(config.tmp + "/mobile/scripts/server.config.js")
        .pipe(replace(reg, replacement))
        .pipe(gulp.dest(config.tmp + "/mobile/scripts"));
});
gulp.task('usemin', function() {
    return gulp.src(config.src + "/**/*.html")
        .pipe(usemin({
            js: [uglify,rev],
            css: [cssmin, rev]
        }))
        .pipe(gulp.dest(config.dist))
});
gulp.task('htmlmin', function() {
    var htmlSrc = config.dist + "/**/*.html";
    return gulp.src(htmlSrc)
        .pipe(htmlmin({collapseWhitespace: true}))
        .pipe(gulp.dest(config.dist));
});
/**
 * 发布到测试环境
 */
gulp.task('build', gulpSequence(
    "clean:dist",
    "copyimages",
    "copycss",
    "copyjs",
    "replace",
    "replace:mobile",
    "copydist",
    "usemin",
    "htmlmin"
));

//gulp.task(name[, deps], fn) 定义任务  name：任务名称 deps：依赖任务名称 fn：回调函数
//gulp.src(globs[, options]) 执行任务处理的文件  globs：处理的文件路径(字符串或者字符串数组) 
//gulp.dest(path[, options]) 处理完后文件生成路径

```

### 运行命令

到这里，gulp已经基本完成
关于上面task任务的创建，根据个人项目需求来做。
最后在终端直接运行

```
gulp build(可自定义)
```

这个就包含了
```
    "clean:dist",
    "copyimages",
    "copycss",
    "copyjs",
    "replace",
    "replace:mobile",
    "copydist",
    "usemin",
    "htmlmin"
```
这些任务

### 尾声

这是我总结的gulp自动化打包用法，希望对大家有帮助。如果有问题，望不吝指出。谢谢~
