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

编程语言的类型分两种：

1. 动态类型语言（Dynamically Typed Language）

   在运行期间才会做数据类型检查的语言。不用给变量指定数据类型。如此，我们只有在运行代码时，才会发现有什么错误，有危险。

2. 静态类型语言（Statically Typed Language）

   数据类型监测发生在数据编译阶段。写程序前要申请变量类型。C，C++，Java等都是静态类型语言。

   

TypeScript是JavaScript的超集。

## 二、优势：为什么要使用Typescript

### 1. 程序更容易理解；

![image-20210223123744362](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223123747.png)

### 2. 效率更好高

![image-20210223123928937](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223123931.png)

### 3. 更少的错误；

举例：x x x is undefined. 

![image-20210223124026672](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223124028.png)

### 4. 非常好的包容性

![image-20210223124159264](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223124201.png)

## 三、类型

### 1. 基础定义

### 2. 数组

数组与类数组的定义类型不同。如下图所写，直接将类数组赋值给数组，会报错。

![image-20210223125458940](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223125500.png)

其实关于类数组，TS早已经给我们写好了类型，比如下面：参数的类数组就是IArguments类型。而其他HTML之类的类数组，也内置了很多类数组类型。

![image-20210223125609632](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223125611.png)

### 3. 元组

数组与元组很相似。数组与元组的区别则是：

数组里的每一项都是同一类型的值，将统一类型的数据聚合到了一起；

而元组中的每一项，则是可以为不同类型的。元组是聚合了不同类型的数据。

> 如下图：arrOfNumbers是数组式定义类型；而user则定义的是元组类型。

![image-20210223130421244](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223130422.png)

### 4. 定义对象类型：使用Interface接口

interface接口：在js中是不存在的，所以TS书写完之后是不会转义过去的，所以它只能用来做类型的静态检查。

![image-20210223130922967](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223131030.png)

#### 1. readOnly

如果我们希望，只在对象创建时某些属性被赋值，那可以使用readonly定义只读属性。

Readonly 与 const  都是只读：但readOnly作用于属性，而const作用于变量。

![image-20210223131904807](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223131906.png)

### 5. 函数Function

https://www.tslang.cn/docs/handbook/functions.html

需要约定输入和输出两部分。

![image-20210223132210250](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223132212.png)

定义函数类型有三种方式：

1. add函数《直接把函数赋值给变量》：一般函数书写后，对输入和输出参数做类型定义；

2. add2函数：带箭头表示，这个箭头不是ES6中的箭头函数，而是Typescript声明函数类型返回值的方法。
3. 使用interface, ISum：接口定义方式。注意返回值后的类型用：表示；

![image-20210224090529806](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210224090531.png)

![image-20210224091002691](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210224091004.png)

> 注意：冒号后面都是声明类型。和代码的逻辑没有什么关系。
>
> 上面这两种写法，需要自己再消化消化、理解理解。// TODO

### 6. 类型推论、联合类型 和类型断言

类型推论：ts会对赋值的值进行类型推断；

联合类型：变量可以具备几种类型，当TS不确定一个联合类型的变量到底是哪个类型时，我们只能访问联合类型中所有类型里共有的方法。

类型断言：用as关键字，我清楚自己这个变量是什么类型。

类型守卫： typescript 在不同的条件分支里面，智能的缩小了范围，这样我们代码出错的几率就大大的降低了。

![image-20210224092206979](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210224092209.png)

类型守卫：有typeof，instanceof，instanceof作用于类的实例。

![image-20210224093858186](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210224093900.png)

### 7. 类 Class

![image-20210224094248374](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210224094251.png)

- **public** 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的
- **private** 修饰的属性或方法是私有的，不能在声明它的类的外部访问
- **protected** 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的

![image-20210224100457128](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210224100459.png)

#### 1. 类与接口

使用接口implements来实现类，抽象类的属性和方法。

![image-20210225085959172](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225090001.png)

下面这个例子中，car和cellphone很难找到一个合适的父类让其继承公共的方法，这时可以使用接口implements来实现，完成逻辑功能的提取和校验。

`switchRadio(trigger:boolean):void; 其中trigger代表是boolean的入参，void代表函数没有返回值`。

![image-20210225090609886](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225090611.png)

下面这个例子是接口之间的继承，不是类。让RadioWithBattery 继承Radio接口，这时Cellphone就具备了两个方法。和上图功能一样。

![image-20210225090743120](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225090744.png)

### 8. 枚举

枚举通常用于常量值和 计算值。

![image-20210225091710689](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225091712.png)

编译代码为 Direction 为双向赋值，使得枚举的值实现递增。

![image-20210225091613515](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225091615.png)

### 9. 泛型《最难的部分》

#### 1. 泛型出现的动机

泛型在定义函数或者类时，不与先指定类型，而是在使用时再指定类型。

![image-20210225092921513](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225092922.png)

泛型写法：echo函数名之后，使用尖括号标记一个T，相当于是一个占位符，可以随意标记。arg入参的类型是T，后面冒号：T，代表函数返回值的类型也是T。

这时在函数调用时，TS就会根据传入参数的类型来判断函数返回值的类型。

![image-20210225093205252](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225093206.png)

对于多个类型的参数变化，可以参照如下方式写。

![image-20210225093708256](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225093709.png)

#### 2. 约束泛型：泛型作用与函数中

只允许函数传入指定属性或者类型的变量。

定义一个具有某参数的接口，让泛型继承这个接口，如此就可以约束传入的参数需要具备length属性。

如下图这种写法只能满足了是数组类型的可以拥有length属性，但其他string类型或者object类型都可能会有string类型，这时就需要接口来声明。

![image-20210225095007993](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225095009.png)

