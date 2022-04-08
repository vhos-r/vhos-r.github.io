---
title: JavaScript Note
tags:
  - javascript
categories:
  - computer language
abbrlink: fbbf86b6
date: 2022-04-04 14:08:48
---

## **词法结构**
---
标识符必须以字母/下划线/美元符号开头

<!-- more-->

## **类型/值/变量**
---
- 原始类型：数值/文本字符串/布尔真值/符号/null/undefined
- 对象类型

javascript可以自由地转换不同类型的值
javascript预定义了全局常量Infinity和NaN以应对正无穷和非数值
var声明的一个最不同寻常的特性是作用域提升

## **表达式与操作符**
---
[] // 空数组

条件式属性访问
+ expression ?. identifier
+ expression ?.[ expression ]

如果expression非真，问号后面的表达式不会执行，即短路操作
+偏向字符串，即只要有一个操作数是字符串，它就会执行拼接操作；比较操作符偏向数值，只有两个操作数均为字符串时才按字符串处理，如果有一个操作数是NaN，则4个比较操作符都返回false
in操作数期待左侧操作数是字符串/符号/可以转换为字符串的值，期待右侧操作数是对象。
instanceof的右侧操作数应该是一个函数
??先定义：第一个操作数求值为null或undefined时才会求值第二个操作数

## **语句**
---
for/of循环专门用于可迭代对象，包括数组/字符串/集合/映射。

for/in循环in后面可以是任意对象
- 不会枚举对象的所有属性，比如名字为符号的属性
- 名字为字符串的属性，只会遍历可枚举属性
- javascript核心定义的各种内部方法是不可枚举的
- 继承的可枚举属性也可被for/in循环枚举

语句标签
identifier：statement
- break [labelname];
- continue [labelname];
    - labelname 属于可选参数

## **对象**
---
创建对象三种方式：
1. 对象字面量
2. 使用new创建对象
3. Object.create()--防止对象被某个第三方库函数意外修改

**几乎所有对象都有原型，但大多数对象没有*prototype*属性**

in可以区分不存在的属性和存在但被设置为undefined的属性

## **数组**
---
扩展操作符(...)在一个数组字面量中包含另一个数组的元素

**实际上并不是操作符，因为只能在数组字面量和本书后面介绍的函数调用中使用它们**

Array()构造函数传入一个数组参数，指定长度：
let a = new Array(10);  // 创建长度为10的空数组
Array.of()可以创建只包含一个元素的数组：
Array.of(10)    // => [10];可以创建只有一个数值元素的数组

## **函数**
---
函数声明语句会被“提升”到包含脚本/函数/代码块的顶部，因此调用以这种方式定义的函数时，调用代码可以出现在函数定义代码之前

箭头函数
- 从定义自己的环境继承this关键字的值

嵌套函数
- 内层函数可以访问包含自己的函数（或更外层函数）的参数和变量

调用函数的五种方式：
1. 作为函数
    - 通过调用表达式被作为函数或方法调用
2. 作为方法
    - 即函数，只不过它保存为对象的属性而已
3. 作为构造函数
    - 使用new关键字调用的函数
4. 透过call() 或 apply() 方法间接调用
5. 通过JavaScript 语言特性隐形调用

**JavaScript函数定义不指定函数形参类型，函数调用不对传入实参进行类型/个数检查**

函数可以有属性
闭包：能捕获自身定义所在函数的局部变量（及参数）绑定

## **类**
---
**只有函数对象才有prototype属性**，这意味着使用同一个构造函数创建的所有对象都继承同一个对象，因而是同一个类的成员

- 类名（按照惯例）应以大写字母开头
- 普通函数和方法的名字则以小写字母开头

**新对象是在调用构造函数之前自动创建的**

与函数声明不同，类声明不会“提升”
原型对象是类标识的基本：当且仅当两个对象继承同一个原型对象时，他们才是同一个类的实例
每个普通 JavaScript 函数自动拥有一个 prototype 属性，这个属性的值是一个对象，有一个不可枚举的constructor 属性，而这个属性的值就是该函数的对象

eg:
```js
let F = function() {};  // 这是一个函数对象
let p = F.prototype;    // 这是一个与F关联的原型对象
let c = p.constructor;  // 这是与原型关联的函数
c === F                 // => true: 对任何F，F.prototype.constructor === F
```

![](/images/类与实例对象关系图.png)

ES6新增了class 关键字，让定义类更方便。但在底层，仍然是构造函数和原型机制在起作用
子类在类声明中通过extends关键字定义
子类可以通过super 关键字调用父类构造函数或父类中被覆盖的方法

## **模块**
---
模块化的目标是让程序员隐藏自己代码的实现细节，从而让不同来源的代码块可以组装成一个大型程序，又不必担心某个代码块会重写其他代码块的函数或变量。本章解释了三种不同的JavaScript模块系统：
- 在JavaScript早期，模块化只能通过巧妙地使用立即调用的函数表达式来实现
```js
const BitSet = (function() {  // 将 BitSet 设置为这个函数的返回值
  ... // 省略实现
  return class BitSet extends AbstractWritableSet {
    ... // 省略实现
  };
}());

// 示例2：
const stats = (function() {
  ...
  function mean(data) { ... };
  function stddev(data) { ... };
  // 将公有函数作为一个对象的属性导出出来
  return { mean, stddev };
}());
```

- Node 在 JavaScript 语言之上加入了自己的模块系统。Node 模块通过require() 导入，并通过设置 Exports 对象的属性或直接设置 module.exports 属性来定义导出
```js
// Node 导出
const mean = data => { ... };
const stddev = data => { ... };
module.exports = { mean, stddev };

// Node 导入
// 内置模块
const fs = require("fs");

// 自定义
const stats = require('./stats.js');

// 解构赋值导入特定属性
const { stddev } = require('./stats.js');
```

- 在ES6 中，Javascript 终于有了自己依托import 和 export关键字的模块系统，ES2020 又通过 import() 增加了对动态导入的支持
```js
// ES6 导出
export const PI = Math.PI;

// ES6 导入
import { mean, stddev } from './stats.js';
import * as stats from './stats.js';

// import() 动态导入
import("./stats.js").then(stats => {
  let average = stats.mean(data);
})
```
## **JavaScript 标准库**

