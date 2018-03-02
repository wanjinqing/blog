---
title: scope
date: 2017-08-17 16:05:55
tags:
---

### 延长作用域链

try-catch语句的catch块：对catch语句来说，会创建一个新的变量对象。

``` bash
try {
    fn();
} catch (e) {
    console.log(e) // error
}
```

with语句：对with语句来说，会将指定的对象添加到作用域链中。

``` bash
function build (){
    var qs = 'afd';
    with (location) {
        var s = href + qs;
    }
    console.log(s) // 可以访问
}
build();
```