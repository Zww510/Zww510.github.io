---
title: JS 继承
---

### 原型链继承
```js
function Parent() {
    this.name = 'kevin'
}

Parent.prototype.getName = function() {
    console.log(this.name)
}

function Child() {}

Child.prototype = new Parent()
var child1 = new Child()
console.log(child1.getName())//kevin
```
<div class="font_min">问题：</div>
<div class="font_min">1.引用类型的属性被所有实例共享，举个例子：</div>

```js
function Parent() {
    this.names = ['kevin', 'daisy']
}
function Child() {}

Child.prototype = new Parent()
var child1 = new Child()
child1.names.push('yayu')
console.log(child1.names)

var child2 = new Child()
console.log(child2.names)
```
<div class="font_min">2.在创建 Child 的实例时，不能向 Parent 传参</div>

### 借用构造函数（经典继承）
```js
function Parent() {
    this.names = ['kevin', 'daisy']
}
function Child() {
    Parent.call(this)
}

var child1 = new Child()
child1.names.push('yayu')
console.log(child1.names)

var child2 = new Child()
console.log(child2.names)
```
<div class="font_min">优点</div>
<div class="font_min">1.避免了引用类型的属性被所有实例共享</div>
<div class="font_min">2.可以在 Child 中向 Parent 传参</div>
<div class="font_min">举个例子</div>

```js
function Parent(name) {
    this.name = name
}
function Child(name) {
    Parent.call(this, name)
}

var child1 = new Child('kevin')
console.log(child1.name)

var child2 = new Child('daisy')
console.log(child2.name)
```
<div class="font_min">缺点：</div>
<div class="font_min">方法都在构造函数中定义，每次创建实例都会创建一遍方法</div>

### 组合继承
<div class="font_min">原型链继承和经典继承的双剑合璧</div>

```js
function Parent(name) {
    this.name = name
    this.colors = ['red', 'blue', 'green']
}
Parent.prototype.getName = function() {
    console.log(this.name)
}

function Child(name, age) {
    Parent.call(this, name)
    this.age = age
}

Child.prototype = new Parent()
Child.prototype.constructor = Child

var child1 = new Child('kevin', 18)
child1.colors.push('black')
console.log(child1)

var child2 = new Child('daisy', 20)
console.log(child2.name)
console.log(child2.age)
console.log(child2.colors)
```
<div class="font_min">优点：融合原型链继承和构造函数的优点，是 JavaScript 中最常用的继承模式</div>

### 原型式继承
```js
function createObj(o) {
    function F() {}
    F.prototype = o
    return new F()
}
```