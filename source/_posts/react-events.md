---
title: react事件
date: 2017-05-22
comments: false
categories: react
description: 这里主要是react的常用事件监听
---


### 【前言】

您的事件处理程序将传递 SyntheticEvent 的实例，这是一个跨浏览器原生事件包装器。 它具有与浏览器原生事件相同的接口，包括 stopPropagation() 和 preventDefault() ，除了事件在所有浏览器中他们工作方式都相同。
如果您发现由于某种原因需要底层浏览器事件，只需使用 nativeEvent 属性来获取它。 每个 SyntheticEvent 对象都具有以下属性

```
boolean bubbles
boolean cancelable
DOMEventTarget currentTarget
boolean defaultPrevented
number eventPhase
boolean isTrusted
DOMEvent nativeEvent
void preventDefault()
boolean isDefaultPrevented()
void stopPropagation()
boolean isPropagationStopped()
DOMEventTarget target
number timeStamp
string type

```
>注意：
>从v0.14起，从事件处理程序返回 false 将不再停止事件传播。 应该根据需要手动 e.stopPropagation() 或 e.preventDefault() 。


### 【事件池】

SyntheticEvent 对象是通过合并得到的。 这意味着在事件回调被调用后，SyntheticEvent 对象将被重用并且所有属性都将被取消。 这是出于性能原因。 因此，您无法以异步方式访问该事件。

```
function onClick(event) {
  console.log(event); // => nullified object.
  console.log(event.type); // => "click"
  const eventType = event.type; // => "click"

  setTimeout(function() {
    console.log(event.type); // => null
    console.log(eventType); // => "click"
  }, 0);

  // 不能工作。 this.state.click 事件只包含空值。
  this.setState({clickEvent: event});

  // 您仍然可以导出事件属性。
  this.setState({eventType: event.type});
}
```

>注意：
>如果要以异步方式访问事件属性，应该对事件调用 event.persist() ，这将从池中删除合成事件，并允许用户代码保留对事件的引用。

#### 剪贴板事件(Clipboard Events)
```
onCopy
onCut
onPaste

//属性:
DOMDataTransfer clipboardData
```

#### 合成事件(Composition Events)
```
onCompositionEnd
onCompositionStart
onCompositionUpdate

//属性
string data
```

#### 键盘事件(Keyboard Events)
```
onKeyDown
onKeyPress
onKeyUp

//属性
boolean altKey
number charCode
boolean ctrlKey
boolean getModifierState(key)
string key
number keyCode
string locale
number location
boolean metaKey
boolean repeat
boolean shiftKey
number which
```

#### 焦点事件(Focus Events)
```
onFocus
onBlur

//属性
DOMEventTarget relatedTarget
```

#### 表单事件(Form Events)
```
onChange
onInput
onSubmit
```

#### 鼠标事件(Mouse Events)
```
onClick onContextMenu onDoubleClick onDrag onDragEnd onDragEnter onDragExit
onDragLeave onDragOver onDragStart onDrop onMouseDown onMouseEnter onMouseLeave
onMouseMove onMouseOut onMouseOver onMouseUp

//属性:
boolean altKey
number button
number buttons
number clientX
number clientY
boolean ctrlKey
boolean getModifierState(key)
boolean metaKey
number pageX
number pageY
DOMEventTarget relatedTarget
number screenX
number screenY
boolean shiftKey
```

onMouseEnter 和 onMouseLeave 事件从离开的元素传播到正在进入的元素，而不是普通的冒泡，并且没有捕获阶段。

#### 选择事件(Selection Events)
```
onSelect
```

#### 触摸事件(Touch Events)
```
onTouchCancel
onTouchEnd
onTouchMove
onTouchStart
```

#### UI事件(UI Events)
```
onScroll

//属性:
number detail
DOMAbstractView view
```

#### 滚轮事件(Wheel Events)
```
onWheel
//属性:
number deltaMode
number deltaX
number deltaY
number deltaZ
```

#### 媒体事件(Media Events)
```
onAbort onCanPlay onCanPlayThrough onDurationChange onEmptied onEncrypted
onEnded onError onLoadedData onLoadedMetadata onLoadStart onPause onPlay
onPlaying onProgress onRateChange onSeeked onSeeking onStalled onSuspend
onTimeUpdate onVolumeChange onWaiting
```

#### 图像事件(Image Events)
```
onLoad
onError
```

#### 动画事件(Animation Events)
```
onAnimationStart
onAnimationEnd
onAnimationIteration
```

#### 转换事件(Transition Events)
```
onTransitionEnd
```

#### 其他事件(Other Events)
```
onToggle
```
