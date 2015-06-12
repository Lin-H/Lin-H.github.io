---
layout: post
title: R学习笔记
category: R
keywords: "R语言,R,快速,quick,notes,笔记"
published: false
---

##R RStudio

所使用的`R`版本为3.2.0，`RStudio`是`R`的IDE，有自动补全，查看API，管理包都十分方便。

##R 基础

`#`符号在`R`的代码中是注释符，跟在后面的代码不做处理直接显示。在此使用`##`代表代码运行后的结果。

```r
#基本运算符 > , < , <= , >= , ==
1 + 1
1 - 1
1 * 1
2 / 3 ##0.6666667
#创建序列1,2,3,4,5,6,7,8,9,10
1:10

#变量赋值
a <- 1
die <- 1:6 ##1,2,3,4,5,6
```

变量的使用不需要定义，赋值后就能使用，变量名不能以数字开头，不能包含`^`,`!`,`$`,`@`,`+`,`-`,`*`,`/`，并且区分大小写。

```r
ls() #使用ls()函数查看存在的变量
## "a" "die" "my_number" "name" "Name"
```

基本运算同样适用于序列

```r
die <- 1:6
die - 1
## 0 1 2 3 4 5
die / 2
## 0.5 1.0 1.5 2.0 2.5 3.0
die * die
## 1 4 9 16 25 36
die + 1:2
## 2 4 4 6 6 8

#向量内积
die %*% die
## 91

#向量外积
die %o% die
##    [,1] [,2] [,3] [,4] [,5] [,6]
## [1,] 1 	 2    3    4    5    6
## [2,] 2    4    6    8    10   12
## [3,] 3    6    9    12   15   18
## [4,] 4    8    12   16   20   24
## [5,] 5    10   15   20   25   30
## [6,] 6    12   18   24   30   36
```

###函数调用

```r
round(3.1415)
## 3

factorial(3)
## 6

mean(1:6)
## 3.5

sample(x = 1:4, size = 2)#从x中随机选择size个元素
## 3 2
```

###函数定义

```r
f <- function(args1 = 1:6, args2){#1:6为args1的默认参数
	#函数主体
	sample(args1, size = 2) #最后一行的值将会作为返回值返回
}
```

下载包，并使用

```r
install.packages("ggplot2")#下载ggplot2包

library("ggplot2")#使用ggplot2包

x <- seq(-1, 1, 0.1)#生成序列从-1开始到1，间隔0.1
y <- x^3
qplot(x, y)#ggplot2包中的函数，
```

得到如下的函数图像

![qplot(x, y)]({{site.url}}/assets/my-R-notes/qplot_function.png)

`replicate`函数，用于多次执行一条`R`表达式

```r
replicate(10, roll())#重复10次roll()
## 3 7 5 3 6 2 3 8 11 7
```

重要的`?`符号

```r
?sqrt
?sample
#查看某个函数的使用方法
??log
#搜索包含log关键字的帮助页面
```

###向量

向量中的数据必须是同一类型，若不是，则会自动转换(元素下标从0开始)

```r
die <- c(1, 2, 3, 4, 5, 6)
die
## 1 2 3 4 5 6

is.vector(die)
## TRUE

typeof(die)
## "double"

int <- c(-1L, 2L, 4L)#整型数据不常用
typeof(int)
## "integer"

text <- c("Hello", "World")
text
## "Hello" "World"
typeof(text)
## "character"
```

浮点数据精度

```r
sqrt(2)^2 - 2#因为数据精度的问题结果不为0
## 4.440892e-16
```

复合类型(Complex)和原始类型(Raw)

```r
comp <- c(1 + 1i, 1 + 2i, 1 + 3i)
typeof(comp)
## "complex"

raw(3)#以字节存储的数据
## 00 00 00  
typeof(raw(3))
## "raw"
```

###属性

属性相当于元数据(metadata)附着在向量上，一般情况下不会显示，也不会对向量的值产生变化，但有些函数需要使用一些属性来进行特殊的操作。默认下向量都没有属性。

