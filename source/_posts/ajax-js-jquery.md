---
title: 原生和jQuery的Ajax用法
date: 2017-04-11
comments: true
categories: ajax
description: javascript和jquery的ajax用法区分
---

### 【jquery-ajax】

```
$.ajax({
    url: 'api/xxxx.php',
    type: 'POST', //GET
    async: true, //是否异步
    data: {
        text: 'aa',
        value: 'bb'
    },
    timeout: 5000, //超时时间
    dataType: 'json' //返回数据格式 (json/xml/html/script/jsonp/text)
    beforeSend: function(xhr){
        console.log('发送前:',xhr)
    },
    success: function(data,textStatus,jqXHR){},
    error: function(xhr,textStatus){},
    complete: function(){
        console.log('完成')
    }
})

```

### 【javascript-ajax】

>请求的5个阶段，对应readyState的值
>0: 未初始化，send方法未调用；
>1: 正在发送请求，send方法已调用；
>2: 请求发送完毕，send方法执行完毕；
>3: 正在解析响应内容；
>4: 响应内容解析完毕；

```
var data = {
    text: 'aa',
    value: 'bb'
}

var xhr = new XMLHttpRequest(); //创建一个ajax对象
    //对ajax对象监听
    xhr.onreadystatechange = function(event){
        //4解析完成
        if(xhr.readyState == 4){
            //200正常返回
            if(xhr == 200){
                console.log('xhr:',xhr)
            }
        }
    }
    //建立链接: {1:发送方式,2:请求地址,3:是否异步}
    xhr.open('POST','url',true);
    xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded')  //可有可无
    xhr.send(data)  //发送

```

{% note primary %} end {% endnote %}