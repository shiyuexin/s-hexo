---
title: form to json
date: 2016-12-20
comments: true
categories: javascript
description: 当开发中需要拿到当前表单数据，又不能提前保存数据的情况下，就需要讲整个form表单序列化，再进行转译，简单说就是将form表单转化成json数据。
---

### 【用法】

```
$('#form').serialize();        //会根据input里面的name，把数据序列化成字符串；eg：name=xxx
$('#form').serializeArray();    //会根据input里面的name，把数据序列化成数组；eg：[object]
    
var jsObj = this.formToJson(decodeURIComponent($('#form').serialize(),true));

formToJson: function (data) {
   data=data.replace(/&/g,"\",\"");
   data=data.replace(/=/g,"\":\"");
   data="{\""+data+"\"}";
   return data;
}
    
var str = JSON.stringify(jsObj);  //json对象转化为json字符串
var str1 = JSON.parse(str);  //json字符串转化为json对象
```

