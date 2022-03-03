---
title: 36 个 JS 手写题
---

##### >> 数据类型判断
<div class="font_min">typeof 可以正确的识别：string，number，undefined，boolean，function，symbol 等数据类型，但是对于其他都会认为是 object，比如 null，date 等，所以通过 typeof 来判断数据类型会不准确，
但是可以使用 object.prototype.toString.call() 来实现</div>

```js
function typeof(val) {
    return Object.prototype.toString.call(val).slice(8, -1).toLowerCase()
}
```

##### >> 继承
<div class="font_min">原型链继承</div>

```js
function Animal() {
    this.colors = ['balck', 'white']
}
Animal.prototype.getColor = function() {
    return this.colors
}
function Dog() { }
Dog.prototype = new Animal()

let dog1 = new Dog()
dog1.colors.push('red')
let dog2 = new Dog()
console.log(dog2.colors)
```

* <div class="font_min">1. 原型中包含的引用类型属性将被所有实例共享。</div>
* <div class="font_min">2. 子类在实例化的时候不能给父类构造函数传参。</div>

##### >> 数组去重
```js
//ES5 实现
function unique(arr) {
    var res = arr.filter((item,index,array) => {
        return array.indexOf(item) === index
    })
    return res
}

//ES6 中的 set 实现
const unique = (arr) => {
    return [...new Set(arr)]
}

//利用 reduce 去重
function unique(arr) {
    return Object.keys(arr.reduce((obj,val) => (obj[val] = null, obj),{}))
}
```

##### >> 数组扁平化
<div class="font_min">数组扁平化就是将 [1,[2,[3]]] 这种多层的数组拍平成一层 [1,2,3]。使用 Array.prototype.flat 可以直接将多层数组拍平成一层</div>

```js
[1,[2,[3]]].flat(2) //[1, 2, 3]
```

<div class="font_min">使用 reduce 和递归 来实现</div>

```js
function flatten(arr) {
    return arr.reduce((arr,item) => {
        return arr.concat(Array.isArray(item) ? flatten(item) : item)
    },[])
}

//[1, 2, 3]
```

