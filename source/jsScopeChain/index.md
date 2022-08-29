---
title: JS 作用域链
---

## 作用域链
<div class="font_min">当查找变量时，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级（词法层面上的父级）执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链</div>

### 函数创建
<div class="font_min">在<a href="https://github.com/mqyqingfeng/Blog/issues/3" data-hovercard-type="issue" data-hovercard-url="/mqyqingfeng/Blog/issues/3/hovercard">《JavaScript深入之词法作用域和动态作用域》</a>中讲到，函数的作用域在函数定义的时候就决定了</div>
<div class="font_min">这是因为函数有一个内部属性 [[scope]]，当在函数创建的时候，就会保存所有父变量对象到其中，你可以理解 [[scope]] 就是所有父变量对象的层级链，但是注意：[[scope]] 并不代表完整的作用域链</div>
<div class="font_min">例如：</div>

```js
function foo() {
    function bar() {}
}
```
<div class="font_min">函数创建时，各自的[[scope]]为：</div>

```js
foo.[[scope]] = [
    globalContext.VO
]

bar.[[scope]] = [
    fooContext.AO,
    globalContext.VO
]
```

### 函数激活
<div class="font_min">当函数激活时，就会进入函数上下文，创建 VO/AO 后，就会将活动对象添加到作用链的前端</div>
<div class="font_min">这时候执行上下文的作用域链，我们命名为 Scope</div>

```js
Scope = [AO].concat([[Scope]])
```
<div class="font_min">至此，作用域链创建完毕</div>

### 捋一捋
<div class="font_min">以下面的例子为例，结合变量对象和执行上下文栈，总结一下函数执行上下文中作用域链和变量对象的创建过程：</div>

```js
var scope = 'global scope'
function checkscope() {
    var scope2 = 'local scope'
    return scope2
}
checkscope()
```
<div class="font_min">执行过程如下：</div>

1. <div class="font_min">checkscope 函数被创建，保存作用域链到内部属性 [[scope]]</div>
```js
checkscope.[[scope]] = [
    globalContext.VO
]
```
2. <div class="font_min">执行 checkscope 函数，创建 checkscope 函数执行上下文，checkscope 函数执行上下文被压入执行上下文栈</div>
```js
ECStack = [
    checkscopeContext,
    globalContext
]
```
3. <div class="font_min">checkscope 函数并不立刻执行，开始做准备工作，第一步：复制函数 [[scope]] 属性创建作用域链</div>
```js
checkscopeContext = {
    Scope: checkscope.[[scope]]
}
```
4. <div class="font_min">第二步：用户 arguments 创建活动对象，随后初始化活动对象，加入形参，函数声明，变量声明</div>
```js
checkscopeContext = {
    AO: {
        arguments: {
            length: 0
        },
        scope2: undefined
    },
    Scope: checkscope.[[scope]]
}
```
5. <div class="font_min">第三步：将活动对象压入 checkscope 作用域链顶端</div>
```js
checkscopeContext = {
    AO: {
        arguments: {
            length: 0
        },
        scope2: undefined
    },
    Scope: [AO, [[Scope]]]
}
```
6. <div class="font_min">准备工作做完，开始执行函数，随着函数的执行，修改 AO 的属性值</div>
```js
checkscopeContext = {
    AO: {
        arguments: {
            length: 0
        },
        scope2: 'local scope'
    },
    Scope: [AO, [[Scope]]]
}
```
7. <div class="font_min">查找到 scope2 的值，返回后函数执行完毕，函数上下文从执行上下文栈中弹出</div>
```js
ECStack = [
    globalContext
]
```

---
## JS 深入变量对象
<div class="font_min">当 JavaScript 代码执行一段可执行代码（executable code）时，会创建对应的执行上下文（executable context）</div>
<div class="font_min">对于每个执行上下文，都有三个重要属性：</div>

* <div class="font_min">变量对象（varible object, VO）</div>
* <div class="font_min">作用域链（Scope chain）</div>
* <div class="font_min">this</div>

### 变量对象
<div class="font_min">变量对象是与执行上下文相关的数据作用域，存储在上下文中定义的变量和函数声明</div>
<div class="font_min">因为不同执行上下文下的变量对象稍有不同，所以我们来聊聊全局上下文下的变量对象和函数上下文下的变量对象</div>

