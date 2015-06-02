---
layout: post
title: Rust快速入门
date: 2015-05-28
category: Rust
keywords: "Rust,快速入门,入门"
last_modified_at: 2015-05-30
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

- Booleans
- char Unicode 4字节
- Numeric i8 i16 i32 i64 u8 u16 u32 u64 isize usize f32 f64
- Arrays 数组的类型为`[T; N]`。`T`为类型，`N`为数组长度
- Slices `Slices`是对另一个数据结构的引用，目的是安全高效地对变量中的部分数据进行引用，类型为`&[T]`
- str 字符串，经常以引用方式`&str`来使用
- Tuples 元组与数组相似，但元素可以是不同的类型
- Functions 函数指针类型

```Rust
// Booleans
let x = true;
let y: bool = false;

// char
let x = 'x';
let two_hearts = '💕';

// Numeric
let x = 42; // x 类型为 i32
let y = 1.0; // y 类型为 f64

// Arrays
let mut m = [1, 2, 3]; // m: [i32; 3]
let a = [0; 20]; // a: [i32; 20]  初始化数组的值为0

// Slices
let a = [0, 1, 2, 3, 4];
let middle = &a[1..4]; // middle中只包含a数组中的[1,2,3]
let complete = &a[..]; // complete中包含a数组中的所有元素

// Tuples
let x = (1, "hello");
let x: (i32, &str) = (1, "hello");
let y = x.0;// 访问元组x的第一个元素

// Functions
fn foo(x: i32) -> i32 { x }
let x: fn(i32) -> i32 = foo;
// x 为foo函数的指针，函数参数为i32类型，返回类型为i32
```

##Comments(注释)

`Rust`有两种注释，行注释(line comments)和文档注释(doc comments)。行注释跟C语言一样使用`//`，把当前行注释掉。文档注释使用`///`，而且支持`Markdown`语法。你可以使用`rustdoc`将文档注释生成为HTML文档。

```Rust
/// Adds one to the number given.
///
/// # Examples
///
/// ```
/// let five = 5;
///
/// assert_eq!(6, add_one(5));
/// ```
fn add_one(x: i32) -> i32 {
    x + 1
}
```

##Control flow statement(控制语句)

###if

```Rust
let x = 5;

if x == 5 {
    println!("x is five!");
} else if x == 6 {
    println!("x is six!");
} else {
    println!("x is not five or six :(");
}
```

###for

```Rust
for x in 0..10 {
    println!("{}", x); // x: i32
}
//可以抽象为
for var in expression {
    code
}
```

跟`Python`的for语句有点像，`expression`是一个迭代器(iterator)即一个可以进行遍历的对象，比如数组。

###while

```Rust
while x > 1 {
    doSomething();
    if done == true {
        break;
    } else {
        continue;
    }
}
loop {
    doSomething();
    //等价于 while true需要break跳出循环
}
```

##Ownership

`Ownership`类似于作用域，在`Rust`程序中变量一旦离开作用域就会被释放。在变量绑定中，没有实现`Copy`特性的类型(如Vec<i32>)在进行赋值时，传递的是内存地址，而`Rust`出于安全考虑当两个变量绑定到同一值上时，`Rust`<br/>会将原来的绑定删除，在函数传参时同样如此。而实现了`Copy`的类型(如基本类型i32)在进行赋值时传递的就是值。

```Rust
let v = vec![1, 2, 3];
let v2 = v;
println!(v)//在这里就会报错，因为v的值([1, 2, 3]的地址)已经传给v2了

let v = 1;
let v2 = v;
println!("v is: {}", v);//在这里就不会报错，因为v是i32类型，传递的是值。
```

##Borrowing

如果一个变量作为参数传入了函数中，那么该变量的作用域就变了，变为在函数内，所以在函数外该变量就不能使用了。在`Rust`中有`Borrowing`的概念，通过给函数传入变量的引用来达到"借用"的目的，使得变量在函数外还能继续使用(Borrow Ownership)。引用与变量绑定一样，默认是`不可修改`的。引用类似C语言的指针，因为在使用时得加上`*`符号。

```Rust
//在类型前加上&符号表示该类型的引用
//在变量前加上&符号辨识变量的引用
fn foo(v1: &Vec<i32>, v2: &Vec<i32>) -> i32 {
    42
}
let v1 = vec![1, 2, 3];
let v2 = vec![1, 2, 3];
let answer = foo(&v1, &v2);
// v1 和 v2 还能继续使用
```

若要创建可修改的引用，需要使用`&mut`，而且被引用的变量也必须是可修改的。修改变量的引用，变量也会被修改。

```Rust
let mut x = 5;
{
    let y = &mut x;
    *y += 1;
}
println!("{}", x);
//输出6
```

一个变量可以有多个引用(不可修改的引用)，但同一时间(前一个可修改引用未被释放)只能有一个可修改引用。而且当作用域(scope)中存在变量的可修改引用(&mut T)时，无法创建该变量的引用。

```Rust
let mut x = 5;

