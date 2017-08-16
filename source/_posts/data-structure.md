---
title: 数据类型
date: 2017-05-15 20:32:15
tags:
- javascript
---

## 杂
* `"use strict"`是编译指示，为了不破坏ES3语法特定选择。
* JavaScript的变量是松散类型的。松散类型可以用来保存不同类型的数据。可以说变量仅是一个用于保存值的占位符而已。

## 数据类型
### Undefined
* 派生自null
* 代表着未声明或未初始化，但是试图使用未声明的变量会报错，`typeof`不会。
    * 推荐做法： 显示声明变量，以区分未声明
* 没有`toString`方法
* **全局对象的一个属性**
* undefined == null
* void 0 === undefined，因为void运算符对给定的表达式进行求值，然后返回undefined。

### Null
* 表示空对象指针(空值、没有对象被呈现)，因此`typeof null === "object"`
* 没有`toString`方法

-----

### Boolean

### String
* ES6可以使用模板字面量`hello ${world}`

### Number
* 八进制字面量，在严格模式下抛出错误
```
    function a() {
        "use strict"
        var a = 012;
        console.log(a);
    }
    //Uncaught SyntaxError: Octal literals are not allowed in strict mode.
```
* 不能直接`toString`。假设是`2.toString()`，JavaScript会认为它是2.0和toString，然后报错，解决手段
```
2 .toString();
(2).toString();
```
* `isFinite()`判断数值是否在最大值和最小值之间。入参会进行类型转换，object类型的值会先调用`valueOf`，如果返回值不能转换为数值，则会基于该值调用`toString`。
* ```
    Number(null) === 0;
    isNaN(Number({})) === true; //对于对象的转换与isFinite同
    isNaN(Number(undefined)) === true;
    ```
* parseInt在ES3中，有些值会被当成八进制字面量，如果不穿基数，所以最好还是要传基数。


### 基本包装类型
**!! JavaScript还拥有3个特殊的*基本包装类型*(Number、String、Boolean)**
每当读取一个基本类型值的时候，后台就会创建一个相应的基本包装类型。
（悄悄创建一个String类型的实例，然后在实例上调用方法...，使用完后销毁）
```
    var str = 'string';
    str.substring(2); //创建一个String实例，在实例上调用方法, 使用后销毁
    str.test = 123; //创建一个String实例，在实例上添加属性
    console.log(str.test) //=> undefined 使用后已经被销毁
```
基本包装类型与引用类型的区别在于对象生存期，自动创建的基本包装类型对象，只存在于代码存在的一瞬间。

而且与字面量也有所区别
```
    var str1 = 'string'; //(字符串字面量, 隐式创建的基本包装类型对象)
    var str2 = new String('string'); //(字符串对象, 显示创建的基本包装类型对象)

    typeof str1 //'string'
    typeof str2 //'object'
```

-----

### Object <u>**(复杂类型 | 引用类型)**</u>
* **引用类型的值（对象）是引用类型的一个实例。引用类型是一种数据结构。**
* 新对象是通过new 构造函数创建的，对象是某个特定引用类型的实例，保存在内存中，操作时操作的实际是对象的引用而不是实际对象
* 构造函数本身也是函数
* 通过创建Object实例的方法，可以创建自定义的对象
* Object类型是它所有实例的基础，Object类型所具有的任何属性和方法也同样存在于更具体的对象中
#### Object
* 通过对象字面领定义对象时，在FF2就不会调用Object构造函数了
* `new Object(null)`或`new Object(undefined)`返回结果与`new Object`相同
* `new Object(true)`与`new Boolean(true)`等价, `new Object(2)`与`new Number(2)`等价
* 当设定`__proto__`不是null或对象时，它是不会被改变的。在对象字面值中，仅有一次变更原型的机会；多次变更原型，会被视为语法错误。不使用冒号标记的属性定义，不会变更对象的原型；而是和其他具有不同名字的属性一样是普通属性定义。
  ```
    var __proto__ = "variable";
    var obj1 = { __proto__ };
    var obj2 = { __proto__: __proto__ };
    var obj3 = { ["__prot" + "o__"]: __proto__ };
    var obj4 = { __proto__() { return "hello"; } };
    console.log(obj1.__proto__) //=>"variable"
    console.log(obj2.__proto__) //=> Object.prototype
    console.log(obj3.__proto__) //=>"variable"
    console.log(obj4.__proto__) //=>"variable"
  ```

#### Array
* 数组是类似列表的对象
```
    var arr1 = new Array(3); //=> [undefined × 3]
    var arr2 = Array.apply(null, arr1) //=> [undefined, undefined, undefined]
    var arr3 = Array.apply(null, {length: 3}) //=> [undefined, undefined, undefined]
    //因为apply()的第二个参数是数组或类数组
```

#### Date
*  `+new Date`可以获得时间戳

#### RegExp

#### Function
* 每个函数都是Function的实例
* Function对象继承自 Function.prototype 属性
* 函数名其实是一个指向函数对象的指针（函数是对象，函数名是指针）
* 解析器会率先读取函数声明（函数声明提升），而通过函数表达式声明的函数，必须等待解析器执行到他的位置
* 参数只是副本


