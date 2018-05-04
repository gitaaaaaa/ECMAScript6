# 1.let命令
### 基本用法
let声明的变量只在它所在的代码块有效
```
{
  let a = 10;
  var b = 1;
}
// a
// ReferenceError: a is not defined.
// b
// 1
```
for循环的计数器，合适使用let命令
```
for (let i = 0; i < 10; i++) {
  //...
}
// console.log(i);
// ReferenceError: i is not defined
```

实例:重新认识for循环
```var
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```
```let
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```
for循环的一个特别之:设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域
### 不存在变量提升 
1. let 关键词声明的变量不具备变量提升（hoisting）特性
### 暂时性死区（temporal dead zone，简称 TDZ） -go know typeof
```
if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
```
ES6 规定暂时性死区和let、const语句不出现变量提升--以上是为了变量一定要在声明之后使用，否则就报错
### 不允许重复声明
```
// let不允许在相同作用域内，重复声明同一个变量。
// 报错
function func() {
  let a = 10;
  var a = 1;
}

// 报错
function func() {
  let a = 10;
  let a = 1;
}
// let不允许在相同作用域内，重复声明同一个变量。
function func(arg) {
  let arg; // 报错
}

function func(arg) {
  {
    let arg; // 不报错
  }
}
```
# 2.块级作用域
```
  ES5只有全局作用域和函数作用域
```
### 为什么需要块级作用域？
```
第一种场景，内层变量可能会覆盖外层变量。
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
变量提升，导致内层的tmp变量覆盖了外层的tmp变量。
```
```
第二种场景，用来计数的循环变量泄露为全局变量。

var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}

console.log(i); // 5
变量i只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。
```
###ES6 的块级作用域
```实例
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```
ES6 允许块级作用域的任意嵌套。
```
{{{{
  {let insane = 'Hello World'}
  console.log(insane); // 报错
}}}};
外层作用域无法读取内层作用域的变量
{{{{
  let insane = 'Hello World';
  {let insane = 'Hello World'}
}}}};
内层作用域可以定义外层作用域的同名变量。
```
```
  ES5:
  广泛应用的立即执行函数表达式（IIFE）不再必要了
  // IIFE 写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```
### 块级作用域与函数声明
```
-问: 函数能不能在块级作用域之中声明？
-答: ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。
```
ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。
-->考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。
```
// 函数声明语句
{
  let a = 'secret';
  function f() {
    return a;
  }
}

// 函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```
-允许在块级作用域内声明函数。
-函数声明类似于var，即会提升到全局作用域或函数作用域的头部。
-同时，函数声明还会提升到所在的块级作用域的头部。
注意，上面三条规则只对 ES6 的浏览器实现有效
```
// 浏览器的 ES6 环境
function f() { console.log('I am outside!'); }

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());
// Uncaught TypeError: f is not a function
```
上面的代码在符合 ES6 的浏览器中，都会报错，因为实际运行的是下面的代码。
```
// 浏览器的 ES6 环境
function f() { console.log('I am outside!'); }
(function () {
  var f = undefined;
  if (false) {
    function f() { console.log('I am inside!'); }
  }

  f();
}());
// Uncaught TypeError: f is not a function
```

# 3.const命令
## 基本用法
```
1.const声明一个只读的常量。一旦声明，常量的值就不能改变。
1.1 当使用常量 const 声明时，请使用大写变量，如：CAPITAL_CASING ? 
const PI= 3.1415;
PI // 3.1415
PI = 3;
// TypeError: Assignment to constant variable.
2.const一旦声明变量，就必须立即初始化
const foo;
// SyntaxError: Missing initializer in const declaration
3.const的作用域与let命令相同：只在声明所在的块级作用域内有效。
if (true) {
  const MAX = 5;
}
MAX // Uncaught ReferenceError: MAX is not defined
4.const命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。
if (true) {
  console.log(MAX); // ReferenceError
  const MAX = 5;
}
5.const声明的常量，也与let一样不可重复声明。
var message = "Hello!";
let age = 25;
// 以下两行都会报错
const message = "Goodbye!";
const age = 30;
```
## 本质 
| 数据类型 | 变量指向 | 数据结构 | 
| - | :-: | -: | 
| 简单（数值、字符串、布尔值） | 内存地址| 已固定 | 
| 复合类型（主要是对象和数组） | 内存地址,但保存为一个指针 | 无法控制 | 

将对象彻底冻结↓
```
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```
## ES6 声明变量的六种方法
ES5 var function
ES6 let const import class

# 4.顶层对象的属性
顶层对象，在浏览器环境指的是window对象，在 Node 指的是global对象。ES5 之中，顶层对象的属性与全局变量是等价的。
```
顶层对象的属性与全局变量挂钩，被认为是 JavaScript 语言最大的设计败笔之一。
带来的问题:
1. 无法在编译时就报出变量未声明的错误
2. 无意中创建了全局变量
3. 顶层对象的属性是全局都可以读取,不利于模块化编程
4. 顶层对象是一个有实体含义的对象(指浏览器的窗口对象)
```
ES6，全局变量将逐步与顶层对象的属性脱钩(let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性)
```
var a = 1;
// 如果在 Node 的 REPL 环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 1

let b = 1;
window.b // undefined
```
## 5. global对象
考虑所有情况,获取顶层对象的两种方法(勉强)
```
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```
