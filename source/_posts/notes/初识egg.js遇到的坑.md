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

[可访问地址]: https://api.stackexchange.com/2.2/search?order=desc&amp;sort=activity&amp;intitle=perl&amp;site=stackoverflow



## 3. 报错“nodejs.RequestError: getaddrinfo ENOTFOUND”，或者“agent status: {"createSocketCount":1,"createSocketErrorCount":0,"closeSocketCount":1,"errorSocketCount":0,"timeoutSocketCount":0,"requestCount":0,"freeSockets":{},"sockets":{},"requests":{}}, socketHandledRequests: 1, socketHandledResponses: 0)” 

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

