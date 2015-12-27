---
layout: post
title: PHP notes
category: PHP
keywords: ""
published: false
---

### namespace

namespace a\s\d\v  定义命名空间，一般是按照路径来定，但跟路径无关系
use namespace [namespace]；//来使用命名空间

### interface

interface 与Java接口作用相同

### trait

trait 结合了interface和class的特点
使用方法
```php
class a {
use Mytrait;
}
```

### Closure

```php
$closure = function ($name) {
    return sprintf( 'Hello %s', $name );
};
echo $closure("Josh");
// Outputs --> "Hello Josh"
function enclosePerson( $name) {
    return function ($doCommand ) use ($name,[ , ]) {//use 添加变量多参数用逗号分隔，或使用bindTo()函数
        return sprintf('%s, %s', $name, $doCommand);
    };
}
```

### Generators

跟Javascript的不同，`PHP`返回的是类似数组那样可迭代的对象

```php
function myGenerator() {
    yield 'value1';
    yield 'value2';
    yield 'value3';
}
foreach (myGenerator() as $yieldedValue) {
    echo $yieldedValue, PHP_EOL;
}
```

### Zend OPcache

`PHP`的编译缓存，默认不开启，开启可加速`PHP`脚本编译过程

### Router Scripts

在开发时，可以使用简单的PHP built-in Server，为了配合框架使用添加`Router Scripts`作为内置服务器的启动参数

```shell
php -S localhost:8000 router.php
```

当请求的是静态资源时，`router.php`返回`false`，其他请求就直接转发给框架的`index.php`文件

```php
//request for static resource
if (preg_match('/\.(\w+)$/', $_SERVER["REQUEST_URI"])) {
    return false; // serve the requested resource as-is.
} else {
    require "./laravel/public/index.php";
}
```

