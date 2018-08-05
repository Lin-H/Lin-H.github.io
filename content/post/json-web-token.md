---
title: Json Web Token身份认证
tag:
- Node.js
categories: 
- Node.js
date: 2016-01-20 22:38:56
keywords: "authentication,node.js,json,token,jwt"
banner: http://linhsblog-10013469.image.myqcloud.com/images/jwt-diagram.png
---

用户身份认证一般有5种方式

- HTTP Basic authentication<br/>
在发送请求时在HTTP头中加入`authentication`字段，将用`Base64`编码的用户名和密码作为值，每次发送请求的时候都要发送用户名和密码，实现比较简单。

- Cookies<br/>
向后台发送用户名和密码，在用户名和密码通过验证后，保存返回的`Cookie`作为用户已经登录的凭证，每次请求时附带这个`Cookie`

- Signatures<br/>
用户拿到服务器给的私钥，在发送请求前，将整个请求使用私钥来加密，发送的将是一串加密信息，此方式只适用于API

- One-Time Passwords<br/>
一次一密，每次登录时使用不同的密码，一般由服务端通过邮件将密码发给用户，这种登录方式比较繁琐

- JSON Web Token<br/>
用户发送按照约定，向服务端发送`Header`、`Payload`和`Signature`，并包含认证信息(密码)，验证通过后服务端返回一个`token`，之后用户使用该`token`作为登录凭证，适合于移动端和api

<!--more-->

因为前后端分离的缘故，现在的后台多数只提供数据部分，一般使用`JSON`格式，所以`JSON Web Token`是比较流行的认证方式。

`JWT`的认证方式相比其他的认证方式有一下优点：
- 信息可用HMAC或RSA加密，信息安全性较高
- 生成的密文短，密文可以包含所有用户信息，认证过期时间或用户权限等自定义信息
- 适合用于手机应用和单页面应用的身份认证
- 使用灵活，一旦取得了`JWT`，可以通过POST方式或添加入HTTP头中发送

### JWT结构

`JWT`包含3个部分

- Header (头部)
- Payload (负载)
- Signature (签名)

#### Header头部

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

`JWT`的头部是固定的，`alg`是算法的意思表示该`JWT`使用的是何种算法加密。`typ`字段值是固定的`JWT`

#### Payload

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

负载部分就是具体的认证信息，通过修改这部分的内容来控制认证信息如用户权限等。除了一些保留字段`exp`(过期时间)、`aud`、`iss`等外，使用方法跟普通Json一样。

#### Signature

签名，也就是密钥，用来保证密文的安全强度

以上3部分都经过[Base64Url]()处理后用`.`分隔再使用`HMAC SHA256`或`RSA`加密为一段字符串
```js
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```
具体的使用在[JWT.IO](http://jwt.io/)上有演示

### JWT使用流程

![jwt diagram](http://linhsblog-10013469.image.myqcloud.com/images/jwt-diagram.png)

客户端POST用户名和密码到服务端，若对安全要求较高可以是加密后的用户名或密码，服务端把拿到的用户名和密码与数据库中的对比，若相同则按照上面的流程生成`JWT`，然后返回客户端。在此之后客户端的所有请求，可以在Authorization HTTP头或POST数据中附带得到的`JWT`。服务端验证`JWT`并解析出Payload部分，以此来判断用户的权限。

`JWT`的使用方法很简单，就拿node.js的包`node-jsonwebtoken`来说加密和验证就两个函数`jwt.sign`，`jwt.verify`并且[jwt.io](http://jwt.io)中提供了很多语言的`JWT`包。

### 参考链接

- [http://jwt.io/introduction/](http://jwt.io/introduction/)
- [http://jwt.io/#libraries](http://jwt.io/#libraries)
- [node-jsonwebtoken](https://github.com/auth0/node-jsonwebtoken)
