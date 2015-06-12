---
layout: post
title: Go语言学习笔记
category: Go
keywords: "Go语言,Go,notes,笔记"
date: 2015-06-13 00:17:17
last_modified_at: 2015-06-13 00:17:17
excerpt: <p><code class="highlighter-rouge">Go</code>是一种并发的、带垃圾回收的、快速编译的静态语言，用于编写简单、可靠、高效的软件。多用在云计算方面。</p>
---
## Go简介

`Go`是一种并发的、带垃圾回收的、快速编译的静态语言，用于编写简单、可靠、高效的软件。多用在云计算方面。

## package(包)

每个 Go 程序都是由包组成的，程序运行的入口是函数 `main`，使用`import`关键字导入其他的包

``` go
package main

import (       //import "fmt"
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}
```

> 在Go的包中，只有首字母大写的函数时被导出的，小写的函数不会被导出

## 变量

变量的类型跟在变量名之后，若有初始值则可省略类型声明，`Go`可以自动推导

```go
var c, python, java bool = true //定义并初始化变量

func main() {
	var i int
	fmt.Println(i, c, python, java)
}
```

在函数内还可以使用简写方式`:=`

```go
func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

## 基本类型

- bool
- string
- int  int8  int16  int32  int64
- uint uint8 uint16 uint32 uint64 uintptr
- byte   uint8 的别名
- rune   int32 的别名 代表一个Unicode码
- float32 float64
- complex64 complex128

### 零值

当变量定以后没有进行赋值的话，变量将会被赋值`零值`

- 数值类型为 `0`，
- bool为 `false`，
- 字符串为 `""`(空串)
- 指针为 `nil`
- slice 为`nil`，长度和容量都是0
- map 为`nil`

### 类型转换

表达式 T(v) 将值 v 转换为类型 `T`

``` go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

### 常量

使用`const`关键字，数值常量是高精度的值

```go
const Pi = 3.14
const World = "世界"
```

## 函数

使用`func`定义函数，函数参数的类型在参数之后，最后是函数的返回值类型

```go
func add(x int, y int) int {//参数可以缩写为(x, y int)
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```

`Go`中的函数可以返回多个值，还可以将返回的变量命名,并且像变量那样使用

```go
func swap(x, y string) (a string, b string) {
	return y, x
}
```

函数是值，可以传递给变量，也可以当做另一个函数的返回值

```go
func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}

	fmt.Println(hypot(3, 4))
}
//adder函数返回另一个函数
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}
```

## 控制语句

### for

`Go`中只有一种循环结构`for`，结构与`C语言`的`for`语句类似，但却没有了括号

```go
for i := 0; i < 10; i++ {
	sum += i
}
fmt.Println(sum)
//与C一样可以省略部分语句
for sum < 1000 {//起到while语句的作用
	sum += sum
}
//死循环
for {
}
```

### if

```go
if v := math.Pow(x, n); v < lim {//变量v的作用域是if语句
	return v
} else {
	fmt.Printf("%g >= %g\n", v, lim)
}
```

### switch

`Go`的`switch`语句与`C语言`的不同，默认情况下，执行了一个分之后不会向下继续执行，匹配成功后就会跳出，除非使用了`fallthrough`继续向下执行。

```go
today := time.Now().Weekday()
switch time.Saturday {
case today + 0:
	fmt.Println("Today.")
case today + 1:
	fmt.Println("Tomorrow.")
case today + 2:
	fmt.Println("In two days.")
	fallthrough
default:
	fmt.Println("Too far away.")
}
//switch true 的用法  简化多个if-else语句
t := time.Now()
switch {
case t.Hour() < 12:
	fmt.Println("Good morning!")
case t.Hour() < 17:
	fmt.Println("Good afternoon.")
default:
	fmt.Println("Good evening.")
}
```

### defer

延迟函数的执行，直到上层函数返回时才执行。延迟的函数调用被压入一个栈中，然后按照先进后出的顺序执行。

```go
func main() {
	defer fmt.Println("world")//在main函数结束后才执行该语句

	fmt.Println("hello")//输出结果为helloworld
}
```

## Point(指针)

```go
var p *int//定义int类型的指针
i := 42
p = &i //取变量i的地址
fmt.Println(*p)//输出p所指的值
```

## Struct(结构体)

结构体使用中的字段`.`来访问，也可以通过指针简介访问。`type`关键字用来定义一种类型

```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4   //通过变量访问
	fmt.Println(v.X)
	p := &v   //通过指针访问
	p.X = 1e9
}
```

## Array(数组)

类型`[n]T`是一个有n个类型为T的元素的数组，数组下标从0开始，并且是固定长度

```go
func main() {
	var a [2]string//定义一个有2个元素的string数组
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)
}

```

## slice

`slice`会指向一个序列的值，并且包含了长度信息,`[]T`是一个元素类型为T的`slice`，在声明时没有指定大小的数组

```go
p := []int{2, 3, 5, 7, 11, 13}
fmt.Println("p ==", p)
```

