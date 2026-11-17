---
author: deft
pubDatetime: 2023-05-27T00:00:00.000Z
title: "React学习之hooks中的闭包问题以及解决策略，useLatest的使用"
slug: react学习之hooks中的闭包问题以及解决策略uselatest的使用
featured: false
draft: false
tags:
  - "JavaScript"
description: "react hooks 使用中的闭包问题"
---


# react hooks 使用中的闭包问题

案例如下：

```jsx
import { useState } from 'react'

export default function () {
  const [count, setCount] = useState(0)

  useEffect(() => {
    setInterval(() => {
      console.log('count: ', count)
    }, 1000)
  }, [])

  return (
    <div>
      <p>count: {count}</p>
      <button onClick={() => setCount(v => v + 1)}></button>
    </div>
  )
}
```

点击按钮修改 count 的值时，发现 setInterval 中打印的值始终为 0，这就是闭包产生的问题。

## 使用 useLatest 解决此问题

```jsx
import React, { useState, useEffect } from 'react'
import { useLatest } from 'ahooks'

export default () => {
  const [count, setCount] = useState(0)

  const latestCountRef = useLatest(count)

  useEffect(() => {
    const interval = setInterval(() => {
      setCount(latestCountRef.current + 1)
    }, 1000)
    return () => clearInterval(interval)
  }, [])

  return (
    <>
      <p>count: {count}</p>
    </>
  )
}
```

ahooks 中 useLatest 源码

```jsx
import { useRef } from 'react'

function useLatest<T>(value: T) {
  const ref = useRef(value)
  ref.current = value

  return ref
}

export default useLatest
```

* 资料参考：
  * [https://blog.csdn.net/hhhhhhaaaaaha/article/details/127070064]()
  * [https://blog.logrocket.com/what-you-need-know-react-useevent-hook-rfc/]()
