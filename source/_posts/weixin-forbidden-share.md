---
title: 微信隐藏分享等操作按钮
date: 2017-12-21
comments: true
categories: 微信
description: 微信分享隐藏“传播类”和“保护类”按钮配置
---

### 基本步骤

1.引入JS文件，在需要调用JS接口的页面引入JS文件:
（支持https）：http://res.wx.qq.com/open/js/jweixin-1.2.0.js
2.通过config接口注入权限验证配置，所有需要使用JS-SDK的页面必须先注入配置信息，否则将无法调用（同一个url仅需调用一次，对于变化url的SPA的web app可在每次url变化时进行调用）；
3.通过ready接口处理成功验证。

<!-- more -->

### config配置

```
wx.config({
    debug: true,  // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId: '',    // 必填，公众号的唯一标识
    timestamp: ,  // 必填，生成签名的时间戳
    nonceStr: '', // 必填，生成签名的随机串
    signature: '',// 必填，签名，见附录1
    jsApiList: ["hideMenuItems"] // 必填，隐藏按钮(其他功能可以自行查看微信文档)
});
```


### 隐藏禁止配置

```
wx.ready(function () {
     wx.hideMenuItems({
        menuList: ['menuItem:share:timeline',     //分享到朋友圈
                    'menuItem:share:appMessage',    //发送给朋友
                    'menuItem:share:qq',            //分享到QQ
                    'menuItem:share:weiboApp',      //分享到Weibo
                    'menuItem:share:facebook',      //分享到FB
                    'menuItem:share:QZone',         //分享到 QQ 空间
                    'menuItem:copyUrl',             //复制链接
                    'menuItem:openWithQQBrowser',   //在QQ浏览器中打开
                    'menuItem:openWithSafari'       //在Safari中打开
                ], // 要隐藏的菜单项，只能隐藏“传播类”和“保护类”按钮
        });
})

```

### 尾声

最后需要声明一点,这类做法不是移除该功能，只是将这些操作按钮统统隐藏掉。
隐藏的菜单项，也只限于隐藏“传播类”和“保护类”按钮。切记！

附上微信公众平台微信js-jdk说明文档地址:
https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115