{                   
    let y = &mut x; // -+ &mut borrow starts here
    *y += 1;        //  |
}                   // -+ ... and ends here

println!("{}", x);  // <- try to borrow x here
```

上面这个例子中x的可修改引用y在大括号中创建，并在大括号外释放，也就是限定了y变量的作用域。如果去掉那两个大括号最后的`println!("{}", x)`会报错。

`Rust`这么做主要是为了避免`数据竞争(data race)`当两个指针指向同一个内存地址时，会出现至少一个访问会等待，导致操作无法同步。

变量的定义必须在引用之前。

```Rust
let y: &i32;
let x = 5;
y = &x;

println!("{}", y);//此处会发生错误，因为x的定义是在y被定义之后。
//正确的写法是
let x = 5;
let y: &i32;
y = &x;
```

##Lifetimes

寿命(Lifetimes)指变量绑定在作用域内的范围。例如下面的例子中，变量的寿命可以显示或隐式定义。

```Rust
// 隐式定义
fn foo(x: &i32, y: &mut i32) {
}

// 显示定义
fn bar<'a, 'b>(x: &'a i32, y: &'b mut i32) {//定义可修改引用的寿命&'b mut i32
}
```

定义变量的寿命主要是为了防止某个被引用的资源释放后，引用出错。(类似C中的野指针)

```Rust
struct Foo<'a> {
    x: &'a i32,
}

fn main() {
    let x;                    // -+ x goes into scope
                              //  |
    {                         //  |
        let y = &5;           // ---+ y goes into scope
        let f = Foo { x: y }; // ---+ f goes into scope
        x = &f.x;             //  | | error here
    }                         // ---+ f and y go out of scope
                              //  |
    println!("{}", x);        //  |
}                             // -+ x goes out of scope
```

在上面的例子中`struct Foo`中的`x`的寿命就是`y`，因为`x`被赋值为`y`。而`y`的作用域仅在两个大括号内，所以当`y`被释放后，`f`也会被释放。

有一个特殊的变量寿命`'static`。也就是静态域，类似C++类中的静态变量。寿命为`'static`的变量绑定会在整个程序中都存在。

```Rust
static FOO: i32 = 5;
let x: &'static i32 = &FOO;
```

函数参数的寿命

- 函数的每个参数若省略定义寿命名则每个参数都有一个独立的寿命名。(只有引用类型的参数才需要寿命名)

```Rust
fn args<T:ToCStr>(&mut self, args: &[T]) -> &mut Command // 省略
fn args<'a, 'b, T:ToCStr>(&'a mut self, args: &'b [T]) -> &'a mut Command // 显示定义
```

- 如果只有一个输入寿命(无论是否省略)，该寿命应用于函数的所有返回值。

```Rust
fn new(buf: &mut [u8]) -> BufWriter; 
fn new<'a>(buf: &'a mut [u8]) -> BufWriter<'a> 
```

- 如果有多个输入寿命，其中一个为`&self` 或 `&mut self`，`self`的寿命将应用于所有省略了寿命的返回值。

```Rust
fn get_mut(&mut self) -> &mut T; 
fn get_mut<'a>(&'a mut self) -> &'a mut T; 
```

> 以上这3个`Rust`中的概念确实比较难懂，我是根据自己的理解写的，若有不同观点请看官方原文。

##Structs

与C语言的结构体类似，将某些数据类型组合在一起，形成新的数据结构。

```Rust
struct Point {  //名称第一个字母大写，采用驼峰命名法
    x: i32,     //不能写成mut x: i32,
    y: i32,
}

fn main() {
    let origin = Point { x: 0, y: 0 }; //定义一个Point类型的变量绑定，并赋值

    println!("The origin is at ({}, {})", origin.x, origin.y);//struct变量访问
}

