---
title: zepto-canvas刮刮卡效果
date: 2018-02-08
comments: true
categories: 抽奖活动
description: 基于canvas的刮奖活动
---

### 前言
前段时间做了个移动端的刮刮卡活动。比较常见的就是支付宝的蚂蚁的刮刮卡，想必大家都不陌生。
现在我在这里略微做个小结，如果能给到大家帮助，我不胜荣幸。
<!-- more -->

### 基础依赖
我做的这个刮刮卡，依赖于zepto.js。当然，大家如果使用jq，也一样可以达到效果，可自行选择


### 效果预览
我们先看下我的项目效果图
![移动端刮刮卡样式](/images/ggl-1.jpg)


### html部分

```
<body class="main_box" style="background-color: #1A1A1A">
    <div class="box">
        <div class="bg_in">
            <div class="title">•&nbsp;刮奖区&nbsp;•</div>
        </div>
        <div class="content">
            <div id="mask_img_bg"><img  src="images/mask_img_bg1.jpg" /></div>
            <img id="redux" src="images/mask-img.jpg" />
        </div>
    </div>
    <div class="show">
        <img class="show_bg" src="images/ok.png">
        <img class="show_bg1" src="images/ok1.png">
        <img class="show_food" src="images/mask_img_bg1.jpg" />
        <a class="close"></a>
        <a class="btn"></a>
    </div>
    <div class="mask"></div>
</body>
```


### CSS部分（大家根据UI自行更改吧，楼楼依旧只是提供一个简单的样式）

```
*{
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
    margin: 0;
    -webkit-touch-callout: none; /* prevent callout to copy image, etc when tap to hold */
    -webkit-text-size-adjust: none; /* prevent webkit from resizing text to fit */
    /* make transparent link selection, adjust last value opacity 0 to 1.0 */
    -webkit-user-select: none; /* prevent copy paste, to allow, change 'none' to 'text' */
    /* -webkit-tap-highlight-color: rgba(0,0,0,0); */
}
html {height: 100%}
a {
    color: #000;
    padding: 0;
    text-decoration: none;
    cursor: pointer;
    font-family: "\5FAE\8F6F\96C5\9ED1", Helvetica, "黑体", Arial, Tahoma
}

.main_box{max-width: 640px; height: 100%; position: relative; margin: 0 auto;font-family: "\5FAE\8F6F\96C5\9ED1", Helvetica, "黑体", Arial, Tahoma}
.main_box .box{height:40%; width: 90%;  position: absolute;left: 5%; top: 5em; background: #ffb8e5; border-radius: 10px;-webkit-border-radius: 10px;}

.main_box .box .bg_in{position: absolute; left: 0.3rem; top: 0.3rem; bottom: 0.3rem; right: 0.3rem; background: #ffe4f3;border-radius: 6px;-webkit-border-radius: 6px;}
.main_box .box .bg_in .title{color: #fd0196;font-size: 0.9rem;text-align: center; padding: 0.25rem 0;}
.main_box .box .content{z-index: 10;position: relative; width: 13rem; height: 6rem;  margin: 2rem auto 0 auto; background:#f30086;border-radius: 6px;-webkit-border-radius: 6px;}
#mask_img_bg{position: absolute; left: 0.2rem; top: 0.2rem; bottom: 0.2rem; right: 0.2rem; background:#fff;border-radius: 6px;-webkit-border-radius: 6px;}
#mask_img_bg img{width:5.3rem; height: 5.5rem; margin: 0 auto;display: block; }
#redux{z-index: 22; position: absolute;padding: 0.2rem; box-sizing: border-box; width:100%; height: 100%;border-radius: 6px;-webkit-border-radius: 6px;}
.main_box .show{display: none; position: absolute;left:50%; top:6rem; width: 286px; height: 245px; margin-left: -143px;z-index: 99;}
.main_box .show .show_bg{width: 286px; height: 245px;}
.main_box .show .show_bg1{width: 239px; height: 138px;position: absolute; top: -20px; left: 50%;margin-left: -120px;z-index: 110;}
.main_box .show .show_food{width:116.5px; height: 121.5px;position: absolute; top: 74px; left: 50%;margin-left: -60px;z-index: 100;}
.main_box .show .close{display: inline-block;width: 32px; height:32px;position: absolute; top: 0; right:0;z-index: 200;}
.main_box .show .btn{display: inline-block;width: 180px; height:37px;position: absolute; bottom: 10px; left: 48px;z-index: 200;}
.main_box .mask{display: none; position: absolute; left: 0; top: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.5);z-index: 98;}
```



### 基础JS部分

