---
title: JS 工具函数
---

#### 数组
##### all: 布尔全等判断
```js
const all = (arr, fn = Boolean) => arr.every(fn)

all([2,3,4,5], x => x > 1) //true
all([{show: true}, {show: true}, {show: true}], x => x.show === true) //true
```

##### allEqual: 检查数组各项相等
```js
const allEqual = arr => arr.every(val => val === arr[0])

allEqual([1,2,3]) //false
allEqual([1,1,1]) //true 
```

##### countOccurrences: 检查数值出现次数
```js
const countOccurrences = (arr, val) => arr.reduce((a, v) => (v === val ? a + 1 : a), 0)

console.log(countOccurrences([1,2,3,1,4,1], 1)) //3
```

##### intersection: 两数组的交集
```js
const intersection = (a, b) => {
    const s = new Set(b)
    return a.filter(x => s.has(x))
}

intersection([1,2,3,6,7],[1,2,3,4]) //[1,2,3]
```

##### sample: 在指定数组中获取随机数
```js
const sample = arr => arr[Math.floor(Math.random() * arr.length)]

sample([3,7,9,11])
```