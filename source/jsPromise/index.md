---
title: Promise
---

##### Promise 核心逻辑实现
<div class="font_min">简单实现以下 Promise 的基础功能，先看原生的 Promise 实现的。</div>

```js
const promise = new Promise((resolve, reject) => {
    resolve('success')
    reject('err')
})

promise.then(value => {
    console.log('resolve',value)
}).catch(err => {
    console.log('reject',err)
})

//输出 resolve success
```

<div class="font_min">我们来分析一下基本原理</div>
<div class="markdown-body">
<div class="font_min">1. Promise 是一个类，在执行这个类的时候会传入一个执行器，这个执行器会立即执行</div>
<div class="font_min">2. Promise 会有三种状态</div>

* <div class="font_min">Pending 等待</div>
* <div class="font_min">Fulfilled 完成</div>
* <div class="font_min">Rejected 失败</div>

<div class="font_min">3. 状态只能有 Pending --> Fulfilled 或者 Pending --> Rejected，且一旦发生改变便不可二次修改</div>
<div class="font_min">4. Promise 中使用 resolve 和 reject 两个函数来改变状态</div>
<div class="font_min">5. then 方法内部做的事情就是状态判断</div>

* <div class="font_min">如果状态是成功，调用成功回调函数</div>
* <div class="font_min">如果状态是失败，调用失败回调函数</div>
</div>

<div class="font_min">下面开始实现</div>

```js
//定义三个常量表示状态
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'

//存储成功回调函数
onFulfilledCallback = null
//存储失败回调函数
onRejectedCallback = null

//新建 MyPromise 类
class MyPromise {
    constructor(executor) {
        //executor 是一个执行器，进入会立即执行
        //并传入resolve和reject方法
        executor(this.resolve, this.reject)
    }

    //存储状态的变量，初始值是pending
    status = PENDING

    //resolve 和 reject 为什么要用箭头函数？
    //如果直接调用的话，普通函数this指向的window或undefined
    //用箭头函数就可以让this指向当前实例对象
    //成功之后的值
    value = null
    //失败之后的值
    reason = null

    //更改成功后的状态
    resolve = (value) => {
        //只有状态是等待，才执行状态修改
        if (this.status === PENDING) {
            //状态修改为成功
            this.status = FULFILLED
            //保存成功之后的值
            this.value = value
            //判断成功回调是否存在，如果存在就调用
            this.onFulfilledCallback && this.onFulfilledCallback(value)
        }
    }

    //更改失败后的状态
    reject = (reason) => {
        //只有状态是等待，才执行状态修改
        if (this.status === PENDING) {
            //状态修改为失败
            this.status = REJECTED
            //保存成功之后的值
            this.reason = reason
            //判断失败回调是否存在，如果存在就调用
            this.onRejectedCallback && this.onRejectedCallback(reason)
        }
    }

    //then 的简单实现
    then(onFulfilled, onRejected) {
        //判断状态
        if (this.status === FULFILLED) {
            //调用成功回调，并且把值返回
            onFulfilled(this.value)
        } else if (this.status === REJECTED) {
            //调用失败的回调，并把原因返回
            onRejected(this.reason)
        } else if (this.status === PENDING) {
            //因为不知道后面状态的变化情况，所以将成功回调和失败回调存储起来
            //等到执行成功失败函数的时候再传送
            this.onFulfilledCallback = onFulfilled
            this.onRejectedCallback = onRejected
        }
    }
}

//异步处理
const promise = new MyPromise((resolve, reject) => {
    setTimeout(() => {
      resolve('success')
    }, 2000); 
  })
  
  promise.then(value => {
    console.log('resolve', value)
  }, reason => {
    console.log('reject', reason)
  })

const promise1 = new MyPromise((resolve, reject) => {
    resolve('success')
    reject('err')
})

promise1.then(value => {
    console.log('resolve', value)
}, reason => {
    console.log('reject', reason)
})

//输出resolve success
```