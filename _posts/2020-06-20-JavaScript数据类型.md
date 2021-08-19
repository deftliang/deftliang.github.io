---
layout:     post
title:      JavaScript 的数据类型
subtitle:  
date:       2020-06-20
author:     deft
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - JavaScript
---

### 数据类型可以分为**基本数据类型**和**引用数据类型**  
基本数据类型 ：`String`、`Number`、`Boolean` 、`Null`、`Undefined`、`Symbol`、`BigInt` ;  
引用数据类型: `Object`  
其中 `Symbol`、`BigInt` 是新增的数据类型  

JS中所有对象都派生自Object;  

Undefined类型只有一个值,即特殊的undefined,所以undefined可以当做关键字来进行判断：  

引用数据类型开辟一个空间运行在开辟的空间中，运行在堆内存中；function预解释的时候当做字符串储存在栈内存中， 这时候引用仅仅只是一个内存地址；当运行的时候，这个堆内存里面的内容当做代码从上而下的以此执行  

### 不同类型的数据储存原理  
栈：原始数据类型（Undefined，Null，Boolean，Number、String）  

堆：引用数据类型（对象、数组和函数）  




### 两种类型的区别是：存储位置不同  
原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；  

引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定,如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。  


当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体  

### null与undefined知识点
undefined值派生自null,所以相等比较null==undefined为true;  

null是空指针但未用,有个占位符,undefined连指针都没有,也没有占位符,所以绝对比较null===undefined为false。  

### 实用的判断类型方法
简单的判断数据类型 typeof就可以了，但是如果严格的来判断，可以使用下面的这个方法  

    let _typeof = function (data) {
        let value = /\[object (\w+)\]/.exec(
            Object.prototype.toString.call(data) // 核心代码
        );
        return value ? value[1].toLowerCase() : '';
    }
