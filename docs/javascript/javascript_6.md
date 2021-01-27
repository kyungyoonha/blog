---
id: javascript_6
title: ※ 유용
sidebar_label: Object.entries(obj)
---

-   key / value 같이 사용할때 유용하다.

```JSX
const obj = {
  a: '1',
  b: '2',
  c: '3',
  d: '4',
  e: '5',
}

Object.entries(obj).forEach(([key, value]) => {
  console.log(`KEY:${key} / VALUE: ${value}`)
}

// KEY:a / VALUE: 1
// KEY:b / VALUE: 2
// KEY:c / VALUE: 3
// KEY:d / VALUE: 4
// KEY:e / VALUE: 5
```
