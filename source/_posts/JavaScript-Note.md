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
---
主要包括以下内容：
1. 重要的数据结构，如Set/Map和定型数组
2. 用于操作日期和URL的Date与URL类
3. JavaScript的正则表达式语法及处理文本模式匹配的RegExp类
4. JavaScript的国际化库，可以格式化日期/时间/数值/以及排序字符串
5. 序列化和反序列化简单数据结构的JSON对象，以及用于打印消息的console对象


## **迭代器与生成器**
---
**可迭代对象**：任何具有专用迭代器方法，且该方法返回迭代器对象的对象
**迭代器对象**：任何具有next()方法，且该方法返回迭代结果对象的对象
**迭代结果对象**：具有value和done的对象

{% note danger %}
***内置可迭代数据类型的迭代器对象本身也是可迭代的***（即，有一个名为Symbol.iterator的方法，返回他们自己）
{% endnote %}

```js
// words函数是一个可迭代对象，返回一个迭代器对象
function words(s) {
    var r = /\s+|$/g;
    r.lastIndex = s.match(/[^ ]/).index;
    return {
        [Symbol.iterator]() {
            return this
        },
        next() {
            let start = r.lastIndex;
            if (start < s.length) {
                let match = r.exec(s);
                if (match) {
                    return {value: s.substring(start, match.index)};
                }
            }
            return {done: true};
        }
    }
}

console.log([...words(" abc def ghi! ")]); // => [ 'abc', 'def', 'ghi!' ]
```

生成器函数：使用function*
```js
function* fib() {
    let x = 0, y = 1;
    for(;;) {
        yield y;
        [x, y] = [y, x + y] // 解构赋值
    }
}
```

yield*: 与yield类似，但它不是只回送一个值，而是迭代可迭代对象并返回送得的每个值
```js
// use yield
function* sequence(...iterables) {
    for(let iterable of iterables) {
        for(let item of iterable) {
            yield item;
        }
    }
}
console.log([...sequence("abc", "efg")]); // => [ 'a', 'b', 'c', 'e', 'f', 'g' ]

// use yield*
function* sequence(...iterables) {
    for(let iterable of iterables) {
        yield* iterable;
    }
}
console.log([...sequence("abc", "efg")]); // => [ 'a', 'b', 'c', 'e', 'f', 'g' ]
```

## **异步JavaScript**
---
传统异步操作采用事件和回调函数的方式执行

{% label primary@回调 %} vs {% label success@期约(promise) %} 

回调| 期约(promise)
---------|----------
 回调常会引发多层嵌套代码，照成缩进过多以致难以阅读和维护 | 期约可以以一种更线性的期约链形式表现，更易读和推断
 回调难以处理错误，异常没有办法传播到异常操作的发起者 | 期约标准化了异步错误处理，使用期约链提供了一种让错误正确传播的途径

**期约链**
```js
fetch(documentURL)                      // 发送 http 请求
    .then(response => response.json())  // 获取 JSON 格式的响应体
    .then(document => {                 // 在取得解析后的JSON时把文档显示给用户
        return render(document);
    }) 
    .then(rendered => {                 // 在取得渲染的文档后缓存在本地数据库中
        cacheInDatebase(rendered);
    })
    .catch(error => handle(error));     // 处理发生的错误
```

```js
// 并行期约
const urls = [ /* 零或多个URL */ ];
promises = urls.map(url => fetch(url).then(r => r.text()));
Promise.all(promises)
    .then(bodies => { /* 处理得到的字符串数组 */ })
    .catch(e => console.error(e))

// 串行期约
function promiseSequence(inputs, promiseMaker) {
    inputs = [...inputs];
    function handleNextInput(outputs) {
        if (inputs.length === 0) {
            return outputs;
        } else {
            let nextInput = inputs.shift();
            return promiseMaker()
                    .then(output => outputs.concat(output))
                    .then(handleNextInput);
        }
        return Promise.resolve([]).then(handleNextInput);
    }
}
function fetchBody(url) { return fetch(url).then(r => r.text()); }
promiseSequence(urls, fetchBody)
    .then(bodies => { /* 处理字符串数组 */ })
    .catch(console.error);
```

**async & await**

> [await](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/await)
await  操作符用于等待一个Promise 对象
[返回值] = await 表达式;
* 表达式
一个 Promise 对象或者任何要等待的值。

**返回值**
返回 Promise 对象的处理结果。如果等待的不是 Promise 对象，则返回该值本身。

> [async](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function)
> async函数是使用async关键字声明的函数。 async函数是AsyncFunction构造函数的实例， 并且其中允许使用await关键字。
async function name([param[, param[, ... param]]]) {
    statements 
}
- name
函数名称。
- param
要传递给函数的参数的名称。
- statements

包含函数主体的表达式。可以使用await机制。
**返回值**
一个Promise，这个promise要么会通过一个由async函数返回的值被解决，要么会通过一个从async函数中抛出的（或其中没有被捕获到的）异常被拒绝。

```js
function resolveAfter2Seconds(x) {
        return new Promise(resolve => {
                setTimeout(() => {
                        resolve(x);
                }, 2000);
        });
}

async function f1() {
        var x = await resolveAfter2Seconds(10);
        console.log(x); // 10
}
f1();
console.log("hello");
```
```
output：
hello
10
```

**for/await 循环**
异步迭代器会产生一个期约，for/await 循环会等待该期约兑现，将兑现值赋给循环变量，然后再运行循环体

异步迭代器(Symbol.asyncIterator)

异步生成器(async function *)
设置[Symbol.asyncIterator]属性来自定义异步可迭代对象。
```js
const myAsyncIterable = new Object();
myAsyncIterable[Symbol.asyncIterator] = async function*() {
    yield "hello";
    yield "async";
    yield "iteration!";
};

(async () => {
    for await (const x of myAsyncIterable) {
        console.log(x);
        // expected output:
        //    "hello"
        //    "async"
        //    "iteration!"
    }
})();
```

## **元编程**
---
**属性特性**
- 可写：修改属性的值
- 可枚举：for/in循环和 Object.keys() 方法枚举属性
- 可配置：可以删除属性/修改属性

模板标签：位于反引号之间的字符串被称为“模板字面量”

[反射API & 代理对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Meta_programming)
> 允许你拦截并定义基本语言操作的自定义行为（例如，属性查找，赋值，枚举，函数调用等）

## **浏览器中的JavaScript**
---
- JavaScript异步/事件驱动的编程模型
- DOM API是所有客户端JavaScript编程的核心
- CSS样式
- svg & `<canvas>` 生成动态图像
- audio & video
- location & history
- http & websocket 协议与web服务器交换数据
- local cache
  - web storage
  - cookie
  - indexedDB
- worker thread
