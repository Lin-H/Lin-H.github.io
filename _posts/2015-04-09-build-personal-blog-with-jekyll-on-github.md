---
layout: post
title:  "使用Jekyll在Github上搭建个人Blog"
date:   2015-04-09 20:32:46
categories: jekyll
---
拥有自己的个人Blog可以把自己学到的技术，想法、经历分享出来，也可以用来写日记。远比QQ空间有意思多了，尤其是对于程序员来说，可以随意折腾。`Jekyll`就是一款生成静态页面(HTML文件)的工具，`Jekyll`是用Ruby编写的。但使用者不需要掌握Ruby，只需要在控制台(terminal)中输入几条简单的命令就能搭建出一个Blog来，但如果你还懂得网站前端的知识如`HTML`，`CSS`，`Javascript`的话，你就能完全自定义你自己的Blog。

在开始之前需要先了解一下[`Github`](https://github.com/)并搭建好开发环境。

##Github 代码托管网站

`Github`是一个基于git版本控制工具的代码托管网站。
以下是`Github`网站的官方介绍
> GitHub is the best place to share code with friends, co-workers, classmates, and complete strangers. Over eight million people use GitHub to build amazing things together.

简单来说就是一个存放代码的地方，代码按仓库(repository)管理，仓库又分为私人仓库(private repository)和公共仓库(public repository)。私人仓库需要收费，而公共仓库免费，并且不限制数量。而`Github`最棒的地方就是多人协作。也就是开源程序，人人都能为代码提交修改。像Linux内核、Node.js 这些著名的程序都在`Github`上托管包括`Jekyll`。而且在`Github`上还能找到很多有意思的项目。所以先注册一个`Github`帐号吧。

完成了帐号注册后就能创建仓库了
![]({{site.url}}/assets/jekyll/create-a-repository.png)
创建的仓库名称必须是username.github.io，其中username就是你的用户名。Description可填可不填。

创建完成之后，就可以把代码上传上去了，这时就需要使用git工具了