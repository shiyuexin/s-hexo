---
title: 获取url参数
date: 2017-01-10
comments: true
categories: javascript
description: 获取当前url里的参数的方法有很多，这里介绍2种，推荐正则大法^_^按需获取，如果想要直接获取所有参数的对象，可参考方法2。
---

### 【前言】

>这里获取url的方法我用的window.location.hash因为我的链接中有#,若链接中无#,可以考虑location.search或location.href，一定要灵活哦。
>主要原理一是正则匹配，还有就是indexOf()配合substring()。

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

### 【获取url】

以此链接为例：http://blog.xonepage.com/#/?a=1&b=2

```
1，设置或获取对象指定的文件名或路径。
window.location.pathname    =>  "/"
2，设置或获取整个 URL 为字符串。
window.location.href    =>  "http://blog.xonepage.com/#/?a=1&b=2"
3，设置或获取与 URL 关联的端口号码。
window.location.port    =>  ""
4，设置或获取 URL 的协议部分。
window.location.protocol    =>  "http:"
5，设置或获取 href 属性中在井号“#”后面的分段。
window.location.hash    =>  "#/?a=1&b=2"
6，设置或获取 location 或 URL 的 hostname 和 port 号码。
window.location.host    =>  "blog.xonepage.com"
7，设置或获取 href 属性中跟在问号后面的部分。
window.location.search  =>  ""  //这里需要注意的是当出现"/#/"search获取为空
如果:http://blog.xonepage.com?a=1&b=2
window.location.search  =>  "?a=1&b=2"
8，获取变量的值(截取等号后面的部分)
var url = window.location.search;
// console.log(url.length);
// console.log(url.indexOf('?'));
// console.log(url.lastIndexOf('='));
var loc = url.substring(url.lastIndexOf('=')+1, url.length);
9，用来得到当前网页的域名
document.domain  =>  "blog.xonepage.com"
```