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

##### 1.1.1 新增

Insert：如果新增数据存在与库中，会报错；

Save:  如果新增数据存在与库中，不会更新，不报错；

#### 1.1.2 删除整个文档

```sql
db.users.drop();
```

### 2. 文档更新

##### 2.1.2 更新

```sql
db.users.update({age: 10}, {$set: {name: 'haha', age:1}}, {multi: true, } )
```

更新需要 使用$set，防止误操作，预防修改的对象整体覆盖之前的整条数据。

- Multi: 设置为true时，可以多条修改。这时必须使用$set才能进行更新；如果Multi为false时，可以不加`\$set` ，整条数据全部覆盖。

##### 2.1.3 自增某字段

```sql
db.users.update({}, {$inc: {age: 1}}, {multi: true})
```

- {}: 针对当前集合，任意数据都自增 age:1；

- 如果muilt为false，默认第一条；

- 如果集合之前有age字段，则+1，如果之前没有，则新增age为1；

  ![image-20210412182740953](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412182750.png)

##### 2.1.4 删除某字段

```sql
db.users.update({}, {$unset: {age: 1}}, {multi: true})
```

- {}: 针对当前集合；

- 如果muilt为false，默认第一条；

- $unset: 删除集合中age的字段；

  ![image-20210412183515645](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412183524.png)

##### 2.1.5 向数组中添加某字段

```sql
db.users.update({}, {$push: {hobby: 'reading'}}, {multi: true})
```

- {}: 针对当前集合；
- 如果muilt为false，默认第一条；
- $push: 在hobby数组中，添加字段reading；

![image-20210412183908865](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412183910.png)

##### 2.1.6 $ne

类似mySql的 `not in` 或者`not exists`。

```sql
db.users.update({name: 'haha', hobby:{$ne: 'reading'}}, {$push: {hobby: 'reading'}})
```

- hobby数组中，没有reading字段的，会添加reading，有reading字段的则不添加，保证数据唯一性。 

![image-20210412192810692](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412192813.png)

##### 2.1.7 $addToSet

向集合中添加元素。

```sql
db.users.update({}, {$addToSet: {hobby: 'reading'}}, {multi: true})
```

- addToSet：功能类似$ne，但是比它好使。新增的字段存在则不添加，不存在则会新增。

![image-20210412193037919](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210412193040.png)

##### 2.1.8 $each

在集合里遍历数组中元素。

```sql
const hobbies = ['read', 'write'];
db.users.update({}, {$addToSet: {hobby: {$each: hobbies}}})
```

- addToSet：功能类似$ne，但是比它好使。新增的字段存在则不添加，不存在则会新增。
- 如果直接写成`{hobby: hobbies}`的形式，那hobby就成了二维数组，数组套数组，显然无法满足hobbies展开放入数组hobby中的需求。

![image-20210413105950563](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210413105952.png)

##### 2.1.9 $pop

在集合里删除数组中元素。

```sql
db.users.update({_id: 1}, {$pop: {hobby: {hobby: 1}}})
```

- 1是数组中索引，也就是删除了第二项write。

![image-20210413110810294](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210413110811.png)

##### 2.1.9 $set

在集合里更新数组中某元素。

```sql
db.users.update({_id: 1}, {$set: {'hobby.0': 'reading'}})
```

- `hobby.0`：代表bobby数组中第1项，read 改成reading。

  

![image-20210413111128133](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210413111129.png)

### 3. 文档删除

```
db.users.drop();
```

- 删除users集合中的所有数据；

```sql
db.users.remove({name: 'haha'});
```

- Remove: 会删除多条文档；

```
db.users.remove({name: 'haha', {justOne: true}});
```

- `{justOne: true}`：只删除第一条满足条件的文档；

### 4. 查询文档

#### 4.1 Find()

返回具体某些字段;

```sql
db.users.find({}, {name: 1});
```

- _id：它特殊，不具体要求是否显示，则都显示。
- ​	`name: 1`：1表示显示此字段，0表示不显示。如此表中只显示name这一字段。

![image-20210413115207227](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210413115209.png)

- `name: 0`：除name之外，其他字段都显示。

  ![image-20210413115556726](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210413115558.png)

- 最好不要0 和 1混着写，不然容易造成如下图的错误。

  ![image-20210413115940097](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210413115941.png) 

#### 4.2 findOne()

只返回满足条件的第一条数据。

![image-20210413120221627](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210413120222.png)

![image-20210413120639003](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210413120640.png)

#### 4.3 $in

在5和10范围内。 

```
db.users.find({age: {$nin: [5,7,9]}})
```

![image-20210414102338295](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210414102340.png)

#### 4.4 $nin

除了5和10之外所有的。

#### 4.5 `$gt` \ `$lt`

$gt: 大于；

$lt: 小于；

```
cdb.users.find({age: {$gt: 5, $lt: 10}});
```

![image-20210414102644326](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210414102659.png) 

#### 4.6 $not

$not：取反；

```
cdb.users.find({age: {$not: {$gt: 5, $lt: 10}}});
```

#### 4.7 $all

$all：查询 包含；【包含关系】

```
db.users.find({friends: {$all: ['A', 'B']}});
```

- 查询只要包含A和B就可以的文档；

```
db.users.find({friends: ['A', 'B']});
```

- 查询friends同时包含A，B的文档；【且关系】

#### 4.8 $in

$in：查询 包含A 或者B都可；【或关系】

```
db.users.find({friends: {$in: ['A', 'B']}});
```

- 查询只要包含A或者B就可以的文档；

#### 4.9 $size

$size：查询数组中length = size 的文档；

```
db.users.find({friends: {$size: 2}});
```

![image-20210414103727053](/Users/jane/Library/Application Support/typora-user-images/image-20210414103727053.png)

#### 4.9 $slice

$slice：查询数组截取后的文档；

```
db.users.find({friends: {$size: 4}}, {friends: {$lice: 2}});
```

- 截取数组中前两项数据；

![image-20210414104134330](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210414104242.png)

#### 4.10 $where

$where：查询当前文档的数据，比较灵活；

```
db.users.find({$where: "this.age>30"}, {name: 1, age: 1});
```

- this指向当前文档。

## 四、执行脚本

![image-20210414110537839](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210414110539.png)

## 五、备份与导出

### 5.1 备份

```
> mongodump --host 127.0.0.1 --port 27017 --out ./backup --collection users --db school 
```

将数据库为school中的users集合，备份到./backup的文件目录中。

- --post 指定机器；
- -- port 指定端口；
- -- out 指定要备份的文件目录；
-  -- collection 指定要备份的集合名字;
- --db 指定要备份的数据库名字；

 

![image-20210414110323909](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210414110325.png)

### 5.2 恢复备份

```
> mongorestore ./backup
```

### 六、权限



![image-20210414111913113](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210414111915.png)

![image-20210414112143005](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210414112144.png)

## 七、建立索引

```
db.users.ensureIndex({age: 1});
```

- ensureIndex：将age建立索引；
- 1： 升序；