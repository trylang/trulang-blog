---
layout: page
title: egg.js简单梳理笔记
date: 2021-02-10 11:02:46
tags:
- egg.js
categories: 平时笔记  
---

# egg.js简单梳理笔记

## 一、目录结构

详见[目录结构](https://eggjs.org/zh-cn/basics/structure.html)

------

## 二、内置对象

详见[框架内置基础对象](https://eggjs.org/zh-cn/basics/objects.html)

框架中内置的一些基础对象，包括从 [Koa](http://koajs.com/) 继承而来的 4 个对象（Application, Context, Request, Response) 以及框架扩展的一些对象（Controller, Service, Helper, Config, Logger）。

### 1. Application

Application 是全局应用对象，在一个应用中，只会实例化一个，继承自[Koa.Application](http://koajs.com/#application)。和 [Koa](http://koajs.com/) 一样，在 Context 对象上，可以通过 `ctx.app` 访问到 Application 对象。

### 2. Context

Context 是一个**请求级别的对象**，继承自 [Koa.Context](http://koajs.com/#context)。在每一次收到用户请求时，框架会实例化一个 Context 对象，这个对象封装了这次用户请求的信息，并提供了许多便捷的方法来获取请求参数或者设置响应信息。框架会将所有的 [Service](https://eggjs.org/zh-cn/basics/service.html) 挂载到 Context 实例上，一些插件也会将一些其他的方法和对象挂载到它上面（[egg-sequelize](https://github.com/eggjs/egg-sequelize) 会将所有的 model 挂载在 Context 上）。

#### 获取方式

最常见的 Context 实例获取方式是在 [Middleware](https://eggjs.org/zh-cn/basics/middleware.html), [Controller](https://eggjs.org/zh-cn/basics/controller.html) 以及 [Service](https://eggjs.org/zh-cn/basics/service.html) 中。Controller 中的获取方式在上面的例子中已经展示过了，在 Service 中获取和 Controller 中获取的方式一样，在 Middleware 中获取 Context 实例则和 [Koa](http://koajs.com/) 框架在中间件中获取 Context 对象的方式一致。

### 3. Request & Response

Request 是一个**请求级别的对象**，继承自 [Koa.Request](http://koajs.com/#request)。封装了 Node.js 原生的 HTTP Request 对象，提供了一系列辅助方法获取 HTTP 请求常用参数。

Response 是一个**请求级别的对象**，继承自 [Koa.Response](http://koajs.com/#response)。封装了 Node.js 原生的 HTTP Response 对象，提供了一系列辅助方法设置 HTTP 响应。

#### 获取方式

可以在 Context 的实例上获取到当前请求的 Request(`ctx.request`) 和 Response(`ctx.response`) 实例。

```javascript
// app/controller/user.js
class UserController extends Controller {
  async fetch() {
    const { app, ctx } = this;
    const id = ctx.request.query.id;
    ctx.response.body = app.cache.get(id);
  }
}
```

- [Koa](http://koajs.com/) 会在 Context 上代理一部分 Request 和 Response 上的方法和属性，参见 [Koa.Context](http://koajs.com/#context)。
- 如上面例子中的 `ctx.request.query.id` 和 `ctx.query.id` 是等价的，`ctx.response.body=` 和 `ctx.body=` 是等价的。
- 需要注意的是，获取 POST 的 body 应该使用 `ctx.request.body`，而不是 `ctx.body`。

### 4. 待续。。。



### 三、插件

#### 1. 校验参数

`egg-validate`;

### 2. 简单的mySql处理插件

`egg-mysql`;

### 3. 运用于复杂、多表联调的mySql插件

`egg-sequelize`; `musql2`

```powershell
yarn add egg-sequelize mysql2
```

