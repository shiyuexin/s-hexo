---
title: localStorage
date: 2016-12-15
comments: true
categories: javascript
description: HTML5 storage提供了一种方式让网站能够把信息存储到你本地的计算机上，并再以后需要的时候进行获取。这个概念和cookie相似，区别是它是为了更大容量存储设计的。Cookie的大小是受限的，并且每次你请求一个新的页面的时候cookie都会被发送过去。
---

### 【前言】

>HTML5的storage是存储在你的计算机上，网站在页面加载完毕后可以通过Javascript来获取这些数据。首先自然是检测浏览器是否支持本地存储。在HTML5中，本地存储是一个window的属性，包括localStorage和sessionStorage，从名字应该可以很清楚的辨认二者的区别，前者是一直存在本地的，后者只是伴随着session，窗口一旦关闭就没了。二者用法完全相同，这里以localStorage为例。

### 【用法】

#### 验证浏览器支持
```
if(window.localStorage){
    alert('支持localStorage');
}else{
    alert('不支持localStorage');
}
```
#### 设置/获取/清除
```
<!--设置-->
localStorage.name = "xxx";          //设置name为"xxx"
localStorage["name"] = "xxx";       //设置name为"xxx"，覆盖上面的值
localStorage.setItem("name","xxx"); //设置name为"xxx"

<!--获取-->
localStorage["name"];           //获取name的值
localStorage.name;              //获取name的值
localStorage.getItem("name");   //获取name的值

<!--清除-->
localStorage.removeItem("name");    //清除name的值
localStorage.clear()                //清除所有

<!--key()-->

var localst = window.localStorage
for(var i=0;i<localst.length;i++){
    //key(i)获得相应的键，再用getItem()方法获得对应的值
    document.write(localst.key(i)+ " : " + localst.getItem(localst.key(i)) + "<br>");
}
//这种效果一样不必用key()
for(var skey in localst){
    console.log(skey," : ",localst[skey])
}

```

### 【实例】

>缓存本地历史搜索关键词 localData()

`注意parse/stringify之间的切换`


```
localData(word){
    var localHistory = localStorage.getItem("localData")
    if(!localHistory){
        localHistory = [];
        localHistory.push(word)
    }else{
        localHistory = JSON.parse(localHistory)
        localHistory.push(word)
    }
    localStorage.setItem("localData",JSON.stringify(this.uniqueArray(localHistory)))
    return localHistory;
}

//每次搜索把'xxx'传如localData()生成/增加localData内的数据;
localData('xxx')


```

>历史关键词去重 uniqueArray()

```
uniqueArray(data){
        data = data || [];
        var arr = {};
        data.map((el,index)=>{
            var v = data[index];
            if (typeof(arr[v]) == 'undefined'){
                arr[v] = 1;
            }
        })
        data.length = 0;
        for (var i in arr){
            if(i < 10)               //截取前10个
            data[data.length] = i;
        }
        return data;
    }
```

