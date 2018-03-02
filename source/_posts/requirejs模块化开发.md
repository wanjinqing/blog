---
title: requirejs模块化开发
date: 2017-08-22 11:49:30
tags:
---

### define定义模块

* 模块名，模块依赖，模块的实现function（return 结果）

    ``` bash
    define('helper', ['jquery'], function ($) {
        return {
            trim: function () {
                return $.trim(str);
            }
        }
    }) 
    ```

### require加载模块

* 模块名，模块的实现function

    ``` bash
    require(['helper'], function (helper) {
        var str = helper.trim('   amd   ');
        console.log(str);
    })
    ```

### 方式一：加载文件

* requirejs以一个相对于baseUrl的地址来加载所有的代码
* 目录结构
![ll](/images/one.png)
* index.html
``` bash
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <script src="/js/require.js"></script>
    <script src="/js/app.js"></script>
</body>
</html>
```

* helper.js
``` bash
define('helper', ['jquery'], function ($) {
    return {
        trim: function (str) {
            return $.trim(str);
        }
    }
})
```

* app.js
``` bash
requirejs.config({
    baseUrl: '/js'
})

require(['helper'], function (helper) {
    var str = helper.trim('   amd   ');
    console.log(str)
})
```

### 方式二：加载文件
修改部分文件为下面内容
* index.html
``` bash
<script data-main="/js/app.js" src="/js/require.js"></script>
```
* app.js
``` bash
require(['helper'], function (helper) {
    var str = helper.trim('   amd   ');
    console.log(str)
})
```

### 加载机制
* requirejs 使用head.appendChild()将每一个依赖加载为一个script标签。
* 加载即执行