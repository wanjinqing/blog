---
title: javaScript arguments对象
date: 2017-08-17 09:52:13
tags:
---

### callee

callee 属性是 arguments 对象的一个成员，他表示对函数对象本身的引用，这有利于匿名函数的递归或确保函数的封装性。callee拥有length属性，arguments.length是实参长度，arguments.callee.length是形参长度，由此能够判断调用时形参长度是否和实参长度一致。 

``` bash
function inner () {
    console.log(arguments.callee);// 指向拥有这个arguments对象的函数，即inner()
    console.log(arguments.callee.caller);// 这个属性保存着调用当前函数的函数的引用,即outer()
    console.log(inner.caller);// [Function: outer]
}
function outer(){
    inner();
}
outer();
```

### 形参与实参

形参和实参都有的情况下，它们的值会同步，否则它们的值不会同步。

``` bash
function f(a, b, c){
    c = '哈哈';
    console.log(c) // 哈哈
    console.log(arguments[2]); // undefined
    arguments[2] = 'joker'
    console.log(c) // 哈哈
    console.log(arguments[2]); // joker
    
    a = '哈哈';
    console.log(a) // 哈哈
    console.log(arguments[0]); // 哈哈
    arguments[0] = 'joker'
    console.log(a) // joker
    console.log(arguments[0]); // joker
}
f(1, 2);
```