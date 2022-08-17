---
title: JS 原型链
---

### 构造函数创建对象
<div class="font_min">我们先使用构造函数创建一个对象</div>

```js
function Person() {}

var person = new Person()
person.name = 'kevin'
console.log(person.name) //kevin
```
<div class="font_min">在这个例子中，Person 就是一个构造函数，使用了 new 创建一个实例对象 Person</div>

### prototype
<div class="font_min">每个函数都有 prototype 属性，如：</div>

```js
function Person() {}

Person.prototype.name = 'kevin'
var person1 = new Person()
var person2 = new Person()
console.log(person1.name) //'kevin'
console.log(person2.name) //'kevin'
```
<div class="font_min">函数的 prototype 属性指向了一个对象，这个对象正是调用该构造函数而创建的实例的原型，也就是 person1 和 person2 的原型</div>
<div class="font_min">原型可以理解为：每一个 JavaScript 对象（null 除外）在创建的时候就会与之关联另外一个对象，这个对象就是我们所说的原型，每一个对象都会从原型 “继承” 属性</div>
<div class="font_min">一张图表示构造函数和实例原型之间的关系</div>
<img src="https://camo.githubusercontent.com/02789d6806b75d34b2017021f58efa3aa7a2ee6be8a0c05fb3293438884b9ec0/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f70726f746f74797065312e706e67" alt="构造函数和实例原型的关系图" data-canonical-src="https://cdn.jsdelivr.net/gh/mqyqingfeng/Blog/Images/prototype1.png" style="max-width: 100%;">

### __proto__
<div class="font_min">每一个 JavaScript 对象（null 除外）都具有的一个属性，叫 __proto__，这个属性会指向该对象的原型</div>
<div class="font_min">我们可以在火狐或谷歌中输入验证这一点</div>

```js
function Person() {}

var person = new Person()
console.log(person.__proto__ === Person.prototype)
```
<div class="font_min">于是我们更新下关系图</div>
<img src="https://camo.githubusercontent.com/3dde335faa15d03ffe3b907f6e5c2b5f4d2183caa4c47ac7486794bc407f663c/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f70726f746f74797065322e706e67" alt="实例与实例原型的关系图" data-canonical-src="https://cdn.jsdelivr.net/gh/mqyqingfeng/Blog/Images/prototype2.png" style="max-width: 100%;">

### constructor
<div class="font_min">一个构造函数可以生成多个实例，原型指向构造函数的第三个属性：constructor，每个原型都有一个 constructor 属性指向关联的构造函数</div>
<div class="font_min">我们可以尝试一下：</div>

```js
function Person() {}

console.log(Person === Person.prototype.constructor)
```
<div class="font_min">然后再更新下图</div>
<img src="https://camo.githubusercontent.com/0aaf005afda83d4e2fdd2bbe523df228b567a091317a2154181771b2706ea2ef/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f70726f746f74797065332e706e67" alt="实例原型与构造函数的关系图" data-canonical-src="https://cdn.jsdelivr.net/gh/mqyqingfeng/Blog/Images/prototype3.png" style="max-width: 100%;">

<div class="font_min">综上我们已经得出：</div>

```js
function Person() { }

var person = new Person()
console.log(person.__proto__ === Person.prototype)//true
console.log(Person.prototype.constructor === Person)//true
//ES5的方法，可以获取对象的原型
console.log(Object.getPrototypeOf(person) === Person.prototype)//true
```

### 实例与原型
<div class="font_min">当读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查找不到，就去找原型中的原型，一直找到最顶层为止</div>
<div class="font_min">举个例子：</div>

```js
function Person() {}

Person.prototype.name = 'kevin'
var person = new Person()
person.name = 'Daisy'
console.log(person.name)//Daisy

delete person.name
console.log(person.name)//kevin
```
<div class="font_min">在这个例子中，我们给实例对象 person 添加了 name属性，当我们打印 person.name 的时候，结果自然为 Daisy</div>
<div class="font_min">但是当我们删除了 person 的 name 属性时，读取 person.name，从 person 中查找不到该属性，于是就会从 person 的原型也就是 person.__proto__，也就是 Person.prototype 中查找，我们可以找到 name 属性，值为kevin</div>
<div class="font_min">但是万一还没有找到呢，原型的原型又是什么</div>

### 原型的原型
<div class="font_min">在前面，已经讲了原型也是一个对象，既然是对象，我们就可以用最原始的方式创建它，那就是：</div>

```js
var obj = new Object()
obj.name = 'kevin'
console.log(obj.name)
```
<div class="font_min">其实原型对象就是通过 Object 构造函数生成的，结合之前所讲，实例的 __proto__ 指向构造函数的 prototype，所以我们在更新下关系图：</div>
<img src="https://camo.githubusercontent.com/ad0ee0e2594c1ac471bbb42321963c130f4fe1ef9ec70389c8ced54544d3fd6c/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f70726f746f74797065342e706e67" alt="原型的原型关系图" data-canonical-src="https://cdn.jsdelivr.net/gh/mqyqingfeng/Blog/Images/prototype4.png" style="max-width: 100%;">

### 原型链
<div class="font_min">那 Object.prorotype 的原型呢？</div>
<div class="font_min">null，我们可以打印：</div>

```js
console.log(Object.prototype.__proto__ === null)
```
<div class="font_min">然而 null 究竟代表了什么呢</div>
<div class="font_min markdown-body">null 表示没有对象，即该处不应该有值</div>
<div class="font_min">所以 Object.prototype.__proto__ 的值为 null 跟 Object.prorotype 没有原型，其实表达了一个意思</div>
<div class="font_min">所以查找属性的时候，查到 Object.prototype 就可以停止查找了</div>
<div class="font_min">最后一张关系图也可以更新为：</div>
<img src="https://camo.githubusercontent.com/9a69b0f03116884e80cf566f8542cf014a4dd043fce6ce030d615040461f4e5a/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f70726f746f74797065352e706e67" alt="原型链示意图" data-canonical-src="https://cdn.jsdelivr.net/gh/mqyqingfeng/Blog/Images/prototype5.png" style="max-width: 100%;">
<div class="font_min">图中由相互关联的原型组成的链状结构就是原型链，也就是蓝色的这条线</div>