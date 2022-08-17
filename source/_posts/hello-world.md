---
title: Js - Es6 - 操作
categories: 搭建博客
---

### 数组转为 tree 结构

``` js
const arrayToTree = (item) => {
    let arr = []
    let obj = {}
    item.forEach(i => {
        if(!obj[i.id]) {
            obj[i.id] = { ...i, children: [] }
        }
        const newObj = obj[i.id]
        if(i.pid === 0) {
            arr.push(newObj)
        }else {
            if(!obj[i.pid]) {
                obj[i.pid] = { children: [] }
            }
            obj[i.pid].children.push(newObj)
        }
    })
    return arr
}
let arr = [{id: 1, name: '部门1',pid: 0},{id: 2, name: '部门2',pid: 1}]
```

### tree 扁平化

``` js
//利用递归和循环
const treeToArray = (tree) => {
    let arr = []
    tree.forEach(item => {
        const { children, ...i } = item
        // if (children && children.length) {
        //     arr = arr.concat(treeToArray(children))
        // }
        children && children.length ? arr = arr.concat(treeToArray(children)) : ''
        arr.push(i)
        })
        return arr
    }
```
``` js
//利用reduce 和 递归
const treeToArray = (tree) => {
    return tree.reduce((arr,item) => {
        const { children, ...i } = item
        return arr.concat(i, children && children.length ? treeToArray(children) : [])
    },[])
}
```

### 多维数组扁平化

```js
const flatten = (arrTree) => {
    return arrTree.reduce((arr, item) => {
        return arr.concat(Array.isArray(item) ? flatten(item) : item)
    },[])
}
// let arr = [1,[2,[3,4,5]]]
// [1, 2, 3, 4, 5]
```

### 数组去重
##### ES6 中的 Set 去重
```js
const noRepetition = (arr) => {
    return [...new Set(arr)]
}
```
##### 利用 reduce 去重
```js
const noRepetition = (arr) => {
    return Object.keys(arr.reduce((obj, val) => (obj[val] = null, obj), {}))
}
```
##### 利用 filter 和 indexOf 去重
```js
const noRepetition = (arr) => {
    return arr.filter((item, index, array) => (index === array.indexOf(item)))
}
//let a = ['1', '2', '3', '1', 'a', '2', 'b', 'b']
```

### 翻转字符串
```js
const reverseString = (str) => {
    return [...str].reverse().join('')
}
```

### 计算数组中某个值出现的次数
```js
const countOccurrences = (arr, key) => arr.filter(item => item === key).length
//countOccurrences([2, 1, 3, 3, 2, 3, 3], 2)
```

### 计算数组元素出现次数
```js
const repetition = (arr) => arr.reduce((obj,item) => (obj[item] = ++obj[item] || 1, obj), {})
//repetition([1, 2, 2, 3, 3, 3, 4, 4, 4, 4])
// {1: 1, 2: 2, 3: 3, 4: 4}
```

### 数组合并成数组对象
```js
const zip = ([x, ...xs], [y, ...ys]) => {
    if(x === undefined || y === undefined) return []
    return [[x, y], ...zip(xs, ys)]
}
let ab = [1, 2, 3, 4, 5]
let ac = ['啊', '我', '额', '人', '他']
let arr = zip(ab, ac).map(([name, value]) => ({name, value}))
// [{name: 1, value: '啊'},{name: 2, value: '我'}]
```

### 利用 reduce 累计加乘
##### 相加
```js
const accumulation = (...val) => val.reduce((num, item) => (num + item), 0)
```
##### 想乘
```js
const multiplicaiton = (...val) => val.reduce((num, item) => (num * item), 1)
```

### 插件
<div class="font_min">深拷贝插件</div>
<div class="font_min key_txt">lodash</div>
