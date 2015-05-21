---
layout: post
title: Rust快速入门
category: Rust
keywords: "Rust,快速入门,入门"
---
##Rust简介

`Rust`


##Variable Bindings(变量绑定)

定义变量绑定使用`let`语句。

> **注意**：在`Rust`中处于安全考虑，定义的变量默认都是不可修改的，如果需要定义可修改的变量需要在变量前加上`mut`关键字。

`Rust`是静态类型语言(statically typed language)，在定义变量时若不指定变量类型，`Rust`会自动进行类型推导(type inference)。

```Rust
fn main() {
    let x = 5;    //类型推导，定义的x类型为i32值为5
    let mut x: i32 = 10;    //定义一个类型为i32，值为10，可修改的变量绑定
}
```

##Functions(函数)

`Rust`使用`fn`关键字来声明函数。

```Rust
//定义一个无参数的函数
fn foo() {
    println!("Hello Rust");
}

//定义一个有2个参数的函数
fn print_sum(x: i32, y: i32) {
    println!("sum is: {}", x + y);
}
```

> **注意**：函数的参数必须指定类型。

函数返回值，在函数声明中使用`->`指定返回的值的类型。函数的最后一行指定了要返回的值，而且这一行不能以分号`;`结尾，否则报错。当然你也可以使用`return`关键字来返回一个值。但如果需要返回的值在函数最后一行，通常不使用`return`。

```Rust
//返回一个32位整数类型
fn add_one(x: i32) -> i32 {
    x + 1  //return x + 1;
}
```

`Rust`是基于表达式的语言，在`Rust`中只有两种语句(statements)，声明语句(declaration statements) 和 表达式语句(expression statements)，表达式语句的作用就是使表达式不返回值。其他的全都是表达式。主要区别为表达式返回值，语句不返回值。赋值表达式的返回值是空的元组`()`。

若函数的返回值是`!`，称为`发散函数(Diverging functions)`，代表该函数不会返回。如

```Rust
fn diverges() -> ! {
    panic!("This function never returns!");
}
```

`panic!()`宏(以!结尾的都是宏，类似的还有`println!()`)的作用是使当前线程崩溃退出，并输出信息。因为运行该函数的线程崩溃退出了，所以该函数不会返回。(我暂时不知道这类发散函数有什么用-_-)

##基本类型(Primitive Types)

