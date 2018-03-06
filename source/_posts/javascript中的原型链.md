---
title: javascript中的原型链
date: 2018-03-04 19:05:02
tags:
---


###  prototype 和 \_\_proto\_\_

* prototype是一个属性，同时它本身也是一个对象,所有的函数都有prototype属性，javascript中是不存在类的概念，我们通常会使用函数来模拟，例如: new Object(), new Array(), new Date();所以Object,Array,Date是函数对象。

* IE里不提供\_\_proto\_\_这个属性，该属性不是标准的方法，标准的方法是Object.getPrototypeOf();

* Math.prototype === undefined

* 每个对象的\_\_proto\_\_属性指向自身构造函数的prototype
Foo.prototype:
&nbsp; &nbsp;   constructor: function Foo(),
&nbsp; &nbsp;   \_\_proto\_\_: Object

``` bash
function Foo() {} 
var fn = new Foo();

// fn的__proto__指向其构造函数Fun的prototype
fn.__proto__ === Foo.prototype

// Foo的__proto__指向其构造函数Function的prototype
Foo.__proto__ === Function.prototype

// 构造函数自身是一个函数，他是被自身构造的
Function.__proto__ === Function.prototype

// Function.prototype的__proto__指向其构造函数Object的prototype
Function.prototype.__proto__ === Object.prototype

// Object作为一个构造函数(是一个函数对象),所以他的__proto__指向Function.prototype
Object.__proto__ === Function.prototype

// Object.prototype作为一切的源头,他的__proto__是null
Object.prototype.__proto__ === null

var a = 1; // 等价于new Number(1);
a.__proto__ === Number.prototype

var s = 'string';
s.__proto__ === String.prototype

```

<font color=#000 size=3>
var s = 'string';
s.\_\_proto\_\_.\_\_proto\_\_.\_\_proto\_\_ === null, 我们沿着a的\_\_proto\_\_一直访问，到达Object的prototype，其实最终访问的是Object.prototype的toString方法。
</font>




### 例子:

``` bash
People
    - name
    - sayName
    - prototype --------- People.Prototype
                    |             - constructor
                    |             - walk 
                    |             - __proto__ -------- Object.prototype
                    |                                  - valueOf
                    |                                  - toString
                    |                                  - constructor
                    |
                    |
                    |
p1                  |     p2
    - name          |       - name
    - sayName       |       - sayName
    - __proto__ ----------- - __proto__           
```