let mut point = Point3d { x: 0, y: 0, z: 0 };
point = Point3d { y: 1, .. point };
//新的ponit y为1，x和z使用原来的point的值
```

###Tuple structs

定义一个类似于`tuple`的结构。

```Rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);
//以下两个变量不相等
let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

###Unit-like structs

可以定义一个无成员的结构

```Rust
struct Electron;
```

##Enums

`Rust`的枚举类型，类型为`Message`的变量绑定可以是`Message`的其中之一

```Rust
enum Message {
    Quit,
    ChangeColor(i32, i32, i32),
    Move { x: i32, y: i32 },
    Write(String),
}

let x: Message = Message::Move { x: 3, y: 4 };
```

##Match

`match表达式`类似于C语言中的`switch`

```Rust
let x = 5;

match x {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    4 => println!("four"),
    5 => println!("five"),
    _ => println!("something else"),//当所有值都不匹配时
}

//x可以为`enum`类型
enum Message {
    Quit,
    ChangeColor(i32, i32, i32),
    Move { x: i32, y: i32 },
    Write(String),
}

fn quit() { /* ... */ }
fn change_color(r: i32, g: i32, b: i32) { /* ... */ }
fn move_cursor(x: i32, y: i32) { /* ... */ }

fn process_message(msg: Message) {
    match msg {
        Message::Quit => quit(),
        Message::ChangeColor(r, g, b) => change_color(r, g, b),
        Message::Move { x: x, y: y } => move_cursor(x, y),
        Message::Write(s) => println!("{}", s),
    };
}
```

##Patterns

模式，`match`中x所匹配的就是模式

```Rust
let x = 1;

match x {
    1 | 2 => println!("one"), //匹配1或2
    3 ... 7 => println!("two"),//匹配3, 4, 5, 6, 7
    'a' ... 'j' => println!("three"),//匹配字母a到j
    e @ 8 ... 10 => println!("got a range element {}", e),//若x为10，e所绑定的值就为10
    _ => println!("anything"),
}
```
匹配数据结构的一部分
```Rust
#[derive(Debug)]
struct Person {
    name: Option<String>,
}

let name = "Steve".to_string();
let mut x: Option<Person> = Some(Person { name: Some(name) });
match x {
    Some(Person { name: ref a @ Some(_), .. }) => println!("{:?}", a),
    _ => {}
}
```

匹配有变量的枚举类型，使用`..`来忽略掉参数

```Rust
enum OptionalInt {
    Value(i32),
    Missing,
}

let x = OptionalInt::Value(5);
let mut y = 5;

match x {
    OptionalInt::Value(i) if i > 5 => println!("Got an int bigger than five!"),//添加if作为判断
    OptionalInt::Value(..) => println!("Got an int!"),
    OptionalInt::Missing => println!("No such luck."),
    ref y => println!("Got a reference to {}", y),//获取引用
    ref mut mr => println!("Got a mutable reference to {}", mr),
}//最后输出Got an int!
```

匹配`struct`类型

```Rust
struct Point {
    x: i32,
    y: i32,
}

let origin = Point { x: 0, y: 0 };

match origin {
    Point { x: x, y: y } => println!("({},{})", x, y),
    Point { x: x, .. } => println!("x is {}", x),
}
```

以上列出的匹配可以任意组合在一起。

##Method Syntax

```Rust
struct Circle {
    x: f64,
    y: f64,
    radius: f64,
}

impl Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * (self.radius * self.radius)
    }
}

fn main() {
    let c = Circle { x: 0.0, y: 0.0, radius: 2.0 };
    println!("{}", c.area());
}
```

