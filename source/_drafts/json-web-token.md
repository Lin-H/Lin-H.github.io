title: json-web-token
tag:
- Node.js
category: Node.js
keywords: "authentication,node.js,json,token,jwt"
banner:
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

因为前后端分离的缘故，现在的后台多数只提供数据部分，一般使用`JSON`格式，所以`JSON Web Token`是比较流行的认证方式。
<!-- more -->

