---
title: ant-design
date: 2016-11-30
comments: true
categories: react
description: ANT DESIGN 据官方说法：一套企业级的前端设计语言和基于 React 的实现。是蚂蚁金服体验技术部出品的一个设计&前端框架，基于 React 框架。目前官网介绍的包括 CSS 和 Components 两大部分。 CSS 部分涵盖了基本的 Layout，Iconfont，Button，Motion。这里主要谈谈antd布局和组件机制
---

### 【特点】
1. 丰富实用的 React UI 组件。
2. 基于 React 的组件化开发模式。
3. 背靠 npm 生态圈。
4. 基于 webpack 的调试构建方案，支持 ES6。

### 【前言】

>应用antd要对nodejs npm模式有一定了解，需先安装[nodejs >>](http://nodejs.cn/)

### 【安装】
```
$ npm install antd
```
#### 示例
```
import { DatePicker } from 'antd';
ReactDOM.render(<DatePicker />, mountNode);
```
#### 引入样式
```
import 'antd/dist/antd.css'; 
```

>如果整站所用组件均为antd，可在全局app.js引入；如局部引用，可在相应js文件头单独引入

### 布局layout
```
import React from 'react'
import { Row, Col} from 'antd'
import Page from '../../../base/Page'
export default class PageLayout extends Page {
    constructor(props) {
        super(props)
    }

    render() {
        return (
            <div>
                <h1>ant布局</h1>
                <h2>间距</h2>
                <Row>
                    <Col span={8}>col-8</Col>
                    <Col span={8} offset={8}>col-8</Col>
                </Row>
                <Row>
                    <Col span={6} offset={6}>col-6 col-offset-6</Col>
                    <Col span={6} offset={6}>col-6 col-offset-6</Col>
                </Row>
                <Row>
                    <Col span={12} offset={6}>col-12 col-offset-6</Col>
                </Row>

                <h2>flex盒模型</h2>
                <p>sub-element align left</p>
                <Row type="flex" justify="start">
                    <Col span={4}>col-4</Col>
                    <Col span={4}>col-4</Col>
                    <Col span={4}>col-4</Col>
                    <Col span={4}>col-4</Col>
                </Row>
                ...
```

>antd采用css3 flex弹性布局，需要注意的是设为Flex布局以后，子元素的float、clear和vertical-align属性将失效。Flex布局基于flex-flow流，而常规布局是基于块和内联流方向，Flex布局使我们建立css布局更加随心所欲、简洁优雅。当然美好的东西就会有该有的缺憾，flex的兼容性如下图所示：

![img](http://ohgbisrm2.bkt.clouddn.com/css_flex_min.png)

>有缺憾就有弥补：将Flexbox多版本混合在一起使用

```
.page-text {
  display: -webkit-box;      /* OLD - iOS 6-, Safari 3.1-6 */
  display: -moz-box;         /* OLD - Firefox 19- (buggy but mostly works) */
  display: -ms-flexbox;      /* TWEENER - IE 10 */
  display: -webkit-flex;     /* NEW - Chrome */
  display: flex;             /* NEW, Spec - Opera 12.1, Firefox 20+ */
 }
 ...
```
如果你将Flexbox多版本混合在一起使用，可以得到以下浏览器的支持：
`Chrome any`
`Firefox any`
 `Safari any`
 `Opera 12.1+`
 `IE 10+`
 `iOS any`
 `Android any`
 虽然ie依然废，只好放弃
 
 [社区贡献脚手架 >>](https://github.com/ant-design)
 [本人贡献脚手架 >>](https://git.oschina.net/xkh/react-antd-redux.git)

