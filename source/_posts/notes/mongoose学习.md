---
title: mongoose学习
date: 2021-04-14 11:45:36
tags:
  - mongoDB
categories: 平时笔记
---

# mongoose 学习

## 一、Mongoose是什么

nodejs操作Mongo

## 二、使用Mongoose

### 2.1 安装 

项目中 `yarn add mongoose`;

### 2.2 使用

```js
// 1.js
const mongoose = require('mongoose');
// mongoose 是一个ORM工作，就是对象模型工具，可以让我们像操作对象一样操作数据库
// school: 为连接的数据库名
const conn = mongoose.createConnection('mongodb://localhost:27017/school', {useNewUrlParser: true});
conn.on('error', function(err) => {
        console.log(err);
        });
conn.on('open',function(err) => {
        console.log('数据库连接成功');
        });

// 1. 创建一个集合的Schema，Schema中规定了集合的文档的属性名和属性类型
 let UserSchema = new mongoose.Schema({
   name: Stirng,
   age: Number
 })  
 // 2. 基于Schema创建模型,返回的是一个对象实例， 'User'为模型名字
 let UserX = conn.model('User', UserSchema); // 用UserSchema来定义模型
 // conn.model('User') 是取出模型
console.log(conn.model('User') === UsersX) // true

```

创建数据

```js
let users = [];
for(let i = 1; i <= 10; i++){
	users.push({id: i, name: `haha-${i}`});
}
// promise then方式
UserX.create(users).then(function(docs) {
  console.log(docs);
});

// async await
(async function() {
  let docs = await UserX.create(users);
  console.log(docs);
})()

```

### 2.3 保存

两种保存方式。

```js
// 3. UserX这个模型有多个方法，可以操作数据库
// Create：创建文档并保存；
// 这里传递的字段比Schema中定义的多，会被忽略掉，字段以Schema为准。
// 如果比Schema定义字段少，则有几个保存几个。
UserX.create({name: 'haha', age: 18}, function(err, doc) {
  // err 错误对象 ；doc 是保存后的文档对象
  doc.name = 'haha1';
  doc.save(); // 这里也可以调用save进行文档修改后的保存
})

// 4. 第二种保存文档的方法：【除了create，还有save】
// entity：实体，实体代表某一个实例、个体
// UserX也是一个构造函数，可以创建实例，创建出来的实例，我们称entity
let user = new UserX({
  name: 'haha',
  age: 16
});
user.save(function(err, doc) {
  console.log(doc);
  doc.age = 23;
  doc.save(); // 还可以针对修改的值，继续保存
}); // 保存文档

        
```

### 2.4 查询

```js
UserX.find({}, function(err, docs) {console.log(docs)});

// 2. {name: 0}：0表示不显示，即查询所有数据中除name字段之外的字段。
UserX.find({}, {name: 0}, function(err, docs) {
  console.log(docs);
})

// 查ID
UserX.findbyId('xxx', function(err, docs) {
  console.log(docs);
})

 // 查范围：$gt: 大于；$lt: 小于
UserX.find({age: {$gt:3, $lt: 6}}, {name: 0}, function(err, docs) {
  console.log(docs);
})

// 判断某个字段是否存在
UserX.find({age: {$exist: true}}, function(err, docs) {
  console.log(docs);
})

// 分页
let pageNum = 2;
let pageSize = 3;
// 1. 知道一共多少条；2. 拿到当页的对象列表
UserX.find().sort({age: 1}).skip((pageNum-1) * pageSize).limit(pageSize).exec((err, docs) => {
  console.log(docs); 
});

// 总数
UserX.count(function(err, total) {
  console.log(total)
})

```

### 2.5 更新

Update: 即使没有使用更新操作符$set, 也是只更新指定的字段，不会整体覆盖。`$set`可用可不用。

```js
UserX.update({name: 'ha'}, {age: 11}, function(err, result) {
	console.log(result);
	// ok 1为true，0为false
})

// 使用$set的方式
UserX.update({name: 'ha'}, {$set: {age: 112}}, function(err, result) {
	console.log(result);
	// ok 1为true，0为false
})
```

### 2.6 删除

```js
// 删除1条
UserX.deleteOne({name: 'ha'}, function(err, result) {
	console.log(result);
	// ok 1为true，0为false
})

// 删除多条
UserX.deleteMany({name: 'ha'}, function(err, result) {
	console.log(result);
	// ok 1为true，0为false
})
```

### 2.7 扩展类上的静态方法

```js
UserSchema.statics.login = function(username, password) {
  return this.findOne({username, password})
}
```

### 2.8 扩展实体或者实例的方法 Entity

在类上还是实例上扩展，关键要看这个操作是针对集合还是单个实例。

```js
UserSchema.methods.login = function() {
  return this.model('UserX').findOne({
    username: this.username,
    password: this.password
  })
}
```



## 三、表之间关联

### 1. 主外键关系

```js
// 2.js
const mongoose = require('mongoose');
// mongoose 是一个ORM工作，就是对象模型工具，可以让我们像操作对象一样操作数据库
// school: 为连接的数据库名
const conn = mongoose.createConnection('mongodb://localhost:27017/school', {useNewUrlParser: true});
conn.on('open',function(err) => {
        console.log('数据库连接成功');
        });

// 1. 创建一个集合的Schema，Schema中规定了集合的文档的属性名和属性类型
 let UserSchema = new mongoose.Schema({
   name: {type: String, required: true},
   age: Number
 })  
 // 2. 基于Schema创建模型,返回的是一个对象实例， 'User'为模型名字
 let UserX = conn.model('User', UserSchema);

 let ObjectId = moongoose.Schema.Types.ObjectId;
// 3. 文章集合的Schema
// user: {type: ObjectId, ref: 'UserX'}
// type: 表示user 的字段，ref表示引用，引用的是UserX的这个表
// 这里表示user的外键引用了UserX的主键
let ArticleSchema = new mongoose.Schema({
  title: String,
  content: String,
  user: {type: ObjectId, ref: 'UserX'}
})

let Article = conn.model('Article', ArticleSchema);

UserX.create({name: 'haha', age: 100}, function(err, doc) {
  let _id = doc._id;
  Article.create({title: '标题', content: '内容', user: _id})
}, function (err, doc) {
  console.log(doc)
})



```

### 2. populate

populate：填充的意思，就是把一个属性从一个外键（ObjectId）类型，转成一个对应的文档对象。

```js
Article.findId('xxx').populate('user').exec().then(function(doc) {console.log(doc)})
```

### 3. virtual 

```js
UserSchema.virtual('area').get(function() {
  return this.phone.split('-')[0];
})
```

### 4. Hook 钩子函数

```js
UserSchema.pre('save', function(next) {
  this.password = crypto.createHmac('sha1', 'aha').update(this.password).digest('hex');
  next();
})
```

















