---
title: JavaScript 执行机制
---

##### 关于 JavaScript
<div class="font_min">javascript 是一门单线程语言，在最新的 HTML5 中提出了 web-worker，但 javascript 是单线程这一核心从未改变。所以一切 javascript 版的"多线程"都是用单线程模拟出来的。</div>

##### JavaScript 事件循环
<div class="font_min">既然 JS 是个单线程，那就像只有一个窗口的银行，客户需要排队一个一个办理业务，同理 js 任务也要一个一个顺序执行。如果一个任务耗时过长，那么后一个任务也必须等着。那么问题来了，如我们想浏览新闻，但是新闻包含的超清图片加载很慢，难道我们的网页要一直卡着知道图片完全显示出来？因此聪明的程序员将任务分为两类：</div>

* <div class="font_min key_txt">同步任务</div>
* <div class="font_min key_txt">异步任务</div>

<div class="font_min">当我们打开网站时，网页的渲染过程就是一大堆同步任务，也如页面骨架和页面元素的渲染。而像加载图片音乐之类的占用资源大耗时久的任务，就是异步任务。</div>
<image style="transform: scale(0.8);" src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2017/11/21/15fdd88994142347~tplv-t2oaga2asx-watermark.awebp" />

<div class="font_min">导图要表达的内容用文字来表述的话：</div>

* <div class="font_min">同步和异步分别进入不同的执行"场所"，同步进入主线程，异步进入 Event Table 并注册函数。</div>
* <div class="font_min">当指定的事情完成时，Event Table 会将这个函数移入 Event Queue（事件队列）。</div>
* <div class="font_min">主线程内的任务执行完毕为空，会去 Event Queue 中读取对应的函数，并进入主线程执行。</div>
* <div class="font_min">上述过程会不断重复，也就是常说的 Event Loop（事件循环）。</div>

<div class="font_min">我们不经要问了，那如何知道主线程执行栈为空？ js 引擎存在 monitoring process 进程（监视进程），会持续不断的检查主线程执行栈是否为空，一旦为空，就会去 Event Queue 那里检查是否有等待被调用的函数。</div>

```bash
let data = [];
$.ajax({
    url:www.javascript.com,
    data:data,
    success:() => {
        console.log('发送成功!');
    }
})
console.log('代码执行结束');
```

* <div class="font_min">ajax 进入到 Event Table，注册回调函数 <span class="key_txt">success</span>。</div>
* <div class="font_min">执行 <span class="key_txt">console.log('代码执行结束')</span>。</div>
* <div class="font_min">ajax 事件完成后，回调函数 <span class="key_txt">success</span> 进入 Event Queue。</div>
* <div class="font_min">主线程从 <span class="key_txt">Event Queue</span> 读取回调函数 success 并执行。</div>

##### 又爱有恨的 setTimeout
<div class="font_min">大名鼎鼎的 setTimeout 无需多言，大家对他的第一印象就是异步可以延时执行，我们经常实现3秒执行：</div>
```bash
setTimeout(() => {
    console.log('延时3秒')
},3000)
```
<div class="font_min">渐渐的 setTimeout 用的地方多了，问题也出现了，有时明明写的是延时3秒，实际确5，6秒才执行函数，这又是咋回事？</div>
<div class="font_min">先看个例子？</div>
```bash
setTimeout(() => {
    task()
}, 3000)
console.log('执行console')
```
<div class="font_min"><span class="key_txt">setTimeout</span> 是异步的，应该会先执行 <span class="key_txt">console.log</span> 这个同步任务，所以结论是：</div>
```bash
//执行console
//task()
```
<div class="font_min">然后我们修改一下前面的代码</div>
```bash
setTimeout(() => {
    task()
}, 3000)
function sleep (time) {
  return new Promise((resolve) => setTimeout(resolve, time));
}
sleep(10000000).then(res => {
    console.log('我是sleep方法')
})
```
<div class="font_min">这段代码在 chrome 执行一下，却发现控制台执行 <span class="key_txt">task()</span> 需要的时间远远超过3秒，说好的延迟3秒，为啥现在需要这么长时间？</div>
<div class="font_min">这时候我们需要重新理解 <span class="key_txt">setTimeout</span> 的定义，我们先说上述代码是如何执行的：</div>

