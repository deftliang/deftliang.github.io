---
layout:     post
title:      What is Ajax？
subtitle:  
date:       2020-05-30
author:     deft
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - AJAX
    - JavaScript
---
## AJAX
#### 什么是AJAX?  
所谓的Ajax就是 Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。  
它不是新的编程语言，而是一种使用现有标准的几种原有技术的结合体。Ajax是一种在无需重新加载整个网页的情况下能够更新部分网页的技术。  

#### 那Ajax的优点那些？  
1，通过异步模式，提升了用户体验  
2，优化了浏览器和服务器之间的传输，减少了不必要的数据往返，减少了宽带占用  
3，Ajax引擎在客户端运行，承担了一部分本来由服务器承担的工作，从而减少了大用户量下的服务器负载  

#### Ajax的缺点呢？  
不支持浏览器back按钮  
安全问题AJAX暴露了与服务器交互的细节  
对搜索引擎的支持比较弱  

要了解Ajax那么我们就要了解XMLHttpRequest了  
#### XMLHttpRequest 对象  
所有现代浏览器均支持 XMLHttpRequest 对象（IE5 和 IE6 使用 ActiveXObject）。  
XMLHttpRequest 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。它可以向服务器提出请求并处理响应，而不阻塞用户， 可以在页面加载以后进行页面的局部更新

#### 如何使用Ajax？  
要完整实现一个Ajax异步调用和局部刷新，需要完成下面几个步骤。  
1，创建XMlHttpRequest对象，也就是异步调用对象  
2，创建一个新的HTTP请求，并且指定该HTTP请求的方法、URL  
3，设置响应HTTP请求状态变化的函数  
4，发送HTTP请求  
5，获取异步调用返回给我们的数据  
6，最后用JavaScript和DOM实现局部刷新  

```javascript
/** 1. 创建连接 **/
var xhr = null
xhr = new XMLHttpRequest()
/** 2. 连接服务器 **/
xhr.open('get', url, true)
/** 3. 发送请求 **/
xhr.send(null)
/** 4. 接受请求 **/
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4) {
    if (xhr.status == 200) {
      success(xhr.responseText)
    } else {
      /** false **/
      fail && fail(xhr.status)
    }
  }
}
```
###### AJAX状态码    
| 状态码  |  说明 |  
| ------------ | ------------ |  
|  0 | 初始化，尚未调用open()方法  |  
|  1 | 调用open()，已经调用send()的方法，正在发送请求  |  
|  2 | 发送：已经调用send()方法，已接收到响应  |  
|  3 | 解析正在解析响应数据  |  
|  4 | 成功  |     
 
###### 服务器状态码  
| 状态码  | 说明  |  
| ------------ | ------------ |  
| 200  | 成功  |  
| 301  | 永久重定向  |  
| 404  | 未找到对应文件  |  
| 500  | 服务器错误  |  
  
