---
title: 如何从闭包中获得变量
date: 2017-05-18 15:51:21
tags:
- javascript
---
这里讲一个简单的例子
```
function foo(){
    var person = {
        "name": "joe",
        "age": 18
    }

    return function getValue(name){
        return person[name];
    }
};

var bar = foo();
bar('name'); //=> 'joe'

Object.defineProperty(Object.prototype, 'self', {
    get: function() {
        return this;
    }
});

bar('self'); //=> {name": "joe", "age": 18}
```
至于这么不让他通过扩展原型链获取数据，重写`__proto__`即可
