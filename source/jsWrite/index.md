---
title: 36 个 JS 手写题
---

##### 数据类型判断
<div class="font_min">typeof 可以正确的识别：string，number，undefined，boolean，function，symbol 等数据类型，但是对于其他都会认为是 object，比如 null，date 等，所以通过 typeof 来判断数据类型会不准确，
但是可以使用 object.prototype.toString.call() 来实现</div>

```js
function typeof(val) {
    return Object.prototype.toString.call(val).slice(8, -1).toLowerCase()
}
```

##### 继承
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

##### 数组去重
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

##### 数组扁平化
<div class="font_min">数组扁平化就是将 [1,[2,[3]]] 这种多层的数组拍平成一层 [1,2,3]。使用 Array.prototype.flat 可以直接将多层数组拍平成一层</div>

```js
[1,[2,[3]]].flat(2) //[1, 2, 3]
[1,[2,[3]]].flat(Infinity) //[1, 2, 3]
```
<div class="font_min">其中使用<span class="key_txt">Infinity</span>作为 flat 的参数，使得无需知道被扁平化的数组的维度</div>

<div class="font_min">使用 reduce 和递归 来实现</div>

```js
function flatten(arr) {
    return arr.reduce((arr,item) => {
        return arr.concat(Array.isArray(item) ? flatten(item) : item)
    },[])
}

//[1, 2, 3]
```

##### call
<div class="font_min">我们可以模拟的步骤可以分为</div>

* <div class="font_min">1. 将函数设为对象的属性</div>
* <div class="font_min">2. 执行该函数</div>
* <div class="font_min">3. 删除该函数</div>

```js
Function.prototype.call2 = function (context = window, ...args) {
    context.fn = this
    let result = context.fn(...args)
    delete context.fn
    return result
}
```

##### apply

```js
Function.prototype.apply2 = function (context = window) {
    context.fn = this
    let result
    if (arguments[1]) {
        result = context.fn(...arguments[1])
    } else {
        result = context.fn()
    }
    delete context.fn
    return result
}
```

##### bind

```js
Function.prototype.bind2 = function (context) {
    var _this = this
    var args = [...arguments].slice(1)
    return function F() {
        if(this instanceof F) {
            return  _this(...args, ...arguments)
        }
        return _this.apply(context, args.concat(...arguments))
    }
}
```

##### reduce
<div class="font_min">如果我们要实现一个功能将函数里的元素全部相加得到一个值，可能会这样写</div>

```js
const arr = [1, 2, 3]
let total = 0
for (let i = 0; i < arr.length; i++) {
  total += arr[i]
}
console.log(total) //6 
```

<div class="font_min">但是如果我们使用 reduce 的话就可以遍历部分的代码优化为一行代码</div>

```js
const arr = [1,2,3]
const sum = (arr) => arr.reduce((num, item) => num + item, 0)
console.log(sum(arr))//6
```

<div class="font_min">对于 reduce 来说，它接受两个参数，分别是回调函数和初始值，接下来我们来分解上诉代码中 reduce 过程</div>

* <div class="font_min">首先初始值为 <span class="key_txt">0</span>，该值会在执行第一次回调函数时作为第一个参数传入</div>
* <div class="font_min">回调函数接受4个参数，分别是累计值，当前元素，当前索引，原数组。后三者都明白作用，这里着重分析第一个参数</div>
* <div class="font_min">在第一次执行回调函数时，当前值和初始值相加得出结果 <span class="key_txt">1</span>，该结果会在第二次执行回调函数时当做第一参数传入</div>
* <div class="font_min">所以在第二次执行回调函数时，相加的值分别就是 <span class="key_txt">1</span> 和 <span class="key_txt">2</span>，以此类推，循环结束后得到结果 <span class="key_txt">6</span>。</div>

##### 空值合并运算符
<div class="font_min">我们在后端接口获取数据时，经常会遇到后端缺字段的情况，导致前端不得不去做空兼容，防止后端不给字段</div>
```js
const price = res.data?.price ?? '暂无定价'
```

##### JSON对象
<div class="font_min">JSON对象有两个办法：stringify() 和 parse()。在简单的情况下，这两个方法分别可以将 JavaScript 序列化为 JSON 字符串，以及将 JSON 解析为原生 JavaScript 值</div>
<div class="font_min">实际上，JSON.stringify() 方法除了要序列化的对象，还可以接受两个参数。这两个参数可以用于指定其他序列化 JavaScript 对象的方式，第一个参数是过滤器，可以是数组或函数。第二个参数是用于缩进结果 JSON 字符串的选项。单独或组合使用这些参数可以更好的控制 JSON 序列化</div>

<div class="font_min mar_top">1.过滤结果</div>

* <div class="font_min">如果第二个参数是一个数组，那么 JSON.stringify() 返回的结果只会包含该数组中列出的对象属性。如下例子：</div>
```js
let book = { 
    title: "Professional JavaScript", 
    authors: [ 
        "Nicholas C. Zakas", 
        "Matt Frisbie" 
    ], 
    edition: 4, 
    year: 2017 
}
let jsonText = JSON.stringify(book, ['title', 'edition'])
//{"title":"Professional JavaScript","edition":4}
```
<div class="font_min">在这个例子中，JSON.stringify() 方法的第二个参数是一个包含两个字符串的数组，'title' 和 'edition'。它们对应着要序列化的对象的属性，因此结果 JSON 字符串中只会包含这两个属性</div>

* <div class="font_min">如果第二个参数是函数，则行为又有不同。提供的函数接受两个参数：属性名（key）和属性值（value）</div>
<div class="font_min">注意：<span class="key_txt">返回 undefined 会导致属性会被忽略。如下例子：</span></div>
```js
let book = {
            title: "Professional JavaScript",
            authors: [
                "Nicholas C. Zakas",
                "Matt Frisbie"
            ],
            edition: 4,
            year: 2017
        }
        let jsonText = JSON.stringify(book, (key, value) => {
            switch (key) {
                case 'authors':
                    return value.join(',')
                case 'year':
                    return 5000
                case 'edition':
                    return undefined
                default:
                    return value
            }
        })
//最终的到的 JSON 字符串是这样的： {"title":"Professional JavaScript","authors":"Nicholas C. Zakas,Matt Frisbie","year":5000}
```