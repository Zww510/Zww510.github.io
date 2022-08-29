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

<div class="font_min">我们来分析一下基本的原理~~~~~~~~~~~~~~~~~~~~</div>
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
onFulfilledCallback = []
//存储失败回调函数
onRejectedCallback = []

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
            // resolve里面将所有成功的回调拿出来执行
            while (onFulfilledCallback.length) {
                // Array.shift() 取出数组第一个元素，然后（）调用，shift不是纯函数，取出后，数组将失去该元素，直到数组为空
                onFulfilledCallback.shift()(value)
            }
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
            onRejectedCallback.shift()(reason)
        }
    }

    //then 的简单实现
    then(onFulfilled, onRejected) {
        // 为了链式调用这里直接创建一个 MyPromise，并在后面 return 出去
        const promise2 = new MyPromise((resolve, reject) => {
            //判断状态
            // 这里的内容在执行器中，会立即执行
            if (this.status === FULFILLED) {
                // 获取成功回调函数的执行结果
                const x = onFulfilled(this.value)
                // 传入 resolvePromise 集中处理
                resolvePromise(x, resolve, reject)
            } else if (this.status === REJECTED) {
                //调用失败的回调，并把原因返回
                onRejected(this.reason)
            } else if (this.status === PENDING) {
                // 因为不知道后面状态的变化，这里先将成功回调和失败回调存储起来
                // 等待后续调用
                onFulfilledCallback.push(onFulfilled)
                onRejectedCallback.push(onRejected)
            }
        })
        return promise2
    }
}

function resolvePromise(x, resolve, reject) {
    // 判断x是不是 MyPromise 实例对象
    if (x instanceof MyPromise) {
        // 执行 x，调用 then 方法，目的是将其状态变为 fulfilled 或者 rejected
        // x.then(value => resolve(value), reason => reject(reason))
        // 简化之后
        x.then(resolve, reject)
    } else {
        // 普通值
        x.then(x)
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

//链式调用
promise.then(value => {
    console.log(1)
    console.log('resolve', value)
    return other()
}).then(value => {
    console.log(2)
    console.log('resolve', value)
})

//输出resolve success
```
##### Promise 中的 .all() , race() , allSettled()
<div class="font_min headers">Promise.all()</div>

* <div class="font_min"><span class="key_txt">Promise.all()</span>方法传入一个数组，如果参数中 Promise 有一个失败，此实例回调失败，失败的原因是第一个 Promise 的结果</div>

<div class="font_min headers">Promise.race()</div>

* <div class="font_min"><span class="key_txt">Promise.race()</span>方法返回一个 Promise，一旦迭代器中的某个 Promise 解决或拒绝，返回的 Promise 就会解决或拒绝。race() 接受的也是数组，不过，得到的却是数组中跑得最快的那个，当最快的一个跑完就立马结束。</div>
* <div class="font_min key_txt">赛跑机制，只认第一名</div>

<div class="font_min headers">Promise.allSettled()</div>

* <div class="font_min"><span class="key_txt">Promise.allSettled()</span>方法返回一个所有给定的 Promise 都已经 fulfilled 或 rejected 后的 Promise，并带有一个对象数组，每个对象表示对应的 Promise 结果。</div>
* <div class="font_min">当所有的 Promise 都被 fulfilled 或 rejected 时，statusesPromise 会解析为一个具有他们状态的数组</div>

```js
/*
{ status: 'fulfilled', value: value } - 如果对应的 Promise 已经被 fulfilled
{ status: 'rejected',  reason: reason} - 如果对应的 Promise 已经被 rejected
*/
```