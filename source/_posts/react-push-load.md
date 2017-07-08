---
title: react下拉加载(移动端)
date: 2017-01-12
comments: true
categories: react
description: react下拉加载扩展并不是很多，reactjs-iscroll还是不错的，考虑到我这里对上拉刷新需求不大，就手动写了个下拉加载
---

### 【前言】
>其实这里主要原理是:js对可视高度获取的应用及什么方法触发。


### 【react下拉加载】

#### 移动端touchEnd
>这里用的react的onTouchEnd()方法，只适用于移动端，至于为什么不选scroll()，touchEnd是在触摸结束执行，比scroll在移动端更适合，能解决很多不必要的麻烦，当然调试时可先用scroll方法代替。

```

constructor(props) {
    super(props, "xxx");
    this.state = {
        allFoods: [],
        pageNumber: 1,
        pageTotal: '',
        loadStatus: false,
        pageStatus: true
    };
    this.pending = false;
    this.pageLoading = false;
}
    
handleTouchEnd(){
    const scrollTop =  document.body.scrollTop;
    const clientHeight = document.body.clientHeight;
    const windowHeight = window.screen.height;
    const offsetBottom = clientHeight - scrollTop - windowHeight +64;
    if(this.pageLoading) {
        this.pageLoading = false;
        return;
    }
    if(offsetBottom <= -64){
        if(this.state.pageNumber < this.state.pageTotal/10){
            if(this.pageLoading) return;
            this.pageLoading = true;
            //加载中状态
            this.setState({
                loadStatus: true
            })
            this.state.pageNumber ++;
            this.categorySearch();
        }else {
            //是否有分页
            this.setState({
                pageStatus: false
            })
        }
    }
}

categorySearch() {
    this.pending = false;
    if (this.pending) return;
    this.pending = true;
    this.ajax({
        url: API.GET_CATEGORY,
        data: {
            page: this.state.pageNumber
        },
        success: (res) => {
            this.setState({
                allFoods: this.state.allFoods.concat(res),
            });
        },
        error: (res) => {
            this.pending = false;
        }
    })
}
```

#### PC端scroll

>pc端如果用scroll滚动事件，或者移动端调试时，可以这样

```
componentWillUnmount() {
    //生命周期结束时销毁事件
    window.removeEventListener('scroll', this.handleScroll.bind(this));
}
componentDidMount() {
    //组件渲染完成注册事件
    window.addEventListener('scroll', this.handleScroll.bind(this));
}

handleScroll() {
    ......
}
```