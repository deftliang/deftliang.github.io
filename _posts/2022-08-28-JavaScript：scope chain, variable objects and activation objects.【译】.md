---
layout: post
title: JavaScript：scope chain, variable objects and activation objects.【译】
subtitle:
date: 2022-08-28
author: deft
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
  - JavaScript
---

这篇文章讨论的是作用域链、变量对象、活动对象以及其关系的基本概念。

### 变量对象

当执行一个 JavaScript 函数时，除了已经创建的全局执行上下文之外，还会创建一个与该函数相关联的执行上下文。变量对象仅仅是一个存储与执行上下文相关的数据的对象。数据包括在上下文中定义的变量和函数声明。让我们考虑下面的例子:


```jsx
function foo(){
    var a = 20;
    function bar(){}
    (function qux(){});
    console.log(a);     // 20
    console.log(bar);   // function bar(){}
    console.log(qux);   // reference error
}
foo();
```

上面的示例中，foo函数的变量对象包含了变量 a 和函数 bar。这里需要主要的一点是，虽然函数生声明包含在变量对象中，但函数表达式并不像我们在示例中看到那样，它表明对 qux 函数的访问会导致引用错误。一个变量对象是抽象和特殊的，它是无法访问的，但它是由JS 引擎处理。

### 活动对象

当调用一个函数时，会创建一个名为 activationobject 的特殊对象，作为变量对象，除此之外，除了变量对象包含的内容之外，它还包含参数对象和形式参数。请看一些示例：


```jsx
function foo(x, y){
    var a = 20;
    function bar(){}
    (function qux(){});
}
```

在上面的示例中，foo ()函数的激活对象包含参数对象 x、 y、变量 a、函数 bar ()。与变量对象类似，函数 qux ()不包含在激活对象中。

作用域链

作用域链是一系列作用域或变量对象，确定了特定执行上下文可以访问哪些变量和函数。让我们看一个例子:


```jsx
var a = 10;
function foo(){
    var b = 12;
    var c = addANumber(20);
    function addANumber(num){
        return a + num;
    }
    console.log(c);     // 30
}
foo();
```

在上面的例子中，在最初的执行，我们的代码会进入全局执行上下文，初始化变量，函数并可能调用其他函数。这些调用将创建其他的执行上下文，与现有的全局执行上下文一起组织称为一个称为“执行上下文堆栈”（execution context stack），全局执行上下文处于栈底中。每个函数在被调用时都会创建一个新的执行上下文，该上下文将被推入执行上下文堆栈中并成为一个活动的执行上下文（active execution context）。当一个函数通过调用另一个函数创建一个新的执行上下文时，它的执行被暂停，控制流将从调用函数传递给新创建的函数。类似地，当函数结束时，它的执行上下文将从执行堆栈中弹出，控制流将交回到执行上下文堆栈中的前一个函数。

在上面的例子中有三个执行上下文: 包含变量 a 和函数 foo ()的全局执行上下文； 包含变量 b、c 和 addANumber 函数的foo函数执行上下文；以及包含参数 num 的 addANumber函数执行上下文。除了变量 b、 c 和 addANnumber ()函数之外，函数 foo ()的上下文还可以访问全局上下文的变量 a。同样，除了变量 num 之外，addANnumber ()函数的上下文还可以访问函数 foo ()的变量 b、 c 和全局上下文的变量 a。但是，全局上下文不能访问 foo ()的上下文，而 foo ()的上下文也不能访问 addANnumber ()的上下文。总之，内部上下文可以访问所有外部包含的上下文，但外部上下文不能访问任何内部上下文。

标识符在一个函数查找过程开始于函数的活动对象，当一个函数引用标识符（可能是一个变量、函数声明或者形式参数）将会在当前的执行上下文中查找这个标识符。如果找不到它，那么会继续在包含这个上下文的作用域链向上中继续搜索，如果在作用域链的末端还找不到，就会被认为是undefined。在上面的例子中，当变量 a 在 addANnumber ()函数中被引用时，该变量将在函数 addANnumber ()的当前执行上下文中被查找，在这里它不能被找到，然后它将在 foo ()函数的执行上下文中被查找，它也不能在这里被找到，最后，它在全局上下文中被找到。

下面的图片解释了这个过程：

![1_9t59w1tOW-iOTJFheu6Fgg (1).png](https://deftliang.github.io/img/in-post/1_9t59w1tOW-iOTJFheu6Fgg.png)


虽然开发人员不直接使用这些概念, 但深入细致地理解它们是很重要的。


[原文连接](https://medium.com/@klausng/javascript-scope-chain-variable-objects-and-activation-objects-4eb017256d0b)
