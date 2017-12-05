---
title: JQ-九宫格抽奖模板
date: 2017-11-30
comments: true
categories: jQuery-插件
description: 关于九宫格抽奖模板
---

### 基础

1.这个模板唯一依赖的是jquery-latest.js

<!-- more -->
### HTML部分

```
<body>
    <div id="lottery">
        <table border="0" cellpadding="0" cellspacing="0">
            <tr>
                <td class="lottery-unit lottery-unit-0"><img src="image/1.jpg"><div class="mask"></div></td>
                <td class="lottery-unit lottery-unit-1"><img src="image/1.jpg"><div class="mask"></div></td>
                <td class="lottery-unit lottery-unit-2"><img src="image/1.jpg"><div class="mask"></div></td>
            </tr>
            <tr>
                <td class="lottery-unit lottery-unit-7"><img src="image/1.jpg"><div class="mask"></div></td>
                <td><a href="#"></a></td>
                <td class="lottery-unit lottery-unit-3"><img src="image/1.jpg"><div class="mask"></div></td>
            </tr>
            <tr>
                <td class="lottery-unit lottery-unit-6"><img src="image/1.jpg"><div class="mask"></div></td>
                <td class="lottery-unit lottery-unit-5"><img src="image/1.jpg"><div class="mask"></div></td>
                <td class="lottery-unit lottery-unit-4"><img src="image/1.jpg"><div class="mask"></div></td>
            </tr>
        </table>
    </div>
</body>
```

### CSS部分（大家根据UI自行更改吧，楼楼只是提供一个简单的样式）

```
#lottery{width:570px;height:510px;margin:0px auto;border:4px solid #ba1809;}
#lottery table{background-color:ba1809;}
#lottery table td{position:relative;width:190px;height:170px;text-align:center;color:#333;font-index:-999}
#lottery table td img{display:block;width:190px;height:170px;}
#lottery table td a{width:190px;height:170px;display:block;text-decoration:none;background:url(image/lottery1.jpg) no-repeat top center;}
#lottery table td a:hover{background-image:url(image/lottery1.jpg);}
#lottery table td.active .mask{display:block;}
.mask{
    width:100%;
    height:100%;
    position:absolute;
    left:0;
    top:0;
    background-color: rgba(252,211,4,0.5);
    display:none;
}
```

### 基础JS部分

```
var lottery={
    index:-1,    //当前转动到哪个位置，起点位置
    count:0,    //总共有多少个位置
    timer:0,    //setTimeout的ID，用clearTimeout清除
    speed:20,    //初始转动速度
    times:0,    //转动次数
    cycle:50,    //转动基本次数：即至少需要转动多少次再进入抽奖环节
    prize:-1,    //中奖位置
    init:function(id){
        if ($("#"+id).find(".lottery-unit").length>0) {
            $lottery = $("#"+id);
            $units = $lottery.find(".lottery-unit");
            this.obj = $lottery;
            this.count = $units.length;
            $lottery.find(".lottery-unit-"+this.index).addClass("active");
        };
    },
    roll:function(){
        var index = this.index;
        var count = this.count;
        var lottery = this.obj;
        $(lottery).find(".lottery-unit-"+index).removeClass("active");
        index += 1;
        if (index>count-1) {
            index = 0;
        };
        $(lottery).find(".lottery-unit-"+index).addClass("active");
        this.index=index;
        return false;
    },
    stop:function(index){
        this.prize=index;
        return false;
    }
};

function roll(){
    lottery.times += 1;
    lottery.roll();//转动过程调用的是lottery的roll方法，这里是第一次调用初始化
    if (lottery.times > lottery.cycle+10 && lottery.prize==lottery.index) {
        clearTimeout(lottery.timer);
        lottery.prize=-1;
        lottery.times=0;
        click=false;
    }else{
        if (lottery.times<lottery.cycle) {
            lottery.speed -= 10;
        }else if(lottery.times==lottery.cycle) {
            var index = Math.random()*(lottery.count)|0;  //中奖物品通过一个随机数生成
            lottery.prize = index;
        }else{
            if (lottery.times > lottery.cycle+10 && ((lottery.prize==0 && lottery.index==7) || lottery.prize==lottery.index+1)) {
                lottery.speed += 110;
            }else{
                lottery.speed += 20;
            }
        }
        if (lottery.speed<40) {
            lottery.speed=40;
        };
        lottery.timer = setTimeout(roll,lottery.speed);//循环调用
    }
    return false;
}

var click=false;

window.onload=function(){
    lottery.init('lottery');
    $("#lottery a").click(function(){
        if (click) {//click控制一次抽奖过程中不能重复点击抽奖按钮，后面的点击不响应
            return false;
        }else{
            lottery.speed=100;
            roll();    //转圈过程不响应click事件，会将click置为false
            click=true; //一次抽奖完成后，设置click为true，可继续抽奖
            return false;
        }
    });
};

```


### 效果图

其实做到这里，项目已经可以直接运行了。关于初始化的转动圈数和转动速度，都可以根据自己的
需求来优化。这个地方就不详细描述了。
我在自己使用过程中发现，我们一般调用接口后取到的结果来开始转动动画，但是不能很好的控制结束。
所以对原有的代码进行了优化
roll方法中，接收两个参数


### 优化后的JS

```
参数1: pos - 转动要停止的位置
参数2：cb - 回调方法 光标转动停止后 要执行的方法
function roll(pos,cb){
    lottery.times += 1;
    lottery.roll();//转动过程调用的是lottery的roll方法，这里是第一次调用初始化
    if (lottery.times > lottery.cycle+5 && lottery.prize==lottery.index) {
        clearTimeout(lottery.timer);
        lottery.prize=-1;
        lottery.times=0;
        setTimeout(cb,500);   // 转动停止后 500毫秒后执行回调函数
        $(".start-button-move").removeClass('move');
        click = false;
    }else{
         if (lottery.times<lottery.cycle) {
                    lottery.speed -= 10;
            }else if(lottery.times==lottery.cycle) {
                var index = Math.random()*(lottery.count)|0;  //中奖物品通过一个随机数生成
                lottery.prize = index;
            }else{
                if (lottery.times > lottery.cycle+10 && ((lottery.prize==0 && lottery.index==7) || lottery.prize==lottery.index+1)) {
                    lottery.speed += 110;
                }else{
                    lottery.speed += 20;
                }
            }
            if (lottery.speed<40) {
                lottery.speed=40;
            };
        lottery.timer = setTimeout(function(){roll(pos,cb)},lottery.speed);//循环调用
    }
    return false;
}

```

### 调用方法

方法调用时候就变成
    roll(X,function(){});
    X是停止的光标位置 （0--8）
    第二个参数是回调函数

###后续
楼楼亲测 在抽奖过程中，快速转动的效果，会因为页面的渲染，在安卓手机上出现闪现等bug
建议小伙伴们在设置初始化转动速度和圈数的时候 适时调整下。这个项目就完整啦、

铛铛铛~~ending