![image-20210225094629322](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225094631.png)

![image-20210225094856092](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225094857.png)

#### 3. 泛型作用于类和接口中

![image-20210225100645817](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210225100647.png)



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

## 	八、命名空间

### 1. 写法：

使用namespace定义和export导出暴露函数或者类。通过new和点语法进行调用。

![image-20210222093701719](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222093710.png)

![image-20210222093750991](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222093752.png)

![image-20210222094012569](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222094013.png)

### 2. 命名空间之间相互应用

如此配置，可以将多个文件打包到一个文件去。

![image-20210222094905006](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222094906.png)

使用/// 进行依赖声明；

![image-20210222094727073](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222094728.png)

在命名空间中也能导出子命名空间。

![image-20210222100402098](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222100403.png)

## 九、如何编写.d.ts的类型定义文件

### 1. 通过declare进行声明。同一个变量声明可以进行重载。

![image-20210222104905936](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222104908.png)

代码优化：

![image-20210222110218771](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222110220.png)

### 2. 通过interface实现函数重载

这里的JQuery，就指的是JQuery可以使用readyFunc这种函数传参，页可以用通过selector通过字符串去传。

![image-20210222110431453](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222110433.png)

### 3. 如果对对象进行声明

![image-20210222111355002](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222111400.png)

### 4. 以下是ES6模块化的写法。

![image-20210222111751177](/Users/jane/Library/Application Support/typora-user-images/image-20210222111751177.png)

##  十、泛型中keyof语法实现类型保护机制

如下代码中红色提示，就是ts无法确定key的值是否是Person类的中属性。需要进行类型保护。

![image-20210222113245741](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222113247.png)

如此写法才是正确的。

![image-20210222113636700](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222113638.png)

这样关于返回值类型才是准确的。

![image-20210222113723906](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222113725.png)

## 十一、类的装饰器

装饰器在ts中还属于实验阶段，需要打开这两个配置项。

装饰器执行时机：在类创建好之后就会立即执行装饰器函数。与实例化类次数无关。

装饰器参数：构造器本身。

![image-20210222130947975](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222131055.png)

![image-20210222131406197](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222131413.png)

多个装饰器的执行顺序：靠近构造函数的装饰器先执行。从下到上、从右到左执行。

![image-20210222131601502](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222131650.png)

装饰器也可以通过返回一个函数，调用的方式表示。

![image-20210222131929667](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222131931.png)

如此做的好处就是：可以通过装饰器传参，做一些判断。

![image-20210222132038838](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222132041.png)

### 1. 标准写法

红框含义：一个函数有很多...args的参数，每一项都是any类型，函数返回值的类型是any。前面的new代表了这个函数是构造函数。泛型T实例化于这个构造函数。`(constructor: T)代表参数是： 构造函数，它的类型是泛型T`。

返回的 `class extends constructor {}`直接返回了构造器本身，没有做任何变动。

![image-20210222133234688](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222133236.png)

这里则是对构造函数做了扩展。将构造函数中的name属性赋值成了lee.

![image-20210222133456834](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222133458.png)

执行顺序：先执行构造函数本身的代码，然后执行装饰器中的代码。

![image-20210222133656253](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222133658.png)

### 2. 在装饰器中添加构造器方法需要注意的点

通过下面这种方法是无法达到在装饰器中添加构造器方法的目的的，因为ts无法监测到这个getName方法是从何来的。

![image-20210222141815776](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222141818.png)

![image-20210222141853842](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222141855.png)

可以使用如下方式：

1. 装饰器包一层，返回一个函数；
2. 将匿名的构造函数作为参数传入到装饰器中，如此返回的构造器Test就可以拿到getName方法了。

![image-20210222142206780](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222142209.png)

## 十二、 方法装饰器

方法装饰器执行时机：在构造器执行中，就会对方法进行装饰，而不是实例化的时候。

![image-20210222232705588](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222232708.png)

![image-20210222233213193](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210222233214.png)

如果装饰器没有做任何修改，那实例化之后，若对实例上的原型方法做改造，则会覆盖掉原来原型上装饰器的方法。优先执行实例化之后的方法。

![image-20210223084359258](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223084408.png)

但如果不想让之后的方法被修改，可以在装饰器上添加该属性，不让重写，之后如果再进行修改此方法，则会报错。

![image-20210223084554544](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223084556.png)

![image-20210223084707559](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223084708.png)

## 十三、访问器的装饰器

访问器即get和set方法，参数和方法装饰器的参数一样。

target代表构造器的prototype原型，key为方法名，descriptor为：描述器。

> 注意点：getter和setter不能同时加相同的装饰器，会报错。

![image-20210223085440527](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223085442.png)

![image-20210223085658460](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223085659.png)

上面这个例子，是对set访问器调用了不可重写的装饰器。如此当调用test.name="dell lee"时，就会出错。

## 十四、属性装饰器

属性装饰器里没有descriptor解释器，需要创建一个解释器并返回，如果想要设置属性的解释属性信息，如此装饰器才有效。

![image-20210223090518767](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223090520.png)

执行顺序：先执行实例上有的属性，如果实例上没有，才会执行原型上的装饰器。

![image-20210223091217083](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223091218.png)

![image-20210223091316178](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223091317.png)

## 十五、参数装饰器

![image-20210223091752565](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223091754.png)

## 十六、实用小例子

### 1. 错误捕获

因为userInfo 定义为undefined,函数调用userInfo.name时肯定会出错，避免这种情况发生，就可以将其封装在装饰器。一旦有错误，就会统一提示。

![image-20210223092528886](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223092530.png)

![image-20210223092619045](https://cdn.jsdelivr.net/gh/trylang/imageManager/picgo/20210223092620.png)