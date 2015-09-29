---
layout: post
title: Erlang
category: Erlang
keywords: ""
published: false
---

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

## Pattern Matching

Pattern matching in Erlang is used to:

- Assign values to variables {A, B} = {square, side}
- Control the execution flow of programs
- Extract values from compound data types

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

