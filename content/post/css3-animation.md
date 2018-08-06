---
layout: post
title: CSS3 Animation
date: 2015-11-11 21:22:45
last_modified_at: 
categories: 
- CSS
tags:
- css3
keywords: "css3,animation"
banner: http://linhsblog-10013469.image.myqcloud.com/images/twitter-bird-sprite.png
---

## CSS3 动画

CSS3 动画不需要写一行代码，简单的动画仅需要定义一个`@keyframes`并设置基本的参数就能动了。对比使用`Javascript`编写的动画，CSS3 动画更简单也更高效，使用`sprite`还能做出gif图片的效果。也可以搭配[`SVG`](https://developer.mozilla.org/en-US/docs/Web/SVG)使用。
<!--more-->
动画一般是由多个不同的图片不断快速切换一张图片叫做一帧，在CSS中也一样，只不过不必须使用图片。而是使用`@keyframes`来定义一帧所要显示的“图片”。

### @keyframes

使用`@keyframes`来定义一组名为`slidein`的关键帧序列，使DOM从左向右移动300px

```css
@keyframes slidein {
    from {
        left: 0;
    }
    to {
        left: 300px;
    }
}
/*兼容webkit内核浏览器*/
@-webkit-keyframes slidein {
    from {
        left: 0;
    }
    to {
        left: 300px;
    }
}
/*兼容火狐浏览器*/
@-moz-keyframes slidein {
    from {
        left: 0;
    }
    to {
        left: 300px;
    }
}
```

创建一个button然后，并设置动画参数，然后应用到这个button上

```html
<body>
<button class="slidein">WATCH ME</button>
</body>
```

应用动画到button元素上

```css
.slidein {
  position: absolute;
  animation-duration: 3s;   /*整个动画持续时间*/
  -moz-animation-duration: 3s;
  -webkit-animation-duration: 3s;
  animation-iteration-count: 3; /*动画重复次数*/
  -moz-animation-iteration-count: 3;
  -webkit-animation-iteration-count: 3;
  animation-name: slidein;  /*动画的名称@keyframe定义的slidein*/
  -moz-animation-name: slidein;
  -webkit-animation-name: slidein;
}
```
<style type="text/css">
@keyframes slidein {
    from {
        left: 0;
    }
    to {
        left: 300px;
    }
}
@-webkit-keyframes slidein {
    from {
        left: 0;
    }
    to {
        left: 300px;
    }
}
/*兼容火狐浏览器*/
@-moz-keyframes slidein {
    from {
        left: 0;
    }
    to {
        left: 300px;
    }
}
.slidein {
  position: absolute;
  animation-duration: 3s;   /*整个动画持续时间*/
  -moz-animation-duration: 3s;
  -webkit-animation-duration: 3s;
  animation-iteration-count: 3; /*动画重复次数*/
  -moz-animation-iteration-count: 3;
  -webkit-animation-iteration-count: 3;
  animation-name: slidein;  /*动画的名称@keyframe定义的slidein*/
  -moz-animation-name: slidein;
  -webkit-animation-name: slidein;
}
.infinite.slidein {
    animation-direction: alternate;
    -moz-animation-direction: alternate;
    -webkit-animation-direction: alternate;
    animation-iteration-count: infinite;
    -moz-animation-iteration-count: infinite;
    -webkit-animation-iteration-count: infinite;
}
</style>
<div style="height: 30px; position: relative;"><button class="slidein">WATCH ME</button></div>

这样就完成了一个简单的动画，可以看到按钮从左边往右边移动，重复3次后停止，可以把动画重复次数设置为`infinite`这样动画就会一直播放下去。

还可以设置动画的方向`animation-direction: alternate;`，这样动画还会倒着播放。你可以[动手试试](http://codepen.io/Lin-H/pen/ojQMrM)

<div style="height: 30px; position: relative;"><button class="slidein infinite">WATCH ME</button></div>

### 参考链接

[Using CSS animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations)