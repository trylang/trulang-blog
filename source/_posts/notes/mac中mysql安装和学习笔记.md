---
layout: page
title: mac中mysql安装和学习笔记
date: 2021-02-12 19:51:18
tags:
- mysql
categories: 平时笔记 
---

# mac中mysql安装和学习笔记

## 安装链接

### 1. [MySQL安装（Mac版）](https://juejin.cn/post/6844903831298375693) 

[MySQL Community (GPL) Downloads »](https://dev.mysql.com/downloads/)

### 2. 安装Mysql可视化工具，

[MySQL Workbench](https://dev.mysql.com/downloads/workbench/)



## 报错解决方案

### 1. [Mac OS zsh: command not found: mysql解决方案](https://segmentfault.com/a/1190000020656076)

### 2. [报错：Access denied for user 'xxx'@'localhost'](https://segmentfault.com/a/1190000012655875)

一个是看mysql是否连接；另一个就是看配置项是否写正确。

> 自己写项目时，就是将配置项复制错了，在eggjs项目中config.default.js文件中，设置简单版的是config.mysql，复杂版的是config.sequelize，当时自己就是config.sequelize错写成了config.mysql导致这个错误发生。

### 3. [Mac Navicat 出现 2003 - Can't connect to MySQL server on '127.0.0.1' (61 "Connection refused")](https://blog.csdn.net/Code_Nice/article/details/81121565)

![img](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210214001352.jpeg)

### 4. [Mac使用Navicat连接mysql时报1045 – Access denied for user ‘root’@’localhost’ (using password: YES)错误](https://www.codenong.com/cs105167619/)



## 学习笔记

### 1. 进入mysql；

`mysql -u root -p`

### 2. 显示mysql数据库；

`show databases;`

### 3. 创建数据库；

`create database egg;`

### 4. 数据库和表的增删改查操作

![企业微信截图_8d8899b1-c1f9-4037-b05e-ab3c6bb2cde1](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210212212509.png)

![image-20210212215521515](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210212215546.png)

