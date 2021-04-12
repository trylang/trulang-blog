---
title: mongoDB学习
date: 2021-04-12 15:54:03
tags:
  - mongoDB
categories: 平时笔记
---

# mongoDB学习

## 一、mac 安装

### 1. 使用 brew 安装

此外你还可以使用 OSX 的 brew 来安装 mongodb：

```
brew tap mongodb/brew
brew install mongodb-community@4.4
```

**@** 符号后面的 **4.4** 是最新版本号。

安装信息：

- 配置文件：**/usr/local/etc/mongod.conf**
- 日志文件路径：**/usr/local/var/log/mongodb**
- 数据存放路径：**/usr/local/var/mongodb**

### 2. 安装可视化工具

![image-20210412160302147](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412160303.png)

![image-20210412160445127](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412160459.png)

## 二、运行mongoDB

我们可以使用 brew 命令或 mongod 命令来启动服务。

brew 启动：

```
brew services start mongodb-community@4.4
```

brew 停止：

```
brew services stop mongodb-community@4.4
```

mongod 命令后台进程方式：

```
mongod --config /usr/local/etc/mongod.conf --fork
```

这种方式启动要关闭可以进入 mongo shell 控制台来实现：

```
> db.adminCommand({ "shutdown" : 1 })
```

如果localhost端口27017，能够正常访问页面，如下图，则说明mongoDB连接成功。

![image-20210412160750467](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412160759.png)

## 三、操作mongoDB

### 1. 集合操作

##### 3.1.1 新增

Insert：如果新增数据存在与库中，会报错；

Save:  如果新增数据存在与库中，不会更新，不报错；

##### 3.1.2 更新

```sql
db.users.update({age: 10}, {$set: {name: 'haha', age:1}}, {multi: true, } )
```

更新需要 使用$set，防止误操作，预防修改的对象整体覆盖之前的整条数据。

- Multi: 设置为true时，可以多条修改。这时必须使用$set才能进行更新；如果Multi为false时，可以不加`\$set` ，整条数据全部覆盖。

##### 3.1.3 自增某字段

```sql
db.users.update({}, {$inc: {age: 1}}, {multi: true})
```

- {}: 针对当前集合，任意数据都自增 age:1；

- 如果muilt为false，默认第一条；

- 如果集合之前有age字段，则+1，如果之前没有，则新增age为1；

  ![image-20210412182740953](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412182750.png)

##### 3.1.4 删除某字段

```sql
db.users.update({}, {$unset: {age: 1}}, {multi: true})
```

- {}: 针对当前集合；

- 如果muilt为false，默认第一条；

- $unset: 删除集合中age的字段；

  ![image-20210412183515645](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412183524.png)

##### 3.1.4 向数组中添加某字段

```sql
db.users.update({}, {$push: {hobby: 'reading'}}, {multi: true})
```

- {}: 针对当前集合；
- 如果muilt为false，默认第一条；
- $push: 在hobby数组中，添加字段reading；

![image-20210412183908865](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412183910.png)

##### 3.1.5 $ne

类似mySql的 `not in` 或者`not exists`。

```sql
db.users.update({name: 'haha', hobby:{$ne: 'reading'}}, {$push: {hobby: 'reading'}})
```

- hobby数组中，没有reading字段的，会添加reading，有reading字段的则不添加，保证数据唯一性。 

![image-20210412192810692](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412192813.png)

##### 3.1.6 $addToSet

向集合中添加元素。

```sql
db.users.update({}, {$addToSet: {hobby: 'reading'}}, {multi: true})
```

- addToSet：功能类似$ne，但是比它好使。新增的字段存在则不添加，不存在则会新增。

![image-20210412193037919](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412193040.png)