* <div class="font_min"><span class="key_txt">task()</span> 进入 Event Table 并注册，计时开始。</div>
* <div class="font_min">执行 <span class="key_txt">sleep</span> 函数，很慢，非常慢，计时仍在继续。</div>
* <div class="font_min">3秒到了，计时器 <span class="key_txt">timeout</span> 完成，<span class="key_txt">task()</span> 进入 Event Queue，但 <span class="key_txt"></span>sleep 也太慢了，还没执行完，只好等待。</div>
* <div class="font_min"><span class="key_txt">sleep</span> 终于执行完了，<span class="key_txt">task()</span> 终于从 Event Queue 进入主线程执行。</div>

<div class="font_min">上述流程走完，我们知道 setTimeout 这个函数，是经过指定时间后，把要执行的任务（本例的 task()）加入到 Event Queue 中，又因为是单线程任务要一个一个执行，如果前面的任务需要的时间太久，那么只能等着，导致真正延迟时间远远大于3秒。</div>
<div class="font_min">经常遇到 <span class="key_txt">setTimeout(fn, 0)</span>这样的代码，0秒后执行又是什么意思呢？是不是可以立即执行？</div>
<div class="font_min"><span class="key_txt">不会立即执行</span>，setTimeout(fn, 0) 的含义是，指定某个任务在主线程最早可得的空闲时间执行，意思就是说不用在等多少秒了，只要主线程执行栈内的同步任务全部执行完毕，栈为空就会马上执行。举例说明：</div>

```bash
//代码1
console.log('先执行这里');
setTimeout(() => {
    console.log('执行啦')
},0);

//代码2
console.log('先执行这里');
setTimeout(() => {
    console.log('执行啦')
},3000);  

//代码1的输出结果是：
//先执行这里
//执行啦

代码2的输出结果是：
//先执行这里
// ... 3s later
// 执行啦
```

##### 又爱有恨的 setInterval
<div class="font_min">他和 setTimeout 差不多，只不过 <span class="key_txt">setInterval</span> 是循环的执行。对于执行顺序来说，<span class="key_txt">setInterval</span> 会每隔指定的时间将注册的函数放置入 <span class="key_txt">Event Queue</span>，如果前面的任务耗时太久，那么同样需要等待。</div>

##### Promise 与 process.nextTick(callback)
<div class="font_min"><span class="key_txt">Promise</span>的定义不在叙述，可以学习阮一峰老师的 <a class="headers" href="https://es6.ruanyifeng.com/#docs/promise">Promise</a></div>
<div class="font_min">除了广义的同步任何和异步任务，我们对任务有更精细的定义：</div>

* <div class="font_min"><span class="key_txt">宏任务（macro-task）</span>：包括整体代码 script, setTimeout, setInterval</div>
* <div class="font_min"><span class="key_txt">微任务（micro-task）</span>：Promise, async/await</div>

<div class="font_min">不同类型的任务会进入对应的 Event Queue，比如 <span class="key_txt">setTimeout</span> 和 <span class="key_txt">setInterval</span> 会进入到相同的 Event Queue。</div>
<div class="font_min">事件循环的顺序，决定 js 代码的执行顺序。进入整体代码（<span class="key_txt">宏任务</span>）后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。</div>

```bash
setTimeout(() => {
    console.log('setTimeout')
})

new Promise(resolve => {
    console.log('promise')
}).then(() => {
    console.log('then')
})

console.log('console')
```

* <div class="font_min">这段代码作为宏任务，进入主线程。</div>
* <div class="font_min">先遇到 setTimeout，那么将其回调函数注册分发到宏任务 Event Queue。</div>
* <div class="font_min">接下来遇到 <span class="key_txt">Promise</span> ，<span class="key_txt">new Promise</span> 立即执行，<span class="key_txt">then</span> 函数分发到微任务 Event Queue。</div>
* <div class="font_min">遇到 <span class="key_txt">console.log()</span> 立即执行。</div>
* <div class="font_min">整体代码 script 作为第一个宏任务执行结束，看看有哪些微任务？我们发现 <span class="key_txt">then</span> 在微任务 Event Queue 里面，执行。</div>
* <div class="font_min">第一轮事件循环结束，开始第二轮循环，当然要从宏任务 Event Queue 开始。我们发现了宏任务 Event Queue 中 <span class="key_txt">setTimeout</span> 对应的回调函数，立即执行。</div>
* <div class="font_min">所有代码执行完毕。</div>

<div class="font_min">事件循环，宏任务，微任务关系图如下：</div>
<image style="transform: scale(0.8);" src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2017/11/21/15fdcea13361a1ec~tplv-t2oaga2asx-watermark.awebp" />