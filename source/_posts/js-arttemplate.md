---
title: Arttemplate
date: 2016-09-20
comments: true
categories: javascript
description: js模版引擎,前端不可或缺的模版机制,为什么这么说呢：1、如果你有动态ajax请求数据并需要封装成视图展现给用户，想要提高自己的工作效率；2、如果你是拼串族或者数组push族，迫切的希望改变现有的书写方式。一直拼JS代码多不易维护可读性差；3、如果你在页面布局中，存在共性模块和布局，你可以提取出公共模板，减少维护的数量；4、还可以使用循环\判断等语句, 减少工作量。
---


## Quick Start


### 编写模板
#### 使用一个type="text/html"的script标签存放模板：(或者.tpl文件)

``` bash
<script id="test" type="text/html">
<h1>{{title}}</h1>
<ul>
    {{each list as value i}}
        <li>索引 {{i + 1}} ：{{value}}</li>
    {{/each}}
</ul>
</script>
```

### 渲染模板

``` bash
var data = {
    title: '标签',
    list: ['文艺', '博客', '摄影', '电影', '民谣', '旅行', '吉他']
};
var html = template('test', data);
document.getElementById('content').innerHTML = html;
```

[Demo](http://aui.github.io/artTemplate/demo/basic.html)

### 简洁语法

``` bash
{{if admin}}
    {{include 'admin_content'}}

    {{each list}}
        <div>{{$index}}. {{$value.user}}</div>
    {{/each}}
{{/if}}
```

### 定义扩展函数

``` bash
template.helper(name, callback)
```

### requirejs使用template

``` bash
require.config({
    baseUrl: "../",
    urlArgs: "bust=" + (new Date()).getTime(),
    paths: {
        "art-template":'Js/template'
    }
});
define(['art-template'],function(template){
    var html=template('id',obj);
});
```

### Template日期格式化组件

``` bash
/**
 * 对日期进行格式化，
 * @param date 要格式化的日期
 * @param format 进行格式化的模式字符串
 *     支持的模式字母有：
 *     y:年,
 *     M:年中的月份(1-12),
 *     d:月份中的天(1-31),
 *     h:小时(0-23),
 *     m:分(0-59),
 *     s:秒(0-59),
 *     S:毫秒(0-999),
 *     q:季度(1-4)
 * @return String
 * @author yanis.wang
 * @see	http://yaniswang.com/frontend/2013/02/16/dateformat-performance/
 */
template.helper('dateFormat', function (date, format) {
    if (typeof date === 'string') {
        if (!/[\-|\s]/.test(date)) {
            date = parseInt(date);
        }
    }
    date = new Date(date);

    var map = {
        "M": date.getMonth() + 1, //月份
        "d": date.getDate(), //日
        "h": date.getHours(), //小时
        "m": date.getMinutes(), //分
        "s": date.getSeconds(), //秒
        "q": Math.floor((date.getMonth() + 3) / 3), //季度
        "S": date.getMilliseconds() //毫秒
    };
    format = format.replace(/([yMdhmsqS])+/g, function(all, t){
        var v = map[t];
        if(v !== undefined){
            if(all.length > 1){
                v = '0' + v;
                v = v.substr(v.length-2);
            }
            return v;
        }
        else if(t === 'y'){
            return (date.getFullYear() + '').substr(4 - all.length);
        }
        return all;
    });
    return format;
})
```

More info: [arttemplate](https://github.com/aui/artTemplate)
