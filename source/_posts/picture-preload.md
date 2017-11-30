---
title: Javascript实现图片预加载
date: 2017-08-24
comments: true
categories: Javascript
description: Javascript实现图片预加载
---

### 【前言】

图片预加载在web前端开发中运用相当广泛，比如一个H5活动主页中加入很多酷炫的动画效果，
我们先把项目中涉及到的图片下载到本地，然后再正式进入我们的项目中，这样可以优化用户体验。
好啦，废话不多说。楼宝宝直接上代码！


### (铛铛铛…干货在此) 图片预加载的具体实现
```
//图片预加载
function preloadimages(arr){
    var newimages=[], loadedimages=0
    var postaction=function(){}        //此处增加了一个postaction函数
    var arr=(typeof arr!="object")? [arr] : arr;
    function imageloadpost(){
        loadedimages++
        if (loadedimages==arr.length){
            postaction(newimages)      //加载完成用我们调用postaction函数并将newimages数组做为参数传递进去
        }
    }
    for (var i=0; i<arr.length; i++){
        newimages[i]=new Image()
        newimages[i].src=arr[i]
        newimages[i].onload=function(){
            imageloadpost();
        };
        newimages[i].onerror=function(){
            imageloadpost()
        }
    }
    return {                         //此处返回一个空白对象的done方法
        done:function(f){
            postaction=f || postaction
        }
    }
}
```

### 方法有了，要如何使用呢~
#### 首先preloadimages方法的参数一定要是数组哟~
相信大家在上个方法中已经看到了
```
//没错，这个就是对数组的校验
var arr=(typeof arr!="object")? [arr] : arr;
```

#### 调用方法及方法处理
```
//建议此处的参数事先定义好，如果数量不多也可以直接写
var imgArr = [
        "images/1.png",
        "images/2.png",
        "images/bottom.png"
    ];
preloadimages(imgArr).done(function(){
    //init();
    console.log('图片已加载完！')
});
```

### 【最后】大家只要在预加载的时候配上高大上的预加载动画~完美！