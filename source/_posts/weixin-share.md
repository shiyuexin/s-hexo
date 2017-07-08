---
title: 微信分享js自定义配置
date: 2017-04-10
comments: true
categories: 微信
description: 微信分享到朋友／朋友圈自定义title，url，
---

### 基本步骤

1.引入JS文件，在需要调用JS接口的页面引入JS文件:
（支持https）：http://res.wx.qq.com/open/js/jweixin-1.2.0.js
2.通过config接口注入权限验证配置，所有需要使用JS-SDK的页面必须先注入配置信息，否则将无法调用（同一个url仅需调用一次，对于变化url的SPA的web app可在每次url变化时进行调用）；
3.通过ready接口处理成功验证。

### config配置

```
wx.config({
    debug: true,  // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId: '',    // 必填，公众号的唯一标识
    timestamp: ,  // 必填，生成签名的时间戳
    nonceStr: '', // 必填，生成签名的随机串
    signature: '',// 必填，签名，见附录1
    jsApiList: [] // 必填，需要使用的微信JS接口列表
});
```

### 封装工具类

```
var weixin = {
    share: function (option) {
        this.option = option;
        Promise.all([this.loadSign(), this.loadFile()]).then(() => {
            this.use();
        });
    },
    loadSign: function() {
        var root;
        var url = root + '/userApi/weixinJsapi';
        return new Promise((resolve, reject) => {
            request.jsonp({
                url: url,
                data: {
                    url: location.href.split('#')[0]
                },
                callbackKey: 'jsonp_callback'
            }).then((res) => {
                this.token = res.data;
                resolve();
            });
        });
    },
    loadFile: function() {
        if (window.wx) return window.noopPromise;
        var script=document.createElement('script');
        script.setAttribute('src', 'http://res.wx.qq.com/open/js/jweixin-1.2.0.js');
        document.body.appendChild(script);
        return new Promise((resolve, reject) => {
            resolve();
        })
    },
    use: function() {
        var wx = window.wx;
        var config = Object.assign({}, { debug: false, jsApiList: [
            'onMenuShareTimeline',
            'onMenuShareAppMessage',
            'onMenuShareQQ',
            'onMenuShareWeibo',
            'onMenuShareQZone'] }, this.token);
        wx.config(config);
        var option = this.option;
        wx.ready(function() {
            wx.onMenuShareTimeline(option);
            wx.onMenuShareAppMessage(option);
            wx.onMenuShareQQ(option);
            wx.onMenuShareWeibo(option);
            wx.onMenuShareQZone(option);                
        })
        
        //调试微信接口
        wx.error(function(res) {
            window.alert('res');
        });
    }
}


```

### 应用工具类

```
var option = {
    title: '标题',
    desc: '描述文案' || '默认描述文案~\n换行',
    imgUrl: 'http://blog.xonepage.com/uploads/portrait_1.jpg',
    link: window.location.href,
    success: function() {
        window.toast('分享成功');
    },
    cancel: function() {
        window.toast('分享取消');
    }
};
weixin.share(option)

```