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
> 1. 接口定义interface 与类型别名type的区别；
>
> 相关链接：
>
> 1. [亲手码出TypeScript最前沿的教程（基础篇）](https://juejin.cn/post/6860263805625204743)
> 2. 

## 一、定义

TypeScript是JavaScript的超集。

## 二、优势

### 3. 代码语义更清晰易懂；

## 三、类型

### 1. 基础定义

![image-20210205092025726](/Users/jane/Library/Application Support/typora-user-images/image-20210205092025726.png)

![image-20210205092328935](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210205092339.png)

### 3. 如果写一行，可默认类型，但如果分行写，则需要进行类型声明。 ![image-20210207090445347](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207090447.png)

### 2. 解构赋值的注解

![image-20210205093258540](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210205093300.png)

### 3. 函数相关类型

#### 1. 函数参数为对象的基本方式

![image-20210207085641792](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207085643.png)

#### 2. 函数定义时有两种方法约束参数和返回值类型：

第一种是约束参数和返回值；这种写法返回值类型number可省略不写，编辑器可直接推断出类型；

第二种是直接约束函数的方式：这种写法返回值类型number不能省略，因为省略结构就不对了；

![image-20210205093800508](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210205093803.png)

#### 4. 接口interface  与 类型别名type的区别

interface和type都可以定义，但interface适用于定义对象或者函数，不能定义简单类型。type什么类型都可以。能用接口定义表示就用接口interface。

![image-20210205094754620](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210205094757.png)

#### 5. 注意点：函数传参是字面量对象，会进行强校验；如果是之前定义好对象，再传参，则是弱校验。

![image-20210205100525259](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210205100526.png)

![image-20210205100037293](/Users/jane/Library/Application Support/typora-user-images/image-20210205100037293.png)

### 4. 数组

也可以这样写：

```typescript
const objectArr: {name: string, age: number}[] = [{name: 'Jane', age: 28}]
```

下面方式分别是适用类型别名 、class方式进行定义。区别在于：class适用于对象和new 对象；而type类型别名下面这样的写法，只适用于对象。

![image-20210207091017833](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207091019.png)



### 5. 元组 tuple

规定数组内各个元素位置上的类型，下面这种写法无法确定各个值的类型。

![image-20210207091712605](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207091714.png)

如下即是适用元组方式来规定各个元素类型的。

![image-20210207091949251](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207091957.png)



### 6. interface接口

这里的say是方法，函数需返回string类型的值；

![image-20210207092802810](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207092804.png)

![image-20210207092905858](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207092907.png)

#### 2. class类应用接口类型，使用implements。如此class类就需要具备接口Person的这些属性。

![image-20210207093009628](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207093011.png)

#### 3. 接口interface实现继承

![image-20210207093418364](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207093419.png)

#### 4. 接口interface定义函数类型

入参是word字符串，返回值为string类型；

![image-20210207093522731](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207093523.png)

![image-20210207093628275](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207093734.png)

编译后会发现，接口interface不会出现在编码代码中，由此可知，它的作用只在开发阶段进行语法提示和校验的功能。

### 7. 类的访问类型和构造器

#### 1. private、protected、public

![image-20210207095433094](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207095434.png)

#### 2. constructor构造器

如下使用public，可以代替public name 定义，以及this.name=name的赋值操作。

![image-20210207100204935](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207100206.png)

继承，子类和父类都有constructor构造器，则在子类中需要调用super()。实例化的时候，constructor构造器会被立即执行。

![image-20210207100531230](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207100533.png)

#### 3. Getter\setter

保护私有属性，getter函数，可以通过直接访问就可以。

![image-20210207102208856](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207102211.png)

#### 4. static

使用static静态方法可以把方法挂载到类上面，如此即便实例很多，也只会调用一次函数。

![image-20210207102443180](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207102444.png)

实现单例模式

![image-20210207103346148](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210207103347.png)

## 四、编译、配置文件

1. `ts-node`: 编译执行ts文件；
2. `tsc`: ts编译成js 文件；

### 1. 查看tsconfig.json文件



## 五、联合类型和类型保护

1. ｜ 为联合类型，取两类型里都有的属性或者方法；

![image-20210208170212030](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208170213.png)

类型保护有以下几种方法：1.类型断言方法，as即是断言，直接告诉ts是什么类型；

2.  in语法类型保护。 

![image-20210208170242409](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208170243.png)

3. Typeof 语法来做类型保护。

![image-20210208170650240](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208170701.png)

4. instaceof操作符来做类型保护，这时必须使用class类定义，如果是interface定义类，则无法使用instanceof类型保护。

![image-20210208170943999](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208170950.png)

![image-20210208171416657](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208171423.png)

## 六、函数泛型

### 1. 使用尖括号<>，泛指各种可能但需要一致的类型。在调用时再指定具体类型。

![image-20210208175127172](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208175135.png)

![image-20210208202823833](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208202833.png)

### 2. 传数组，可以Array<ABC>, 也可以ABC[] 。

![image-20210208202913952](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208202921.png)

### 3. 可定义多个泛型类型。

如此在调用函数时，可任意传类型。

可以`join<number, number>(1,1)`;

也可以`join<number, string>(1, '1')`;

亦可以不写。`join(1, '1');`由ts自己类型推断确定。

![image-20210208203305647](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208203315.png)

### 4. 函数也返回T类型，是:T, 如此表达。但如下图写不正确。

因为返回T类型不一定返回字符串。

![image-20210208203913362](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208203924.png)

## 七、类中的泛型以及泛型类型

### 1. 在class类名后<T>;

![image-20210208205225156](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208205228.png)

### 2. 如果泛型里必须有某些属性或者方法，这时需要继承。

但这里getItem的返回类型不能是T，因为return的是name，是string类型，所以这里的返回类型是确定String，而不是T。

T 泛型继承于Item类型，必须有name属性。

![image-20210208205737832](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208205740.png)

getItem如此写就对了。

![image-20210208205930236](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208205931.png)

### 3. T泛型只能只能选择指定的几种类型，不能随意类型，需要如下做。

将泛型T继承于 多个联合类型。

![image-20210208210559886](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208210602.png)

如下指定类型调用是不行的。因为泛型只能是number或者string，这里就不能指定成Test类型了。

![image-20210208210745633](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208210747.png)

如此写成number或者string就没问题了。

![image-20210208210933256](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208210937.png)

### 4. 使用泛型作为一个具体的类型注解。

![image-20210208211307504](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210208211310.png)