---
title: 原型继承方法
date: 2017-05-18 14:02:53
tags:
- javascript
---

鉴于我十分容易忘记原型继承的方法，所以记录一下，方便复习_(:з」∠)_

### 原型链继承
```
function SuperType(name){
    this.name = name || "joe";
}

SuperType.prototype.getValue = function(name){
    return this[name];
}

function SubType(age){
    this.age = age || 18;
}

SubType.prototype = new SuperType(); //通过创建SuperType的实例继承，重写了原型对象
//就是没法改到name

```

### 借用构造函数
```
function SuperType(){
    this.name = "joe";
}

SuperType.prototype.getValue = function(name){
    return this[name];
}

function SubType(age){
    SuperType.call(this); //还可以传参哟
    this.age = age || 18;
}
//没有继承到原型链

```

### 组合继承(借用并继承原型链)
```
function SuperType(){
    this.name = "joe";
}

SuperType.prototype.getValue = function(name){
    return this[name];
}

function SubType(age){
    SuperType.call(this); //还可以传参哟
    this.age = age || 18;
}

SubType.prototype = new SuperType();

```

### 共享原型
```
function SuperType(){
    this.name = "joe";
}

SuperType.prototype.getValue = function(name){
    return this[name];
}

function SubType(age){
    this.age = age || 18;
}

SubType.prototype = SuperType.prototype;

```

### 临时借用构造函数
```
function inherit(sub, super){
    function F(){};
    F.prototype = super.prototype;
    sub.prototype = new F();
}
```

### 原型式继承
借助原型可以基于已有的对象创建新的对象，还不用创建自定义类型
与`Object.create`类型
```
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}
```

### 寄生式继承
函数不能复用，会降低效率
```
function createAnother(original){
    var clone = object(original);
    clone.sayHi = function(){
        alert("hi");
    };
    return clone;
}
```

### 寄生组合式继承
```
function inherit(sub, super){
    var prototype = Object(super.prototype);
    prototype.constructor = sub;
    sub.prototype = prototype;
}
```

### 复制继承

