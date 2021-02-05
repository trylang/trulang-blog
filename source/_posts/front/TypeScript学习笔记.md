---
layout: page
title: TypeScript：学习笔记
date: 2021-02-5 09:06:21
tags: 
  - 前端JS
  - 前端基础
categories: front

---

#  TypeScript学习笔记

> 重点掌握：
>
> 1. 变量使用的原始值与引用值;
> 2. 理解执行上下文;
> 3. 理解垃圾回收；

## 一、定义

TypeScript是JavaScript的超集。

## 二、优势

### 3. 代码语义更清晰易懂；

## 三、类型

### 1. 基础定义

![image-20210205092025726](/Users/jane/Library/Application Support/typora-user-images/image-20210205092025726.png)

![image-20210205092328935](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210205092339.png)

### 2. 解构赋值的注解

![image-20210205093258540](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210205093300.png)

### 3. 函数定义时有两种方法约束参数和返回值类型：

第一种是约束参数和返回值；这种写法返回值类型number可省略不写，编辑器可直接推断出类型；

第二种是直接约束函数的方式：这种写法返回值类型number不能省略，因为省略结构就不对了；

![image-20210205093800508](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210205093803.png)

### 4. 接口interface  与 类型别名type的区别

interface和type都可以定义，但interface适用于定义对象或者函数，不能定义简单类型。type什么类型都可以。能用接口定义表示就用接口interface。

![image-20210205094754620](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210205094757.png)

### 5. 注意点：函数传参是字面量对象，会进行强校验；如果是之前定义好对象，再传参，则是弱校验。

![image-20210205100525259](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210205100526.png)

![image-20210205100037293](/Users/jane/Library/Application Support/typora-user-images/image-20210205100037293.png)