重新slice

```go
p := []int{2, 3, 5, 7, 11, 13}
fmt.Println("p[1:4] ==", p[1:4])//1到3元素
fmt.Println("p[:3] ==", p[:3])//0到2元素
fmt.Println("p[4:] ==", p[4:])//从4开始到最后的元素
```

构造slice

```go
a := make([]int, 5)  // len(a)=5
b := make([]int, 0, 5)//使用第三个参数可以指定容量len(b)=0, cap(b)=5
```

使用`append`函数向slice添加元素

```go
var a []int
printSlice("a", a)
a = append(a, 2, 3, 4)//append会返回一个新的slice，添加的元素在末尾
```

### 使用for对slice进行遍历

```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {//还可以将i替换成_来忽略索引值
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```

## map

键值映射，类似于`Python`中的字典，`map`必须用`make`创建

```go
type Vertex struct {//定义一个结构体
	Lat, Long float64
}
//声明map类型的变量,键是string,值是Vertex
var m map[string]Vertex //也可以初始化map，在声明后加上大括号，里面加上初始值

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}
//或者省略键名
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```

### 对map的操作

```go
//插入或修改
m[key] = elem
//读取
elem = m[key]
//删除
delete(m, key)
//判断某个键是否存在，若存在ok=true,否则ok=false
elem, ok = m[key]
```

## Method

`Go`中没有类，但可以在结构体上定义方法，除了结构体以外，还可以对其他类型添加方法

```go
type Vertex struct {
	X, Y float64
}
//(v *Vertex)为Abs方法的接收者，这里的v(可以使用其他名称)相当于类中的this，指代当前对象(Vertex类型的变量)
func (v *Vertex) Abs() float64 {//使用指针类型来避免值传递，而且可以修改v变量
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := &Vertex{3, 4}//定义一个Vertex类型的变量
	fmt.Println(v.Abs())//该变量中就拥有了Abs这个方法
}
```

## Interface(接口)

接口是一些方法的集合或其他的接口，这些方法只有声明部分，并没有被实现。接口类型的变量可以存放任何实现了这些方法的变量。

```go
type Vertex struct {
	X, Y float64
}
type Abser interface {
	Abs() float64
}
func (v *Vertex) Abs() float64 {//*Vertex类型实现了Abser接口中的Abs方法
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
func main() {
	v := &Vertex{3, 4}//定义一个Vertex类型的变量
	var a Abser  //定义一个Abser接口类型的变量
	a = &v   //因为*Vertex类型实现了Abser接口，所以a可以存放*Vertex的变量
}
```

在`fmt`包中，有一个通用的接口`Stringer`

```go
type Stringer struct {
    String() string
}
```

接口中的`String()`方法类似于`Java`的`toString()`，作用是用字符串描述自身

## Error(错误)

与`Stringer`类似，`error`也是一个通用接口，返回值类型为`error`的函数，可以检查该错误是否等于`nil`来判断是否出错

```go
i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
}
fmt.Println("Converted integer:", i)
```

## goroutine

`goroutine`是由`Go`运行时环境管理的轻量级线程

```go
go f(x, y, z)//定义f函数以另一条线程运行
f(x, y, z)//运行f函数
```

`goroutine`在相同的地址空间中运行，因此访问共享内存必须进行同步，使用`sync`关键字(但并不常用)

## channel

`channel`是有类型的管道(阻塞式)，使用`<-`符号来操作(代表数据流通方向)，`channel`使用关键字`chan`定义，之后需要使用`make`来创建

```go
func sum(a []int, c chan int) {
	sum := 0
	for _, v := range a {
		sum += v
	}
	c <- sum // 将和送入 c
}

func main() {
	a := []int{7, 2, 8, -9, 4, 0}
	//ch := make(chan int)
	c := make(chan int)//管道的类型是int
	go sum(a[:len(a)/2], c)
	go sum(a[len(a)/2:], c)
	x, y := <-c, <-c // 从 c 中获取

	fmt.Println(x, y, x+y)
}
```

缓冲管道定义的方式，第二个参数定义缓冲区长度，只有在缓冲区满的时候才会阻塞

```go
ch := make(chan int, 100)
```

### range and close

使用`close`来关闭管道，例如循环 `for i := range c` 会不断从 channel 接收值，直到它被关闭(其他情况下一般无需关闭)

```go
func fibonacci(n int, c chan int) {
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)
}

func main() {
	c := make(chan int, 10)
	go fibonacci(cap(c), c)
	for i := range c {
		fmt.Println(i)
	}
}
```

## select

select 语句使得一个`goroutine`在多个操作上等待，select 会阻塞，直到条件分支中的某个可以继续执行，这时就会执行那个条件分支。当多个都准备好的时候，会随机选择一个。

```go
func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		default:	//当其他分支都没有准备好时，选择default
			fmt.Println("default")
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}
```

## 参考链接

- [Go 指南](http://tour.studygolang.com/list)