---
layout: post
title: Vue source code reading
published: false
---

记录我自己阅读[Vue.js](vuejs.org)源码的一些笔记，函数都有相应的注释，所以主要记录函数的实现部分的技巧和思考

## util

存放一些功能性的函数、helper function

### lang.js

#### debounce

```js
/**
 * 在输入操作停止一定时间后触发相应的事件函数
 * 如连续键盘事件，只触发一次键盘事件
 *
 * @param {Function} func 目标函数
 * @param {Number} wait 延迟时间，毫秒
 * @return {Function} - the debounced function
 */

exports.debounce = function (func, wait) {
  var timeout, args, context, timestamp, result
  var later = function () {
    var last = Date.now() - timestamp
    if (last < wait && last >= 0) {
      timeout = setTimeout(later, wait - last)
    } else {
      timeout = null
      result = func.apply(context, args)
      if (!timeout) context = args = null
    }
  }
  return function () {
    context = this
    args = arguments
    timestamp = Date.now()
    if (!timeout) {
      timeout = setTimeout(later, wait)
    }
    return result
  }
}
```

`debounce`运行后会返回一个函数,可以称为`F`，这个`F`函数每次运行后会记录一个`timestamp`也就是事件触发的时间，并设置内部定义的`later`函数延迟`wait`毫秒后运行，`later`就是关键，`later`每次运行时都会检查**现在**和`timestamp`的间隔时间，这样就能保证总是在最后一次事件触发后的`wait`毫秒运行目标函数。

若大于等于`wait`则运行目标函数，否则继续延迟`wait - last`的时间后再运行`later`，`last`为上次触发事件后已经等待的时间。

现在仍有部分不理解，上述代码中的返回值`result`会一直都为`undefined`，因为`setTimeout`为异步函数，在`result`被赋值前，`result`已经被返回了。

