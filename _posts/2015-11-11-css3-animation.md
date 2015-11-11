---
layout: post
title: CSS3 Animation
category: CSS
keywords: "css3,animation"
excerpt: <p>CSS3 动画不需要写一行代码，简单的动画仅需要定义一个<code class="highlighter-rouge">@keyframes</code>并设置基本的参数就能动了。对比使用<code class="highlighter-rouge">Javascript</code>编写的动画，CSS3 动画更简单也更高效，使用<code class="highlighter-rouge">sprite</code>还能做出gif图片的效果。也可以搭配<a href="https://developer.mozilla.org/en-US/docs/Web/SVG"><code class="highlighter-rouge">SVG</code></a>使用。</p>
---

## CSS3 动画

CSS3 动画不需要写一行代码，简单的动画仅需要定义一个`@keyframes`并设置基本的参数就能动了。对比使用`Javascript`编写的动画，CSS3 动画更简单也更高效，使用`sprite`还能做出gif图片的效果。也可以搭配[`SVG`](https://developer.mozilla.org/en-US/docs/Web/SVG)使用。

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
  animation-iteration-count: 3; /*动画重复次数*/
  animation-name: slidein;  /*动画的名称@keyframe定义的slidein*/
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
.slidein {
  position: absolute;
  animation-duration: 3s;   /*整个动画持续时间*/
  animation-iteration-count: 3; /*动画重复次数*/
  animation-name: slidein;  /*动画的名称@keyframe定义的slidein*/
}
.infinite.slidein {
    animation-direction: alternate;
    animation-iteration-count: infinite;
}
</style>
<div style="height: 30px; position: relative;"><button class="slidein">WATCH ME</button></div>

这样就完成了一个简单的动画，可以看到按钮从左边往右边移动，重复3次后停止，可以把动画重复次数设置为`infinite`这样动画就会一直播放下去。

还可以设置动画的方向`animation-direction: alternate;`，这样动画还会倒着播放。你可以[动手试试](http://codepen.io/Lin-H/pen/ojQMrM)

<div style="height: 30px; position: relative;"><button class="slidein infinite">WATCH ME</button></div>

### 参考链接

[Using CSS animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations)