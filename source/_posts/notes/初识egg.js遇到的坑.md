---
layout: page
title: 初识egg.js遇到的坑
date: 2021-02-10 10:14:12
tags: 
  - egg.js
categories: 平时笔记 
---

# 初识egg.js遇到的坑

## 1. Failed to connect to localhost port 8080: Connection refused

使用命令`curl http://localhost:7001` 会报Connection refused。

这时使用 `curl http://127.0.0.1:7001`，就没有问题了。

> 资料参考：
>
> [Linux curl命令详解](https://www.cnblogs.com/duhuo/p/5695256.html)

## 2.  Connect timeout for 5000ms, GET https://hacker-news.firebaseio.com/v0/topstories.json -2

Egg.js例子里有些链接，浏览器是可以打开的，但是直接在egg.js中使用`await this.ctx.curl(url,{})`时会报超时。

自己没有找到特别好的办法，只是换了一个都能访问的请求数据地址，替换成这个地址就不报超时的错误了。

> [可访问地址](https://api.stackexchange.com/2.2/search?order=desc&amp;sort=activity&amp;intitle=perl&amp;site=stackoverflow)

------

## 3. 报错“nodejs.RequestError: getaddrinfo ENOTFOUND”

或者“agent status: {"createSocketCount":1,"createSocketErrorCount":0,"closeSocketCount":1,"errorSocketCount":0,"timeoutSocketCount":0,"requestCount":0,"freeSockets":{},"sockets":{},"requests":{}}, socketHandledRequests: 1, socketHandledResponses: 0)” 

这时要检查下代码是否有问题，我当时就是把配置文档写错了。

当时按照egg.js文档中，是这样写的，结果会报上述错误。![image-20210210104350165](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210210104400.png)

在config/plugin.js 文件中，应该要如此引入模版，不会报错。

```javascript
module.exports = {
  nunjucks: {
    enable: true,
    package: 'egg-view-nunjucks',
  },
  // had enabled by egg
  // static: {
  //   enable: true,
  // }
};
```

## 4. 解决Node.js mysql客户端不支持认证协议引发的“ER_NOT_SUPPORTED_AUTH_MODE”问题。

> 参考资料：
>
> [解决Node.js mysql客户端不支持认证协议引发的“ER_NOT_SUPPORTED_AUTH_MODE”问题](https://waylau.com/node.js-mysql-client-does-not-support-authentication-protocol/)

### 报错信息

```javascript
D:\workspaceGithub\nodejs-book-samples\samples\mysql-demo\index.js:17
      throw error;
      ^

Error: ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client
    at Handshake.Sequence._packetToError (D:\workspaceGithub\nodejs-book-samples\samples\mysql-demo\node_modules\mysql\lib\protocol\sequences\Sequence.js:47:14)
    at Handshake.ErrorPacket (D:\workspaceGithub\nodejs-book-samples\samples\mysql-demo\node_modules\mysql\lib\protocol\sequences\Handshake.js:123:18)
    at Protocol._parsePacket (D:\workspaceGithub\nodejs-book-samples\samples\mysql-demo\node_modules\mysql\lib\protocol\Protocol.js:291:23)
    at Parser._parsePacket (D:\workspaceGithub\nodejs-book-samples\samples\mysql-demo\node_modules\mysql\lib\protocol\Parser.js:433:10)
    at Parser.write (D:\workspaceGithub\nodejs-book-samples\samples\mysql-demo\node_modules\mysql\lib\protocol\Parser.js:43:10)
    at Protocol.write (D:\workspaceGithub\nodejs-book-samples\samples\mysql-demo\node_modules\mysql\lib\protocol\Protocol.js:38:16)
    at Socket.<anonymous> (D:\workspaceGithub\nodejs-book-samples\samples\mysql-demo\node_modules\mysql\lib\Connection.js:91:28)
    at Socket.<anonymous> (D:\workspaceGithub\nodejs-book-samples\samples\mysql-demo\node_modules\mysql\lib\Connection.js:525:10)
    at Socket.emit (events.js:196:13)
    at addChunk (_stream_readable.js:290:12)
    --------------------
```

### 出错原因

导致这个错误的原因是，目前，最新的mysql模块并未完全支持MySQL 8的“caching_sha2_password”加密方式，而“caching_sha2_password”在MySQL 8中是默认的加密方式。因此，下面的方式命令是默认已经使用了“caching_sha2_password”加密方式，该账号、密码无法在mysql模块中使用。

```mysql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
Query OK, 0 rows affected (0.12 sec)
```

### 解决方法

解决方法是从新修改用户root的密码，并指定mysql模块能够支持的加密方式：

```mysql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
Query OK, 0 rows affected (0.12 sec)
```

上述语句，显示指定了使用“mysql_native_password”的加密方式。这种方式是在mysql模块能够支持。