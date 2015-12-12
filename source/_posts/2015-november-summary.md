title: 广州11月份实习
categories: Others
tag:
  - life
date: 2015-12-12 23:16:37
keywords: "广州,实习,Vue,Electron"
banner: http://linhsblog-10013469.image.myqcloud.com/images/november.png
---


2015年11月份，仍在广州实习。9月份时候的实习做的是Node.js开发，现在是Javascript工程师，也就是前端了。工作的内容也不难，但接触到的东西还是很有收获的。了解到公司的内部结构，使用的技术，真实的产品。尤其是使用浏览器内核来开发软件，解决了我好几年前关于软件开发的疑惑。还有就是MVVM框架Vue.js的使用，这个是主要内容了。
<!-- more -->
第一天去的时候，主要是学习内部的公用库，sass和js。关于css预处理器的选择有`Sass`，`Less`，`Stylus`，其实本质上都差不多基本就是学会了一个就能使用这3种，有的是细微差别，如果你不喜欢写分号，喜欢Python那种缩进形式的我推荐`Stylus`。至于我自己的话就是比较倾向于`Sass`了，以前都是使用Ruby来进行编译，但现在随着`Node.js`的崛起，也有了`node-sass`，就连我现在的这个博客也从`Jekyll`迁移到了`Hexo`上，为的是统一工具链 :)

js部分的话使用的还是传统的`jQuery`和事件监听(请原谅我说jQuery很传统)，开发流程都是集中在给DOM绑定事件，触发事件后修改DOM，当时读代码的时候确实头晕，往往需要结合已有的项目去读，不然不了解HTML的结构不知道某些函数操作是干嘛的，项目一旦复杂起来维护就比较麻烦了。

而且jQuery的很多常用函数都能使用原生js来实现，`addEventListener`,`removeEventListener`,'document.querySelector'等等。所以我们前端就开始了迁移工作，使用`webpack`和`Vue.js`来改写。

`webpack`是个很棒的打包工具，也自带有一些插件，可以完成代码压缩，公共库打包，css文件单独抽取等操作，但类似于sprite图片的合并就需要`gulp`来构建了，更何况`gulp`有可以调用`webpack`的插件。除此之外，还使用ES6来开发，通过`Babel`来编译，以`Vue`组件的形式来编写代码css、HTML、Javascript都写在同一个文件中，通过`vue-loader`来打包，所以整个的工具链是`Vue` -> `Babel` -> `webpack` -> `gulp`。

唯一让我不习惯的就是,使用的版本控制系统是`SVN`，感觉真是不好用，还是`Git`大法好。好像使用的原因是因为`SVN`比较容易部署，装个软件就行了，`SVN`上传代码慢，分支功能差，合并冲突的功能也不好，谁手快，谁就不用解决冲突。真的感觉`Git`甩`SVN`几条街。

## [Vue](http://vuejs.org/)

![](components.png)

至于`Vue`是什么我就不多说了，轻量的MVVM框架。为什么我们选用了`Vue`而没有选择`Angular.js`，`Backbone`，`Ember`之类的？最大原因是因为简单，容易学会。相比与`Angular.js`来说`Vue`很轻量，它并没有包含特别多的功能，当你需要其他功能如ajax的时候，通过插件的形式加载进去。而且比`Angular.js`更容易上手，说实话，我当初开始接触`Angular.js`的时候真的不觉得容易。而且`Angular.js`与`Vue`最大的不同是`Vue`更容易组件化，代码量也少。整个页面是就是一棵组件组成的树。

```js
//创建并注册一个自定义组件
Vue.component('my-component', {
  template: '<div>Hello {{name}}, This is a custom {{comp}}!</div>',
  data: function() {//内部的变量，可以绑定到模板上
    return {
      comp: 'component'
    }
  },
  methods: {
    a: function(){},
    b: function(){}
  },
  props: {//从外部传进来的属性
    name: {
        type: String,
        default: 'Lin-H'
    }
  }
});

//创建一个根节点来挂在组件
new Vue({
    el: 'body',//根节点的作用范围是整个body标签
});
```
```html
<body>
<my-component><my-component/>
</body>
<!-- 会被渲染成 -->
<body>
<div>Hello Lin-H, This is a custom component!</div>
</body>
```

所以我平时的工作主要的就是将一些用的比较多的功能组件化，比如表格组件，页面内滚动组件，弹出框组件等。总的来说`Vue`可以满足很多需求，并不会因为自身小，造成功能弱。

我个人认为`Vue`是要比`Angular.js`要优秀的，不过`Angular2`还是值得期待的。

## Software

很多年前，我一直有个疑问，像`QQ`客户端这种软件是如何编写的，聊天对话框，用户界面都做得很好看，难道都是通过`C#`来写的吗？但是当我接触`C#`，用它来写UI的时候，感觉也并不是那么容易，那么得心应手。直到我来实习才发现，这些软件是运行在浏览器上的，准确来说是软件中嵌入了一个浏览器内核，然后使用网页的技术来编写客户端软件，不仅简单，而且通过CSS3也能做出足够美观的界面。

有个例子是`Microsoft`的开源编辑器[Visua Studio Code](https://code.visualstudio.com/)，就是使用了这种技术，使用的是[Electron](electron.atom.io)，`chromium`内核。
![](vscode.png)
在用户看来，这就是一款软件，并不知道这其实是一个网页。如果去掉内核自带的边框，就只能看到网页中`body`标签内的部分，标题栏的最小化，关闭按钮都会隐藏，然后自己使用HTML在顶部做出一个“标题栏”。这么做的好处是你可以完全自定义整个窗口，包括标题栏和关闭按钮使整个软件的风格统一，而且还能使用`Node.js`的模块，无论是本地模块还是第三方模块都能提供给这个“浏览器”使用，这意味着你可以编写出功能丰富的“网页”来。

但这种开发模式也有缺点，最明显的就是整个软件的体积会比较大，因为包含了内核。而且渲染性能比较低，因为在浏览器中遇到大量需要不断重绘的网页，都会很卡。但对于日常软件来说完全没有问题。

