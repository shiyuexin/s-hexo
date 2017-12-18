---
title: v-distpicker省市区三级联动
date: 2017-12-18
comments: true
categories: vue
description: vue插件
---

### v-distpicker

V - Distpicker 是一个简单、易用、灵活的 Vue 组件，且兼容 PC端、移动端的省市区三级联动的插件。
由于楼楼最近使用 vue比较多，加上项目需求，就发现了这个好用的插件，总结下推荐给大家。


<!-- more -->

### 效果预览

![pc样式](/images/3.png)

![手机样式](/images/1.jpg)

![手机样式](/images/2.jpg)

### 注册组件

注册全局组件
```
import VDistpicker from 'v-distpicker'

Vue.component('v-distpicker', VDistpicker);
```

注册组件
```
import VDistpicker from 'v-distpicker'

export default {
  components: { VDistpicker }
}
```

### 组件使用

基础用法
```
<v-distpicker></v-distpicker>
```

默认值
```
<v-distpicker province="广东省" city="广州市" area="海珠区"></v-distpicker>
```

移动端
```
<v-distpicker type="mobile"></v-distpicker>
```

### 数据双向绑定

```
 <v-distpicker :province="province" :city="city" :area="area" @province="selectProvince" @city="selectCity" @area="selectArea"></v-distpicker>

  selectProvince(value) {
      console.log(value);
  },
  selectCity(value) {
      console.log(value);
  },
  selectArea(value) {
      console.log(value);
  }
```


#尾声

用起来还是很容易的，不需要花时间去开发。推荐给大家！

参考地址:https://distpicker.iline.co/




