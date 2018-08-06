---
title: 体验Angular2
categories: 
- Javascript
tags:
- Javascript
- Angular2
date: 2015-12-19 22:42:59
keywords: "Angular2,angular"
banner: http://linhsblog-10013469.image.myqcloud.com/images/angular2.jpg
---

前几天，`Angular2`终于发布了beta版本，API基本稳定了，我觉得是时候体验一下了。所以我决定用`Angular2`做一个提示框组件来试试。来看看新的`Angular`有什么优缺点。
<!--more-->
## 安装

根据官方的[5 MIN QUICKSTART](https://angular.io/docs/ts/latest/quickstart.html)教程可以很快地搭建起开发环境。不得不说，`Angular2`的依赖还真多，虽然有些是为了兼容其他浏览器的，但真正的主体部分也不小还需要依赖`rxjs`。我这里使用的是`Typescript`来编写，需要对`Typescript`的编译器配置一下，全都按照官方的来就行。当然也可以选择`ES5`或`ES6`。

使用`npm install`安装完成后再运行`npm start`就会自动编译运行了，有改动的时候会自动刷新页面，很方便。

## 提示框组件

为了简单起见，我直接以提示框组件为根节点启动,并添加一个按钮来显示提示框

`app.component.ts`
```js
import {Component} from 'angular2/core';

@Component({
    selector: 'tip',
    template: `<div class="tip" [ngStyle]="{'display': isShow?'block':'none', 'opacity': opacity}">
        <div class="content">
            <p>{{text}}</p>
        </div>
    </div>
    <button (click)="show()">show</button>`,
    styles: [`.tip {
            position: fixed;
            top: 30px;
            left: 400px;
            background-color: #ccc;
            border-radius: 4px;
            transition: opacity .4s ease;
        }
        .content {

        }
        .content p {
            margin: 10px 6px;
        }`]
})
export class TipComponent {
    public text: String;
    private isShow: Boolean = false;
    private opacity: Number = 0;

    show(text: String = "This is default text") {
        this.text = text;
        this.isShow = true;
        let self = this;
        setTimeout(function() {
            self.opacity = 1;
        }, 10);
        setTimeout(function() {
            self.opacity = 0;
            setTimeout(function() {
                self.isShow = false;
            }, 400);
        }, 2000);
    }
}
```
`boot.ts`
```typescript
import {bootstrap}    from 'angular2/platform/browser'
import {TipComponent} from './app.component'

bootstrap(TipComponent);
```
`index.html`
```html
<html>
<head>
	<title>Angular 2 QuickStart</title>
	<!-- 1. Load libraries -->
	<script src="node_modules/angular2/bundles/angular2-polyfills.js"></script>
	<script src="node_modules/systemjs/dist/system.src.js"></script>
	<script src="node_modules/rxjs/bundles/Rx.js"></script>
	<script src="node_modules/angular2/bundles/angular2.dev.js"></script>
	<!-- 2. Configure SystemJS -->
	<script>
		System.config({
        packages: {
          app: {
            format: 'register',
            defaultExtension: 'js'
          }
        }
      });
      System.import('app/boot')
            .then(null, console.error.bind(console));
	</script>
</head>
<!-- 3. Display the application -->
<body>
	<tip></tip>
</body>
</html>
```

以上就是代码，还是比较简单的，但可能是`Angular2`刚公测，文档还不完善，有些功能不知道该用什么函数。

提示框组件的功能，点击按钮后就弹出一个提示框，传入text变量来改变显示的文本。这个组件的话我是用`Vue`来做的，显示和消失的时候都会有过渡动画，这里我的过渡属性是`opacity`，第一个问题来了，一开始提示框`display: none; opacity: 0`，要显示的时候将`display`改为`block`，`opacity`改为`1`以触发过渡效果，如果两个属性同时修改不会有过渡效果出现，需要用`setTimeout`来延迟`opacity`属性的修改，保证`display`已经被修改为`block`，这样才会有过渡效果。同样的在提示框消失时修改顺序要倒过来，先改`opacity`再改`display`。简而言之就是在为隐藏元素添加过渡效果的时候，需要自己手动处理，`Vue`的话就不需要，通过`v-show`和`transition`属性就能轻松添加过渡效果，不过我认为在正式版本`Angular2`应该会添加类似的功能。

另一个问题就是弹出的提示框需要水平居中，因为提示框的内容是变化的，所以每次显示的时候都需要计算`left`来达到居中效果。在`Vue`中可以通过`this.$el`来获取提示框的DOM，但在`Angular2`中我就不知道该怎么做了，翻了一遍官方文档，暂时没找到相关的api。

除此之外，类似`Vue`的`<slot>`内容分发功能，我在`Angular2`中找到了类似的`<ng-content>`但并不是我想要的效果，而且在文档中也找不到。本来是想作个内滚动组件的，在`Vue`中使用`<slot>`很容易做出内滚动组件。

总得来说，这一次尝试，就现在的`Angular2`版本来说并没有让我感觉强大，期待正式2.0版本能使功能、文档都完善。另一个就是`Angular2`的体积很大，毕竟包含了很多功能，这在网页载入速度上就会有劣势了，所以我觉得就现在的`Angular2`来说，比较适合用来开发大型的，企业级的网页应用，或者直接嵌入软件客户端中。但毕竟没有成熟，能否像`Angular1`那样成功很难说，因为与其他框架对比之下我觉得`Vue`和`React`的方案更优秀。看来短期之内我还是先专注与`Vue`和`React`吧 :)
