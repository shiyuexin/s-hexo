---
title: JS时间戳操作
date: 2016-11-28
comments: true
categories: javascript
description: 先说说为什么会这篇短文,关于时间"字符串&时间戳"转换的操作常遇到,在pc浏览器中大多时候一种或者两种即可满足需要,但是在移动端"微信/APP"对时间戳转换方法就不如chrome支持的那么完美。。。。
---


### 一.获取[当前时间戳]的三种方法
``` bash
var newTime = Date.parse(new Date());       //输出 1478764799000
```

``` bash
var newTime = (new Date()).valueOf();       //输出 1478764799000
```

``` bash
var newTime = new Date().getTime()；        //输出 1478764799000
```

>第一种：获取的时间戳是把毫秒改成000显示，第二种和第三种是获取了当前毫秒的时间戳。
>第三种方法较为常用

### 二.[当前时间]特定格式字符串
#### 2.1 原生方法格式化
``` bash
var newDate = new Date();

newDate.toString()            // Thu Nov 10 2016 18:07:23 GMT+0800 (CST) (中国标准完整日期)
newDate.toDateString()        // Thu Nov 10 2016                         (中国标准日期)
newDate.toTimeString()        // 18:07:23 GMT+0800 (CST)                 (中国标准时间)
newDate.toLocaleDateString()  // 2016/11/10
newDate.toLocaleString()      // 2016/11/10 下午6:07:23
newDate.toLocaleTimeString()  // 下午6:07:23
newDate.toGMTString()         // Thu, 10 Nov 2016 10:07:23 GMT
newDate.toISOString()         // 2016-11-10T10:07:23.363Z
newDate.toJSON()              // 2016-11-10T10:07:23.363Z
newDate.toUTCString()         // Thu, 10 Nov 2016 10:07:23 GMT
```

>经亲测:后4种方法不建议使用

#### 2.2 自定义格式化
``` bash
//任意格式转换
new Date().toLocaleDateString().replace(/(\d+)[^\d](\d+)[^\d](\d+)[^\d]?/, "$1.$2.$3"); //2016.11.10
new Date().toLocaleDateString().replace(/(\d+)[^\d](\d+)[^\d](\d+)[^\d]?/, "$2-$3"); //11-10
new Date().toLocaleString().replace(/(\d+)[^\d](\d+)[^\d](\d+)[^\d]?/, "$1.$2.$3 "); //2016.11.10 18:07:23
```

``` bash
//"2016-11-10"替换分隔符
new Date().toLocaleDateString().replace(/-/g,'/')    //2016/11/10
```

#### !注意:在移动端/app进行字符串&时间戳的相互转换,尽量用标准时间格式
``` bash
例如:"2016-11-10"在pc浏览器直接可getTime()来获取时间戳,
在微信/app就可能存在不认的情况,
"2016-11-10"先转换为自定义格式"2016/11/10"再进行时间戳转换就ok了.
```

### 三.时间格式化[通用组件]
#### 3.1 年/月/日/时/分/秒/季
``` bash
// 对Date的扩展，将 Date 转化为指定格式的String
// 月(M)、日(d)、小时(h)、分(m)、秒(s)、季度(q) 可以用 1-2 个占位符， 
// 年(y)可以用 1-4 个占位符，毫秒(S)只能用 1 个占位符(是 1-3 位的数字) 
// 例子： 
// (new Date()).Format("yyyy-MM-dd hh:mm:ss.S") ==> 2006-07-02 08:09:04.423 
// (new Date()).Format("yyyy-M-d h:m:s.S")      ==> 2006-7-2 8:9:4.18 

Date.prototype.Format = function (fmt) { //author: meizz 
    var o = {
        "M+": this.getMonth() + 1, //月份 
        "d+": this.getDate(), //日 
        "h+": this.getHours(), //小时 
        "m+": this.getMinutes(), //分 
        "s+": this.getSeconds(), //秒 
        "q+": Math.floor((this.getMonth() + 3) / 3), //季度 
        "S": this.getMilliseconds() //毫秒 
    };
    if (/(y+)/.test(fmt)) fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
    for (var k in o)
    if (new RegExp("(" + k + ")").test(fmt)) fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
    return fmt;
}
```

#### 3.2 年/月/日/时/分/秒/季/周
``` bash
// 对Date的扩展，将 Date 转化为指定格式的String * 月(M)、日(d)、12小时(h)、24小时(H)、分(m)、秒(s)、周(E)、季度(q)可以用 1-2 个占位符 
// 年(y)可以用 1-4 个占位符，毫秒(S)只能用 1 个占位符(是 1-3 位的数字) * eg: * (new Date()).pattern("yyyy-MM-dd hh:mm:ss.S")==> 2006-07-02 08:09:04.423      
// (new Date()).pattern("yyyy-MM-dd E HH:mm:ss") ==> 2009-03-10 二 20:09:04      
// (new Date()).pattern("yyyy-MM-dd EE hh:mm:ss") ==> 2009-03-10 周二 08:09:04      
// (new Date()).pattern("yyyy-MM-dd EEE hh:mm:ss") ==> 2009-03-10 星期二 08:09:04      
// (new Date()).pattern("yyyy-M-d h:m:s.S") ==> 2006-7-2 8:9:4.18

Date.prototype.pattern=function(fmt) {         
    var o = {         
    "M+" : this.getMonth()+1, //月份         
    "d+" : this.getDate(), //日         
    "h+" : this.getHours()%12 == 0 ? 12 : this.getHours()%12, //小时         
    "H+" : this.getHours(), //小时         
    "m+" : this.getMinutes(), //分         
    "s+" : this.getSeconds(), //秒         
    "q+" : Math.floor((this.getMonth()+3)/3), //季度         
    "S" : this.getMilliseconds() //毫秒         
    };         
    var week = {         
    "0" : "/u65e5",         
    "1" : "/u4e00",         
    "2" : "/u4e8c",         
    "3" : "/u4e09",         
    "4" : "/u56db",         
    "5" : "/u4e94",         
    "6" : "/u516d"        
    };         
    if(/(y+)/.test(fmt)){         
        fmt=fmt.replace(RegExp.$1, (this.getFullYear()+"").substr(4 - RegExp.$1.length));         
    }         
    if(/(E+)/.test(fmt)){         
        fmt=fmt.replace(RegExp.$1, ((RegExp.$1.length>1) ? (RegExp.$1.length>2 ? "/u661f/u671f" : "/u5468") : "")+week[this.getDay()+""]);         
    }         
    for(var k in o){         
        if(new RegExp("("+ k +")").test(fmt)){         
            fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length)));         
        }         
    }         
    return fmt;         
}       
     
```

#### 3.3 template日期格式化组件
详见[ArtTemplate](/2016/09/20/js-arttemplate/)