### 全局上下文
<div class="font_min markdown-body">全局对象是预定义的对象，作为 JavaScript 的全局函数和全局属性的占位符，通过使用全局对象，可以访问所有其他所有预定义的对象，函数和属性</div>
<div class="font_min markdown-body">在顶层 JavaScript 的代码中，可以用关键字 this 引用全局对象，因为全局对象是作用域链的头，这意味着所有非限定性的变量和函数名都会作为该对象的属性来查询</div>
<div class="font_min markdown-body">例如，当 JavaScript 代码引用 parseint() 函数时，它引用的是全局对象的 parseint 属性。全局对象是作用域链的头，还意味着在顶层 JavaScript 代码中声明的所有变量都将成为全局对象的属性</div>

1. <div class="font_min">可以通过 this 引用，在客户端 JavaScript 中，全局对象就是 Window 对象</div>
```js
console.log(this)// window
```
2. <div class="font_min">全局函数是由 Object 构造函数实例化的一个对象</div>
```js
console.log(this instanceof Object)// true
```
3. <div class="font_min">预定义了一堆，一大堆函数和属性</div>
```js
//都能生效
console.log(Math.random())
console.log(this.Math.random())
```
4. <div class="font_min">作为全局变量的宿主</div>
```js
var a = 1
console.log(this.a)// 1
```
5. <div class="font_min">客户端 JavaScript 中，全局对象有 window 属性指向自身</div>
```js
var a = 1
console.log(window.a)// 1

this.window.b = 2
console.log(this.b)// 2
```
<div class="font_min headers">全局上下文中的变量对象就是全局对象！</div>

### 函数上下文
<div class="font_min">在函数上下文中，我们用活动对象（activation object, AO）来表示变量对象</div>
<div class="font_min mar_top">活动对象和变量对象其实是一个东西，只是变量对象是规范上的或者说是引擎实现的，不可在 JavaScript 环境中访问，只有到当进入一个执行上下文中，这个执行上下文的变量对象才会被激活，所有才叫 activation object 呐，而只有被激活的变量对象，也就是活动对象上的各种属性才能被访问</div>
<div class="font_min mar_top">活动对象是在进入函数上下文时刻被创建的，它通过函数的 arguments 属性初始化。arguments 属性值是 Arguments 对象</div>

### 执行过程
<div class="font_min">执行上下文的代码会分成两个阶段进行处理：分析和执行，我们也可以叫做：</div>
<div class="font_min">1. 进入执行上下文</div>
<div class="font_min">2. 代码执行</div>

#### 进入执行上下文
<div class="font_min">当进入执行上下文时，这时候还没有执行代码</div>
<div class="font_min">变量对象会包括：</div>
<div class="font_min">1. 函数的所有形参（如果是函数上下文）</div>

* <div class="font_min">由名称和对应值组成的一个变量对象的属性被创建</div>
* <div class="font_min">没有实惨，属性值设为 undefined</div>

<div class="font_min">2. 函数声明</div>

* <div class="font_min">由名称和对应值（函数对象（function-object））组成一个变量对象的属性被创建</div>
* <div class="font_min">如果变量对象已经存在相同名称的属性，则完全替换这个属性</div>

<div class="font_min">3. 变量声明</div>

* <div class="font_min">由名称和对应值（undefined）组成一个变量对象的属性被创建</div>
* <div class="font_min">如果变量名跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性</div>

<div class="font_min">举个例子：</div>

```js
function foo(a) {
    var b = 2
    function c() {}
    var d = function () {}
    b = 3
}

foo(1)
```

<div class="font_min">在进入执行上下文后，这时候的 AO 是：</div>

```js
AO = {
    arguments: {
        0: 1,
        length: 1
    },
    a: 1,
    b: undefined,
    c: reference to function c() {},
    d: undefined
}
```

#### 代码执行
<div class="font_min">在代码执行阶段，会顺序执行代码，根据代码，修改变量对象的值</div>
<div class="font_min">还是上面的例子，当代码执行完后，这时候的 AO 是：</div>

```js
AO = {
    arguments: {
        0: 1,
        length: 1
    },
    a: 1,
    b: 3,
    c: reference to function c() {},
    d: reference to FunctionExpression "d"
}
```
<div class="font_min">到这里变量对象的创建过程就介绍完毕了，让我们简洁的总结我们上述所说：</div>
<div class="font_min">1. 全局上下文的变量对象初始化是全局对象</div>
<div class="font_min">2. 函数上下文的变量对象初始化只包括 Arguments 对象</div>
<div class="font_min">3. 在进入执行上下文时会给变量对象添加形参，函数声明，变量声明等初始的属性值</div>
<div class="font_min">4. 在代码执行阶段，会再次修改变量对象的属性值</div>