---
layout: post
title: 搞懂useState和useEffect学习记录
subtitle:
date: 2022-02-3
author: deft
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
  - JavaScript
---
# 搞懂useState和useEffect


读神光的[文章](https://zhuanlan.zhihu.com/p/608959809?utm_campaign=shareopn&utm_medium=social&utm_oi=613394108539080704&utm_psn=1612462935640641537&utm_source=wechat_session)写的一些笔记


![Untitled-2023-02-24-1449.png](https://deftliang.github.io/img/in-post/react-reconcile.png)

jsx 通过 render function 转变为 vdom. vdom 会在 schedule 安排下去 reconcile 成 fiber

diff 算法就是在 reconcile 过程中

副作用函数, 也就是 useEffect, 生命周期等函数, 会在 reconcile 结束之后处理

所以 react 渲染流程大体上分为两个阶段: render 阶段 和 commit 阶段

commit 还分为三个小阶段: before mutation, mutation 和 layout

layout 阶段在操作 dom 之后，所以这个阶段是能拿到 dom 的，**ref 更新**是在这个阶段，**useLayoutEffect 回调函数的执行**也是在这个阶段。

**总结: react 的渲染流程 render + commit(before mutation、mutation、layout)**

hooks 中的数据存放位置?  像 useState中state, useRef中ref等

hook api 在 fiber 的 memoizedState 链表结构中获取到数据.


state 的更新, 以及重新渲染:

useState 的 mountState 阶段返回的 setXxx 是绑定了几个参数的 dispatch 函数。执行它会创建 hook.queue 记录更新，然后标记从当前到根节点的 fiber 的 lanes 和 childLanes 需要更新，然后调度下次渲染。

下次渲染执行到 updateState 阶段会取出 hook.queue，根据优先级确定最终的 state，最后返回来渲染。

这样就实现了 state 的更新和重新渲染。
