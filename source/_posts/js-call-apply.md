---
title: Cell&Apply
date: 2017-07-04
comments: true
categories: javascript
description: 
---


## Quick Start

`cell`:调用一个对象的一个方法，以另一个对象替换当前对象。 
call([thisObj[,arg1[, arg2[, [,.argN]]]]]) 
`参数`:thisObj 可选项。将被用作当前对象的对象。 
arg1, arg2, , argN 可选项。将被传递方法参数序列。 
`说明`:call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。 
> 如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。 
> 是不是迷了，我们直接看例子解释


```
    function add(a,b) { 
        alert(a+b); 
    } 
    function sub(a,b) { 
        alert(a-b); 
    } 
    add.call(sub,3,1); 
```


这个例子中的意思就是用 add 来替换 sub，add.call(sub,3,1) == add(3,1) ，所以运行结果为：alert(4); // 注意：js 中的函数其实是对象，函数名是对 Function 对象的引用。 

看一个稍微复杂一点的例子 

function Class1() 
{ 
    this.name = "class1"; 

    this.showNam = function() 
    { 
        alert(this.name); 
    } 
} 

function Class2() 
{ 
    this.name = "class2"; 
} 

var c1 = new Class1(); 
var c2 = new Class2(); 

c1.showNam.call(c2); 

注意，call 的意思是把 c1 的方法放到c2上执行，原来c2是没有showNam() 方法，现在是把c1 的showNam()方法放到 c2 上来执行，所以this.name 应该是 class2，执行的结果就是 ：alert（"class2"）; 

怎么样，觉得有意思了吧，可以让a对象来执行b对象的方法，这是java程序员所不敢想的。还有更有趣的，可以用 call 来实现继承 

function Class1() 
{ 
    this.showTxt = function(txt) 
    { 
        alert(txt); 
    } 
} 

function Class2() 
{ 
    Class1.call(this); 
} 

var c2 = new Class2(); 

c2.showTxt("cc"); 

这样 Class2 就继承Class1了，Class1.call(this) 的 意思就是使用 Class1 对象代替this对象，那么 Class2 中不就有Class1 的所有属性和方法了吗，c2 对象就能够直接调用Class1 的方法以及属性了，执行结果就是：alert（“cc”）; 

对的，就是这样，这就是 javaScript 如何来模拟面向对象中的继承的，还可以实现多重继承。 

function Class10() 
{ 
    this.showSub = function(a,b) 
    { 
        alert(a-b); 
    } 
} 

function Class11() 
{ 
    this.showAdd = function(a,b) 
    { 
        alert(a+b); 
    } 
} 


function Class2() 
{ 
    Class10.call(this); 
    Class11.call(this); 
} 

很简单，使用两个 call 就实现多重继承了 
当然，js的继承还有其他方法，例如使用原型链，这个不属于本文的范畴，只是在此说明call 的用法 
说了call ，当然还有 apply，这两个方法基本上是一个意思 
区别在于 call 的第二个参数可以是任意类型，而apply的第二个参数必须是数组


More info: [arttemplate](https://github.com/aui/artTemplate)
