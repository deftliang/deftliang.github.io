---
layout:     post
title:      JavaScript中的作用域、作用域链和闭包
subtitle:  
date:       2020-05-30
author:     deft
header-img: img/post-bg-e2e-ux.jpg
catalog: true
tags:
    - JavaScript
---
#### 一、作用域  
JavaScript中变量的作用域有两种: **全局作用域** 和 **函数作用域** (ES6中js引入块级作用域这里先不做研究)  
###### 全局作用域:  
代码在程序的任何地方都能被访问，window 对象的内置属性都拥有全局作用域。  
```javascript
var carName = " Volvo";
// 此处可调用 carName 变量
function myFunction() {
    carName = "Volvo";
    // 函数内可调用 carName 变量
}
```
  这里值得注意的是如果变量在函数内没有声明（没有使用 var 关键字），该变量为全局变量。  
  隐式全局变量 : 声明的变量没有var,就叫隐式全局变量；
  ```javascript
      function fn(){
         innerVar = "inner";  //这里没有用var声明所有innerVar是全局变量
      }
      fn();
      console.log(innerVar);// result:inner

```
  
###### 局部作用域：
和全局作用域相反，局部作用域一般只在固定的代码片段内可访问到，而对于函数外部是无法访问的，最常见的例如函数内部  

```javascript
function fn(){
	var innerVar = "inner";
}
fn();
console.log(innerVar);// 发出错误:ReferenceError: innerVar is not defined

```

#### 二、作用域链
作用域链（自由变量的查找)  
变量与函数的查找规则: 当我们调用一条数据的时候，js首先会在当前作用域中进行查找，如果找不到，就向上找到父级的作用域，如果在父级的作用域中也找不到，就继续向上查找，直到window的作用域。如果在window中也找不到，就报错了。  

#### 三、闭包（Closure）  
闭包的概念: 闭包是一个拥有许多变量和绑定了这些变量的环境的表达式(通常是一个函数);
"闭包"（closure）定义是非常抽象的。这里引用阮一峰大牛的理解：闭包就是能够读取其他函数内部变量的函数。
由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。
所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。  


```javascript
   function f1(){
       var n=999;
       function f2(){
          alert(n);
       }
       return f2;
    }
    var result=f1();
    result(); // 999
```


闭包的用途

闭包可以用在许多地方。它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。  

使用闭包的注意点

1）由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

2）闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

