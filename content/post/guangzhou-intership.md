---
title: 广州实习
date: 2015-10-07 18:36:00
categories: 
- Others
tags:
- Node.js
- Life
keywords: "intership,note,广州,实习,node.js,javascript"
last_modified_at: 2015-11-02 23:47:58
// banner: http://linhsblog-10013469.image.myqcloud.com/images/guangzhou.jpg
---

从19月份开始在广州实习，主要从事`Node.js`的开发，使用`MongoDB`数据库，`Express`框架。在后台主要提供的是`RESTful`的接口，前端使用`Angular`框架进行数据整合。以下做一个简单的总结。(部分代码来自于[npmjs.org](https://www.npmjs.com))
<!--more-->

## [urllib](https://www.npmjs.com/package/urllib)

`urllib`是一个用于发送HTTP请求的库，功能强大，使用简单。当时有个项目需要将对方已有的，针对web页面的api接口重新封装成适用于app端的接口。并且还需要添加一些额外数据。当时就用到了这个库。

```js
var urllib = require('urllib');

urllib.request('http://cnodejs.org/', function (err, data, res) {
  if (err) {
    throw err; //错误处理
  }
  //res是请求成功后得到的HTTP回复
  console.log(res.statusCode);
  console.log(res.headers);
  //data就是实际得到的数据内容
  console.log(data.toString());
});
```

## [formstream](https://www.npmjs.com/package/formstream)

使用`Node.js`提交表单，表单在从前端发送至后端的时候并不是使用明文传输，在同时传输文件和其他value时，使用的是`multipart/form-data`，会以二进制的形式发送所有数据。`Express`框架在接收到提交过来的表单的时候可以保留文件的二进制形式，然后使用`formstream`往这个二进制流中添加额外的数据(formstream中有相应的方法),最后得到的也是一个二进制流，在`urllib`中设置好头部后，添加表单数据就能发送了。

```js
var formstream = require('formstream');
var http = require('http');

var form = formstream();

// form.file('file', filepath, filename);
// 向表单中添加文件
form.file('file', './logo.png', 'upload-logo.png');

// 添加其他的值form 中的 field
form.field('foo', 'fengmk2').field('love', 'aerdeng');

// 直接添加而二进制的流
// form.buffer(name, buffer, filename, mimeType)
form.buffer('file2', new Buffer('This is file2 content.'), 'foo.txt');

var options = {
  method: 'POST',
  host: 'upload.cnodejs.net',
  path: '/store',
  headers: form.headers()//设置HTTP请求头
};
var req = http.request(options, function (res) {
  console.log('Status: %s', res.statusCode);
  res.on('data', function (data) {
    console.log(data.toString());
  });
});
```

## [moment](http://momentjs.com/)

`moment`是一个十分强大的时间处理库，几乎任何你想要的操作时间的函数都会有。

```js
moment().format('LLL');   // November 2, 2015 11:17 PM
moment().add(7, 'days').subtract(1, 'months').year(2009).hours(0).minutes(0).seconds(0); //对日期进行加减操作
moment([2100]).isLeapYear() // 是否是闰年
```

## [async](https://github.com/caolan/async)

又是一个十分强大的库，`async`是一个用来进行更好异步编程的库，为什么说是更好，因为Node.js本身的异步方式无论是`callback`，`XHR`或者`Promise`都有自身的缺陷，`callback`本身不适合有依赖的回调过程，`Promise`难以控制需要同步的异步方法。但是有了`async`就可以进行多个异步任务，只要告诉它异步任务执行的条件就行，还能编写同步的异步方法。

```js
async.auto({
    get_data: function(callback){
        console.log('in get_data');
        // 这里是异步运行的代码
        callback(null, 'data', 'converted to array');
    },
    make_folder: function(callback){
        console.log('in make_folder');
        // 异步代码，创建一个文件夹来存储文件
        // 这一函数与get_data函数一同运行
        callback(null, 'folder');
    },
    write_file: ['get_data', 'make_folder', function(callback, results){
        console.log('in write_file', JSON.stringify(results));
        // 当拿到数据，且创建好文件夹后调用
        // 把数据写入文件中
        callback(null, 'filename');
    }],
    email_link: ['write_file', function(callback, results){
        console.log('in email_link', JSON.stringify(results));
        // 当数据写入完毕后调用
        // results.write_file这个变量中存的就是write_file函数返回的结果
        callback(null, {'file':results.write_file, 'email':'user@example.com'});
    }]
}, function(err, results) {
    //最后这个函数是当所有异步任务都完成时调用的
    console.log('err = ', err);
    console.log('results = ', results);
});

//按顺序执行的异步任务async.series(array, callback)
//array中是以数组形式存储的一些列函数
async.series([{
    one: function(callback){
        setTimeout(function(){
            callback(null, 1);
        }, 200);
    },
    two: function(callback){
        setTimeout(function(){
            callback(null, 2);
        }, 100);
    }
}],
function(err, results) {
    // results is now equal to: {one: 1, two: 2}
});
```

在`Node.js`中异步调用随处可见，使用`async`能够更好的控制异步任务，尽管一开始使用这个库有点困难，但学会后能够大大提高效率。

## MongoDB

非关系型数据库，说直接点就是存储`JSON`格式数据的数据库，没有表的概念，只有`集合Collection`，就相当于SQL中的表。数据叫做一个文档

项目中使用的库是[`Mongoose`](http://mongoosejs.com/)，使用起来还是挺简单的

```js
//查询name为john，age大于等于18的数据
MyModel.find({ name: 'john', age: { $gte: 18 }});

//插入
MyModel.create({ type: 'jelly bean' } function (err, jellybean, snickers) {
  if (err) // ...
});

//修改age大于18的数据，设置oldEnough字段为true
MyModel.update({ age: { $gt: 18 } }, { oldEnough: true }, callback);

//删除title为baby born from alien father的json
Comment.remove({ title: 'baby born from alien father' }, function (err) {
});
```

基本操作都没有什么难度，但在关联表查询的时候就复杂一点，需要手动设置条件，因为非关系型数据库没有表关联。所以许多复杂的操作都是通过MongoDB中的操作符来实现的。

比如像前面看到的`$gt`就是大于的意思，`$gte`大于等于，列举一些比较常用的

- $in {a: {$in: [1, 2, 3]}}满足条件a的值为1或2或3的文档
- $or 或条件 {$or: [{a: {$gt: 0}}, {b: $lg: 0}]}满足a大于0或是b小于0
- $regex 对某个字段进行正则匹配，通过即为满足条件
- $inc 增加某个字段指定数量{a: {$inc: 1}}a字段加1
- $currentDate 当前时间，相当于SQL的now()
- $pop 删除一个数组字段的元素
- $push 插入一个数组类型的字段
- $sort 排序，常用的是{$sort: "desc"}降序排列
- $limit 只取指定数量的文档
- $skip 跳过指定数量的文档

还有其他很多操作符[Operators &mdash; MongoDB Manual 3.0](https://docs.mongodb.org/manual/reference/operator/)