```
/*
* Usage:
* $('#myImage').eraser(); // simple way
*
* $(#canvas').eraser( {
*   size: 20, // define brush size (default value is 40)
*   completeRatio: .65, // allows to call function when a erased ratio is reached (between 0 and 1, default is .7 )
*   completeFunction: myFunction // callback function when complete ratio is reached
* } );
*
* $('#image').eraser( 'clear' ); // erases all canvas content 清除画布
*
* $('#image').eraser( 'reset' ); // revert back to original content 重置画布
*
* $('#image').eraser( 'size', 80 ); // change the eraser size  设置橡皮擦大小
*/

(function( $ ){
var methods = {
    init : function( options ) {
        return this.each(function(){
            var $this = $(this),
                data = $this.data('eraser');

            if ( !data ) {
                var width = $this.width(),
                    height = $this.height(),
                    pos = $this.offset(),
                    $canvas = $("<canvas/>"),
                    canvas = $canvas.get(0),
                    size = ( options && options.size )?options.size:40,
                    completeRatio = ( options && options.completeRatio )?options.completeRatio:.7,
                    completeFunction = ( options && options.completeFunction )?options.completeFunction:null,
                    parts = [],
                    colParts = Math.floor( width / size ),
                    numParts = colParts * Math.floor( height / size ),
                    n = numParts,
                    ctx = canvas.getContext("2d");

                // replace target with canvas
                $this.after( $canvas );
                canvas.id = this.id;
                canvas.className = this.className;
                canvas.width = width;
                canvas.height = height;
                ctx.drawImage( this, 0, 0 ,width ,height );
                $this.remove();

                // prepare context for drawing operations
                ctx.globalCompositeOperation = "destination-out";
                ctx.strokeStyle = 'rgba(255,0,0,255)';
                ctx.lineWidth = size;

                ctx.lineCap = "round";
                // bind events
                $canvas.bind('mousedown.eraser', methods.mouseDown);
                $canvas.bind('touchstart.eraser', methods.touchStart);
                $canvas.bind('touchmove.eraser', methods.touchMove);
                $canvas.bind('touchend.eraser', methods.touchEnd);

                // reset parts
                while( n-- ) parts.push(1);

                // store values
                data = {
                    posX:pos.left,
                    posY:pos.top,
                    touchDown: false,
                    touchID:-999,
                    touchX: 0,
                    touchY: 0,
                    ptouchX: 0,
                    ptouchY: 0,
                    canvas: $canvas,
                    ctx: ctx,
                    w:width,
                    h:height,
                    source: this,
                    size: size,
                    parts: parts,
                    colParts: colParts,
                    numParts: numParts,
                    ratio: 0,
                    complete: false,
                    completeRatio: completeRatio,
                    completeFunction: completeFunction
                };
                $canvas.data('eraser', data);

                // listen for resize event to update offset values
                $(window).resize( function() {
                    var pos = $canvas.offset();
                    data.posX = pos.left;
                    data.posY = pos.top;
                });
            }
        });
    },
    touchStart: function( event ) {
        var $this = $(this),
            data = $this.data('eraser');

        if ( !data.touchDown ) {
            var t = event.changedTouches[0],
                tx = t.pageX - data.posX,
                ty = t.pageY - data.posY;
            methods.evaluatePoint( data, tx, ty );
            data.touchDown = true;
            data.touchID = t.identifier;
            data.touchX = tx;
            data.touchY = ty;
            event.preventDefault();
        }
    },
    touchMove: function( event ) {
        var $this = $(this),
            data = $this.data('eraser');

        if ( data.touchDown ) {
            var ta = event.changedTouches,
                n = ta.length;
            while( n-- )
                if ( ta[n].identifier == data.touchID ) {
                    var tx = ta[n].pageX - data.posX,
                        ty = ta[n].pageY - data.posY;
                    methods.evaluatePoint( data, tx, ty );
                    data.ctx.beginPath();
                    data.ctx.moveTo( data.touchX, data.touchY );
                    data.touchX = tx;
                    data.touchY = ty;
                    data.ctx.lineTo( data.touchX, data.touchY );
                    data.ctx.stroke();
                    event.preventDefault();
                    break;
                }
        }
    },
    touchEnd: function( event ) {
        var $this = $(this),
            data = $this.data('eraser');

        if ( data.touchDown ) {
            var ta = event.changedTouches,
                n = ta.length;
            while( n-- )
                if ( ta[n].identifier == data.touchID ) {
                    data.touchDown = false;
                    event.preventDefault();
                    break;
                }
        }
    },
    evaluatePoint: function( data, tx, ty ) {
        var p = Math.floor(tx/data.size) + Math.floor( ty / data.size ) * data.colParts;
        if ( p >= 0 && p < data.numParts ) {
            data.ratio += data.parts[p];
            data.parts[p] = 0;
            if ( !data.complete) {
                if ( data.ratio/data.numParts >= data.completeRatio ) {
                    data.complete = true;
                    if ( data.completeFunction != null ) data.completeFunction();
                }
            }
        }

    },
    mouseDown: function( event ) {
        var $this = $(this),
            data = $this.data('eraser'),
            tx = event.pageX - data.posX,
            ty = event.pageY - data.posY;
        methods.evaluatePoint( data, tx, ty );
        data.touchDown = true;
        data.touchX = tx;
        data.touchY = ty;
        data.ctx.beginPath();
        data.ctx.moveTo( data.touchX-1, data.touchY );
        data.ctx.lineTo( data.touchX, data.touchY );
        data.ctx.stroke();
        $this.bind('mousemove.eraser', methods.mouseMove);
        $(document).bind('mouseup.eraser', data, methods.mouseUp);
        event.preventDefault();
    },
    mouseMove: function( event ) {
        var $this = $(this),
            data = $this.data('eraser'),
            tx = event.pageX - data.posX,
            ty = event.pageY - data.posY;
        methods.evaluatePoint( data, tx, ty );
        data.ctx.beginPath();
        data.ctx.moveTo( data.touchX, data.touchY );
        data.touchX = tx;
        data.touchY = ty;
        data.ctx.lineTo( data.touchX, data.touchY );
        data.ctx.stroke();
        event.preventDefault();
    },
    mouseUp: function( event ) {
        var data = event.data,
            $this = data.canvas;
        data.touchDown = false;
        $this.unbind('mousemove.eraser');
        $(document).unbind('mouseup.eraser');
        event.preventDefault();
    },
    clear: function() {
        var $this = $(this),
            data = $this.data('eraser');
        if ( data )
        {
            data.ctx.clearRect( 0, 0, data.w, data.h );
            var n = data.numParts;
            while( n-- ) data.parts[n] = 0;
            data.ratio = data.numParts;
            data.complete = true;
            if ( data.completeFunction != null ) data.completeFunction();
        }
    },
    size: function(value) {
        var $this = $(this),
            data = $this.data('eraser');
        if ( data && value )
        {
            data.size = value;
            data.ctx.lineWidth = value;
        }
    },
    reset: function() {
        var $this = $(this),
            data = $this.data('eraser');
        if ( data )
        {
            data.ctx.globalCompositeOperation = "source-over";
            data.ctx.drawImage( data.source, 0, 0 );
            data.ctx.globalCompositeOperation = "destination-out";
            var n = data.numParts;
            while( n-- ) data.parts[n] = 1;
            data.ratio = 0;
            data.complete = false;
        }
    }
};
$.fn.eraser = function( method ) {
    if ( methods[method] ) {
        return methods[method].apply( this, Array.prototype.slice.call( arguments, 1 ));
    } else if ( typeof method === 'object' || ! method ) {
        return methods.init.apply( this, arguments );
    } else {
        $.error( 'Method ' +  method + ' does not yet exist on Zepto.eraser' );
    }
};
})( Zepto );

```


