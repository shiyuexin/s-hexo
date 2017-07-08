---
title: javascript面向对象开发【待续】
date: 2017-01-13
comments: true
categories: javascript
description: javascript面向对象开发，对初学者来说确实不太容易理解，这里是之前在阮老师那里学来的笔记，Javascript的Object模型，分享借鉴
---

### 【对象&实例】

#### 封装数据/方法
```
//构造函数
function Me (name, age) {
	//本地属性
	this.name = name;
	this.age = age;
	// this.height = 173;
	// this.hobby = function(){
	// 	console.log('hobby')
	// }
}

//继承自prototype的属性
Me.prototype.height = 173
Me.prototype.hobby = function(){
	console.log('hobby')
}
```
#### 创建实例 
```
//实例化两个对象
var m1 = new Me('xkh','26')
var m2 = new Me('xking','25')

//遍历某个对象的属性
for(var prop in m1) { console.log("m1["+prop+"]="+m1[prop]); }

m1.hobby()
m2.hobby()

console.log(m1,m2)
console.log('两个是不是在一个储存:',m1.hobby == m2.hobby)    //true 如果hobby是本地属性这里就是false
console.log('对象和实例之间的关系:',Me.prototype.isPrototypeOf(m1))   //true
console.log('是本地属性还是继承自prototype的属性:',m1.hasOwnProperty(name))  //false
console.log('in运算符判断实例中是否含有某属性:',('name' in m1))    //true
```

### 【构造函数的继承】

现在有两个构造函数
```
function Me1(){
    this.name = 'xkh'
}

function Me2(age, height){
    Me1.apply(this, arguments);
    this.age = age,
    this.height = height
}

var m3 = new Me2();
console.log(m3.name)
```
