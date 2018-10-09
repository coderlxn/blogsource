---
title: js基础语法
date: 2018-09-21 10:43:53
tags: javascript
---
##### Basic
* js的执行单位为行，一般一行就一个语句，以分号结尾

##### 字面量
一般固定的值被称为字面量，主要有
* 数字字面量
* 字符串字面量
* 表达式字面量   `5 + 6`
* 数组字面量 `[40, 100, 1, 5, 25, 10]`
* 对象字面量 `{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}`
* 函数字面量  `function myFunction(a, b) { return a * b;}`

##### 变量
* 变量是一个名称，字面量是一个值
* 通过`var` 来声明变量。虽然用var关键字声明和不用关键字声明，很多时候运行并没有问题，但是这两种方式还是有区别的。
```js
// num1为全局变量，num2为window的一个属性
var num1 = 1;
num2 = 2;
// delete num1;  无法删除
// delete num2;  删除
function model(){
var num1 = 1; // 本地变量
num2 = 2;     // window的属性
    // 匿名函数
    (function(){
        var num = 1; // 本地变量
        num1 = 2; // 继承作用域（闭包）
        num3 = 3; // window的属性
    }())
}
```
* `let` 允许声明一个作用域被限制在块级中的变量、语句或者表达式。在Function中局部变量推荐使用let变量，避免变量名冲突。
* 如果只是声明变量而没有赋值，则该变量的值是`undefined`
* JavaScript 是一种动态类型语言，也就是说，变量的类型没有限制，变量可以随时更改类型。
```js
var a = 1;
a = 'hello';
```
* 如果使用var重新声明一个已经存在的变量，是无效的
* JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）
```js
console.log(a);
var a = 1;
```
* 上面代码首先使用console.log方法，在控制台（console）显示变量a的值。这时变量a还没有声明和赋值，所以这是一种错误的做法，但是实际上不会报错。因为存在变量提升，真正运行的是下面的代码。
```js
var a;
console.log(a);
a = 1;
```
* 可以将变量的值设置为null来清空变量

##### 对象
* 对象是键值对的容器，值可以是一个函数
```js
var person = {
    firstName: "John",
    lastName : "Doe",
    id : 5566,
    fullName : function() 
	{
       return this.firstName + " " + this.lastName;
    }
};
```

##### 作用域
*  在函数内部用var声明的变量是局部变量，只能在函数内部访问，函数运行完毕会被删除
* 变量在函数外定义，即为全局变量
* 变量在函数内部定义，但没有使用var，该变量为全局变量
* 在HTML中，全局变量是window对象，所有数据变量都属于window对象

##### 字符串直接量与new String的区别
```js
var a = "foo";
var b = new String("foo");

console.log(a); // foo
console.log(b); // String {0: "f", 1: "o", 2: "o", length: 3, [[PrimitiveValue]]: "foo"}

console.log(typeof a); // "string"
console.log(typeof b); // "object"

console.log(a == b);  // true
console.log(a === b); // false

console.log(a.charAt === b.charAt); // true
```
可以看出，变量a是一个primitive值（原生值），一个primitive值不会拥有自己的属性与方法。
而b是一个primitive类型（primitive类型：Undefined, Null, Boolean, String, Number）的实例，它有一个值为实例化String类型时所传入的参数的内置属性[[PrimitiveValue]]，这个String的实例享有String.prototype上所有的方法。
但是，为何当我们访问a.charAt时能够访问到String.prototype.charAt呢？
因为当我们尝试访问一个primitive值的属性时，JS引擎内部会调用一个内置[[toObject]] 方法，将字面量的”foo”转为一个[[PrimitiveValue]]为”foo”的String对象，然后从其原型链中尝试查找需要访问的属性，使用结束后再释放掉这个String对象。

##### 数据类型
* 5中不同的数据类型
  1. string
  2. number
  3. boolean
  4. object
  5. function
* 3中对象类型
  1. Object
  2. Date
  3. Array
* 2个不含值的数据类型
  1. null
  2. undefined

##### null与undefined的区别
```js
typeof undefined             // undefined
typeof null                       // object
null === undefined         // false
null == undefined           // true
```

##### 类型转换
* 将数字转换为字符串  `String(x)` 或者 `(x).toString()`
* 将布尔型转换为字符串	`String(false)` 或者 `true.toString()`
* 将日期转换为字符串	`String(new Date())` 或者 `obj = new Date(); obj.toString()`
* 将字符串转换为数字	`Number("321")`
* Operator + 可用于将变量转换为数字：
```js
var y = "5";      // y 是一个字符串
var x = + y;      // x 是一个数字

var y = "John";   // y 是一个字符串
var x = + y;      // x 是一个数字 (NaN)
```

##### 变量提升
只有声明的变量会提升，初始化的不会
```
var x = 5; // 初始化 x
elem = document.getElementById("demo"); // 查找元素 
elem.innerHTML = "x 为：" + x + "，y 为：" + y;           // 显示 x 和 y
var y = 7; // 初始化 y
```
y 输出了 undefined，这是因为变量声明 (var y) 提升了，但是初始化(y = 7) 并不会提升，所以 y 变量是一个未定义的变量


##### 严格模式
* 不允许使用未声明的变量
```js
"use strict";
x = 3.14;                // 报错 (x 未定义)
```
* 不允许删除变量或对象
```js
"use strict";
function x(p1, p2) {}; 
delete x;                // 报错 
```
* 不允许变量重名:
```js
"use strict";
function x(p1, p1) {};   // 报错
```
* 不允许使用八进制:
* 不允许使用转义字符:
* 不允许对只读属性赋值:
```js
"use strict";
var obj = {};
Object.defineProperty(obj, "x", {value:0, writable:false});

obj.x = 3.14;            // 报错
```
* 不允许对一个使用getter方法读取的属性进行赋值
* 不允许删除一个不允许删除的属性：
* 变量名不能使用 "eval" 字符串:
* 变量名不能使用 "arguments" 字符串:
* 由于一些安全原因，在作用域 eval() 创建的变量不能被调用：
* 禁止this关键字指向全局对象。

##### 常见错误
* 在常规的比较中，数据类型是被忽略的
```
var x = 10;
var y = "10";
if (x == y)     //返回true
```
* JavaScript 中的所有数据都是以 64 位浮点型数据(float) 来存储
```js
var x = 0.1;
var y = 0.2;
var z = x + y            // z 的结果为 0.3
if (z == 0.3)            // 返回 false
```