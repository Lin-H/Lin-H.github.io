title: 2015-november-summary
tag:
- life
---

2015年11月份，仍在广州实习。9月份时候的实习做的是Node.js开发，现在是Javascript工程师，也就是前端了。工作的内容也不难，但接触到的东西还是很有收获的。了解到公司的内部结构，使用的技术，真实的产品。尤其是使用浏览器内核来开发软件，解决了我好几年前关于软件开发的疑惑。还有就是MVVM框架`Vue.js`的使用，这个是主要内容了。

第一天去的时候，主要是学习内部的公用库，sass和js。关于css预处理器的选择有`Sass`，`Less`，`Stylus`，其实本质上都差不多基本就是学会了一个就能使用这3种，有的是细微差别，如果你不喜欢写分号，喜欢Python那种缩进形式的我推荐`Stylus`。至于我自己的话就是比较倾向于`Sass`了，以前都是使用Ruby来进行编译，但现在随着`Node.js`的崛起，也有了`node-sass`，就连我现在的这个博客也从`Jekyll`迁移到了`Hexo`上，为的是统一工具链:)

js部分的话使用的还是传统的`jQuery`和事件监听(请原谅我说jQuery很传统)，开发流程都是集中在给DOM绑定事件，触发事件后修改DOM，当时读代码的时候确实头晕，往往需要结合已有的项目去读，不然不了解HTML的结构不知道某些函数操作是干嘛的，项目一旦复杂起来维护就比较麻烦了。

而且jQuery的很多常用函数都能使用原生js来实现，`addEventListener`,`removeEventListener`,'document.querySelector'等等。所以我们前端就开始了迁移工作，使用`webpack`和`Vue.js`来改写。

`webpack`是个很棒的打包工具，也自带有一些插件，可以完成代码压缩，公共库打包，css文件单独抽取等操作，但类似于sprite图片的合并就需要`gulp`来构建了，更何况`gulp`有可以调用`webpack`的插件。除此之外，还使用ES6来开发，通过`Babel`来编译，以`Vue`组件的形式来编写代码css、HTML、Javascript都写在同一个文件中，通过`vue-loader`来打包，所以整个的工具链是`Vue` -> `Babel` -> `webpack` -> `gulp`。

## Vue

