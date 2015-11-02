---
layout: post
title: Erlang
category: Erlang
keywords: ""
published: false
---

接触`Erlang`的最主要目的就是因为`Erlang`的并发性能，据说是最强的，WhatsApp就靠着`Erlang`50个人就完成了WhatsApp的整个开发，除此之外在并发上还有`Rust`和`Node.js`，想看看`Erlang`的并发性能是否真的这么神奇。

### Tips

windows 使用`werl.exe`可以使用`Tab`自动补全，`ih()`查看`i module`的帮助，更多帮助使用`help()`

每条语句以`.`结尾

`base#value`，进制#数值 返回值为10进制的数

### Atoms

字面上的常量，单引号包裹`Erlang is fun`，小写字母`@`,`_`都是有效的`Atoms`

### Boolean

用`Atoms`表示`true`和`false`

- and
- andalso
- or
- orelse
- xor
- not

### Equality

- ==	Equal to
- /=	Not equal to
- =:=	Exactly equal to
- =/=	Exactly not equal to
- =<	Less than or equal to
- <		Less than
- >=	Greater than or equal to
- >		Greater than

### Variables

变量名以大写字体开头，可包括数字和下划线`_`

### tuple

`Tuple`的元素的类型可以不同，Examples of tuples are:

- {foo, bar, 123}
- {employee, 'Roberto', 'Aloi'}

### list

Examples of lists are:

- [january, february, march]
- [123, pigeon, [a,b,c]]

`String`也是`list`，可以使用`[H|T]`来取出`list`中的头部和尾部

```erlang
[97, 98, 99]. %会输出 "abc"
[97, 98, 99, 1]. %就会原样输出，因为有一个1无法装换成字符
```

`++`连接`list`,`--`减去相同的一个子串

```erlang
[2 * X || X <- L].  % [2,4,6,8,10]
EvenNumbers = [N || N <- [1, 2, 3, 4], N rem 2 == 0]. % [2, 4]
[ X || <<X>> <= <<1,2,3,4,5>>, X rem 2 == 0].  % [2, 4]  
```

## Pattern Matching

Pattern matching in Erlang is used to:

- Assign values to variables {A, B} = {square, side}
- Control the execution flow of programs
- Extract values from compound data types

使用`_`来匹配任意值，但不使用

```erlang
Point = {point, 10, 45}.
{_, Ten, _} = Point.  % Ten 为10
```

## Function

```erlang
    area({square, Side}) ->
      Side * Side;
    area({circle, Radius}) ->
      math:pi() * Radius * Radius;
    area(Other) ->
      {error, unknown_object}. 
```

根据`Pattern matching`来判断该调用那个函数，使用`MODULE:FUNCTION(ARGS)`来调用模块中的函数。在`Erlang`中也有像`Haskell`的`list`使用方法

```erlang
[X * 2 || X <- [1, 2, 3, 4, 5], X rem 2 == 0].
```

函数的定义`Name(Args) -> Body.`,`Name`是`atom`,`Body`可以是多个表达式，用逗号分开。最后一个表达式的值当做返回值返回。

### when

与`if`类似,同时满足多个条件用逗号分隔, 满足其中之一用分号分隔

```erlang
old_enough(X) when X >= 16 -> true;
old_enough(_) -> false.
```

### if

```erlang
help_me(Animal) ->
    Talk = if Animal == cat  -> "meow";
              Animal == beef -> "mooo";
              Animal == dog  -> "bark";
              Animal == tree -> "bark";
              true -> "fgdadfgna"
    end,
    {Animal, "says " ++ Talk ++ "!"}.
```

### case ... of

```erlang
case {A,B} of
Pattern Guards -> ...
beach(Temperature) ->
    case Temperature of
        {celsius, N} when N >= 20, N =< 45 ->
            'favorable';
        {kelvin, N} when N >= 293, N =< 318 ->
            'scientifically favorable';
        {fahrenheit, N} when N >= 68, N =< 113 ->
            'favorable in the US';
        _ ->
            'avoid beach'
    end.
```

### Bit Syntax

```erlang
Color = 16#F09A29. %15768105
Pixel = <<Color:24>>. %<<240,154,41>>  以24位分开
<<R:8, Rest/binary>> = Pixels.  % 只取前8位
```

- Value
- Value:Size
- Value/TypeSpecifierList
- Value:Size/TypeSpecifierList

type `integer | float | binary | bytes | bitstring | bits | utf8 | utf16 | utf32`
Signedness `signed | unsigned`
Endianness `big | little | native`

```erlang
<<X2/signed>> =  <<-44>>. % <<"Ô">>
<<X2/integer-signed-little>> =  <<-44>>. % <<"Ô">>
<<N:8/unit:1>> = <<72>>. % <<"H">>
N. % 72
<<N/integer>> = <<72>>. % <<"H">>
```

### Module

```erlang
-module(useless).
-export([add/2, Function/2]). % 导出相应的函数
```

### Macro

定义宏`-define(MACRO, some_value).`与C语言很像，使用宏`?MACRO`

```erlang
-define(sub(X,Y), X-Y).
?sub(23,47)
```