```r
attributes(die)
## NULL
```

常见的属性有`names`,`dimensions (dim)`,`classes`。

```r
#向die中添加names属性
names(die) <- c("one", "two", "three", "four", "five", "six")

names(die)
## "one" "two" "three" "four" "five" "six"

attributes(die)
## $names
## [1] "one" "two" "three" "four" "five" "six"

die
## one two three four five six
## 1 2 3 4 5 6

#删除names属性
names(die) <- NULL
```

Dim属性可以将向量转换成n维的数组(或2维的矩阵)

```r
dim(die) <- c(2, 3)
die
##    [,1] [,2] [,3]
## [1,] 1    3    5
## [2,] 2    4    6
```

###矩阵

```r
m <- matrix(die, nrow = 2, byrow = FALSE)#按列将die向量填入2行的矩阵中
m
##    [,1] [,2] [,3]
## [1,] 1    3    5
## [2,] 2    4    6
```

###数组

```r
ar <- array(c(11:14, 21:24, 31:34), dim = c(2, 2, 3))#第一个参数是向量，第二个参数dim，为3维数组
ar
## , , 1
##
##    [,1] [,2]
## [1,] 11   13
## [2,] 12   14
##
## , , 2
```

`class`函数可以查看变量的类型，返回结果是"matrix"，"character"之类。

```r
dim(die) <- c(2, 3)
typeof(die)
## "double"
class(die)
## "matrix"
```

###日期和时间

日期以字符串方式显示，但却是浮点类型，代表从12:00 AM January 1st 1970到现在的秒数

```r
now <- Sys.time()
now
## "2014-03-17 12:00:00 UTC"
typeof(now)
## "double"

unclass(now)
## 1395057600

#将秒数格式化为对应的日期
mil <- 1000000
mil
## 1e+06
class(mil) <- c("POSIXct", "POSIXt")#日期的class属性
mil
## "1970-01-12 13:46:40 UTC"
```

###因子

因子用来储存拥有固定值的信息，如人的性别只有两种，男和女。因此因子的值是几个值中之一。

```r
gender <- factor(c("male", "female", "female", "male"))
typeof(gender)
## "integer"
attributes(gender)
## $levels   说明gender只有两种值"female" "male"
## [1] "female" "male"
##
## $class
## [1] "factor"
```

使用`unclass`来查看`R`是如何储存factor

```r
unclass(gender)
## [1] 2 1 1 2    默认下"female"设为1，"male"设为2
## attr(,"levels")
## [1] "female" "male"
```

###List列表

list可以以数组的方式储存不同类型的数据(元素下标从1开始)

```r
list1 <- list(100:130, "R", list(TRUE, FALSE))
list1
## [[1]]
##  [1] 100 101 102 103 104 105 106 107 108 109 110 111 112
## [14] 113 114 115 116 117 118 119 120 121 122 123 124 125
## [27] 126 127 128 129 130
##
## [[2]]
## [1] "R"
##
## [[3]]
## [[3]][[1]]
## [1]   TRUE
##
## [[3]][[2]]
## [1]   FALSE
```

###数据框

数据框就像是2维的列表，就像是Excel中的表格，所以每一列都有一个名称，接着就是相应的数据。但每一列的数据长度必须相同。

```r
#使用data.frame()函数来创建数据框
df <- data.frame(face = c("ace", "two", "six"), suit = c("clubs", "clubs", "clubs"), value = c(1, 2, 3))
#创建了一个有3列数据的数据框，可以在函数中加入更多的数据来创建更多的列
df
## face suit value
## ace clubs 1
## two clubs 2
## six clubs 3
```

还可以使用`str()`函数来查看数据框或列表中都有哪些类型的数据。默认情况下`R`会将所有字符串全都以因子`factor`来存储。可以使用`stringsAsFactors = FALSE`关掉。

###Loading Data