先定义一个`struct` 叫`Circle`，再用`impl`往`Circle`中添加一个方法`area`，每个方法都会有一个特殊的参数，可以是`self`，`&self`，`&mut self`其中之一。 传值方式与[Functions](#Functions)一节相同。在上面的代码中`self`指代的就是`c`这个变量(类似于其他语言中的this)，所以在这里我们使用的是引用，而且一般情况下也都是使用引用。

###Chaining method calls(链式调用)

通过返回`self`来达到链式调用的目的

```Rust
struct Circle {
    x: f64,
    y: f64,
    radius: f64,
}

impl Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * (self.radius * self.radius)
    }

    fn grow(&mut self, increment: f64) -> &Circle {
        self.radius += increment;
		return self;
    }
}

fn main() {
    let mut c = Circle { x: 0.0, y: 0.0, radius: 2.0 };
    println!("{}", c.area());

    let d = c.grow(2.0).area();
    println!("{}", d);
}
```

官方的代码是返回一个新的`Circle`。此处我做了下修改以更符合返回`self`的一般情况。

###Associated functions

联合函数不需要`self`参数

```Rust
struct Circle {
    x: f64,
    y: f64,
    radius: f64,
}

impl Circle {
    //new方法返回一个Circle
    fn new(x: f64, y: f64, radius: f64) -> Circle {
        Circle {
            x: x,
            y: y,
            radius: radius,
        }
    }
}

fn main() {
    let c = Circle::new(0.0, 0.0, 2.0);//联合函数的调用方法Struct::function()
    //类似于其他语言中的静态方法
}
```

###Builder Pattern

为了使用户只能修改`struct`中特定的属性，需要使用另一个`struct`来作限制，如`Circle`的`CircleBuilder`。

```Rust
struct Circle {
    x: f64,
    y: f64,
    radius: f64,
}

impl Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * (self.radius * self.radius)
    }
}

struct CircleBuilder {
    x: f64,
    y: f64,
    radius: f64,
}

impl CircleBuilder {
    fn new() -> CircleBuilder {
        CircleBuilder { x: 0.0, y: 0.0, radius: 1.0, }
    }

    fn x(&mut self, coordinate: f64) -> &mut CircleBuilder {
        self.x = coordinate;
        self
    }

    fn y(&mut self, coordinate: f64) -> &mut CircleBuilder {
        self.y = coordinate;
        self
    }

    fn radius(&mut self, radius: f64) -> &mut CircleBuilder {
        self.radius = radius;
        self
    }

    fn finalize(&self) -> Circle {
        Circle { x: self.x, y: self.y, radius: self.radius }
    }
}

fn main() {
    let c = CircleBuilder::new()
                .x(1.0)
                .y(2.0)
                .radius(2.0)
                .finalize();

    println!("area: {}", c.area());
    println!("x: {}", c.x);
    println!("y: {}", c.y);
}
```

通过使用`CircleBuilder`来创建`Circle`，就可以对`Circle`的创建和修改做出约束。

##Vectors(向量)

向量(`Vec<T>`)是动态可增长的数组，存储在堆上。使用`vec!`宏创建。

```Rust
let v = vec![1, 2, 3, 4, 5]; // v: Vec<i32>
let v = vec![0; 10]; // 10 个 0
println!("The third element of v is {}", v[2]);//下标从0开始
//遍历向量
for i in &v {
    println!("A reference to {}", i);
}

for i in &mut v {
    println!("A mutable reference to {}", i);
}

for i in v {
    println!("Take ownership of the vector and its element {}", i);
}
```

##Strings

`Rust`有两种字符串类型`&str`(`&'static str`)和`String`，都是UTF-8编码(一个字符占4字节)

`&str`类型的字符串如

```Rust
let string = "Hello there."; // string: &'static str
```

存在于静态域，整个程序都可以访问，固定长度，无法被修改。

`String`是在堆上创建的字符串，可加长，通常使用`to_string`从`&str`格式化得到。

```Rust
let mut s = "Hello".to_string(); // mut s: String 使用了to_string()方法才可以修改s
println!("{}", s);

s.push_str(", world.");
println!("{}", s);
```

可以使用`&`将`String`强制格式化为`&str`

```Rust
fn takes_slice(slice: &str) {
    println!("Got: {}", slice);
}

fn main() {
    let s = "Hello".to_string();
    takes_slice(&s);
}
```

> **注意**:  `String`可以轻易地变成`&str`，但`&str`格式化为`String`需要分配内存(因为是在堆上创建)，所以必要情况下才这么做。

###Indexing

无法通过`s[0]`来访问某个字符，因为字符是UTF-8编码，但可以这样做

```Rust
let hachiko = "忠犬ハチ公";
let dog = hachiko.chars().nth(1); // 类似于 hachiko[1]
```

> **注意**: chars()操作需要遍历整个字符串

###Concatenation

如果你有一个`String`类型的字符串，可以将`&str`类型的字符串连接到末尾。

```Rust
let hello = "Hello ".to_string();
let world = "world!";
let hello_world = hello + world;
```

如果是两个`String`类型的字符串，连接时第二个需要转换为`&str`类型

```Rust
let hello = "Hello ".to_string();
let world = "world!".to_string();
let hello_world = hello + &world;
```

##Generics