### 初始化画布

```
$(window).on('load',function(){
    $('#redux').eraser( {
        size: 20,   //设置橡皮擦大小
        completeRatio: .6, //设置擦除面积比例
        completeFunction: showResetButton   //大于擦除面积比例触发函数
    });
    function showResetButton(){
        $(".main_box .show,.main_box .mask").fadeIn(300)
    }
    $(".main_box .mask,.main_box .show .close,.main_box .show .btn").click(function  () {
        $(".main_box .show,.main_box .mask").fadeOut(300)
    })
})
```

### 小结

写到这里，整个刮刮卡已经完成了。可是小编在开发过程中，发现几个比较坑爹的地方。给大家简单介绍下，
你们可以看看自己有没有这样的类似问题。

坑1: 用过canvas的小伙伴都知道，在使用drawImage的时候，如果图片渲染没有完成，会出现绘图失败情况。
所以小编自己做的时候，是在点击“点我刮奖”按钮之后进行的绘图。

坑2: 关于重绘的reset方法

```
reset: function() {
    var $this = $(this),
        data = $this.data('eraser');
    if ( data )
        {
            data.ctx.globalCompositeOperation = "source-over";
            data.ctx.drawImage( data.source, 0, 0 );    
            data.ctx.globalCompositeOperation = "destination-out";
            var n = data.numParts;
            while( n-- ) data.parts[n] = 1;
            data.ratio = 0;
            data.complete = false;
        }
}
```

大家都知道，如果抽奖完成一次之后，要对canvas画布二次重绘。这个封装的方法中，直接调用reset方法，即可实现。
但是会发现你的默认图片二次重绘会变形，不是你默认图片样式。
是因为这个地方调用了
```
data.ctx.drawImage( data.source, 0, 0 );
```
但事实上，这个方法一共可接收多个，这里传了三个，改成如下，即可实现
```
data.ctx.drawImage( data.source, 0, 0 ,data.w, data.h);
```

顺便普及下:
```
context.drawImage(img,x,y);  //在画布上定位图像

context.drawImage(img,x,y,width,height);  //在画布上定位图像，并规定图像的宽度和高度

context.drawImage(img,sx,sy,swidth,sheight,x,y,width,height);  //剪切图像，并在画布上定位被剪切的部分
```

到这里，关于这个刮奖活动就结束了。如果有任何不对的地方，希望各位不吝指出，谢谢