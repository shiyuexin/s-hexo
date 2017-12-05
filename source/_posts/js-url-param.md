---
title: 获取url参数
date: 2017-09-21
comments: true
categories: Javascript
description: 获取当前url参数
---

### 前言

我们在开发过程中有时候就需要用到当前url中的参数，也会把一些参数放在路由中，挂在url上。
这时候我们就需要想办法获取到url上的参数。这里简单介绍两种获取的方式。

<!-- more -->
### 【url中有无#号】
1.如果url中带有#    ---> window.location.hash
2.url中不带#号     ---> location.search
3.获取完整url     ---> location.href

### 【获取url参数】

#### 1.正则法

```
function getQueryString(name) {
    var reg = new RegExp("(^|&|)"+ name +"=([^&]*)(&|$)");
    var r = window.location.hash.substr(1).match(reg);
    if(r != null) return  unescape(r[2]); return null;
}
```

调用:

```
var xxx = getQueryString('xxx')    //xxx参数名
```

#### 2.传统法

```
function getQueryString() {
        var url = window.location.hash;
        //截取url中"?"符后的字串（包含"?"）
        url = url.substring(url.indexOf("?"),url.length-1);
        var theRequest = new Object();
        if (url.indexOf("?") != -1) {
            var str = url.substr(1);
            var strs = str.split("&");
            for(var i = 0; i < strs.length; i ++) {
                theRequest[strs[i].split("=")[0]] = unescape(strs[i].split("=")[1]);
            }
        }
        return theRequest;
    }
```

调用:

```
var xxx = getQueryString().xxx;     //xxx参数名
getQueryString() //所有的参数对象
```

### 【示例】

以此链接为例：http://xxxx/#/?a=1&b=2

```
1，设置或获取对象指定的文件名或路径。
window.location.pathname    =>  "/"
2，设置或获取整个 URL 为字符串。
window.location.href    =>  "http://xxxx/#/?a=1&b=2"
3，设置或获取与 URL 关联的端口号码。
window.location.port    =>  ""
4，设置或获取 URL 的协议部分。
window.location.protocol    =>  "http:"
5，设置或获取 href 属性中在井号“#”后面的分段。
window.location.hash    =>  "#/?a=1&b=2"
6，设置或获取 location 或 URL 的 hostname 和 port 号码。
window.location.host    =>  "xxxx"
7，设置或获取 href 属性中跟在问号后面的部分。
window.location.search  =>  ""  //这里需要注意的是当出现"/#/"search获取为空
如果:http://xxxx/#/?a=1&b=2
window.location.search  =>  "?a=1&b=2"
8，获取变量的值(截取等号后面的部分)
var url = window.location.search;
// console.log(url.length);
// console.log(url.indexOf('?'));
// console.log(url.lastIndexOf('='));
var loc = url.substring(url.lastIndexOf('=')+1, url.length);
9，用来得到当前网页的域名
document.domain  =>  "xxxx"
```