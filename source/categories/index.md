---
 title: 八股文
---

### HTML && CSS
##### >>HTML5 新特性，语义化
###### <div class="headers">1,概念</div>
<div class="font_min">HTML5的语义化指的是 <span class="key_txt">合理正确的使用语义化的标签来创建页面结构。</span> 正确的标签做正确的事</div>

###### <div class="headers">2,语义化标签</div>
<div class="font_min">header footer nav main artice section aside</div>

###### <div class="headers">3,语义化的优点</div>
* <div class="font_min">在没有css样式的情况下，页面整体也会呈现很好的结构效果</div>
* <div class="font_min">代码结构清晰，易于阅读</div>
* <div class="font_min">利于开发和维护，方便其他设备解析 根据语义渲染网页</div>
* <div class="font_min">有利于搜索引擎优化 （SEO），搜索引擎爬虫会根据不同的标签来赋予不同的权重</div>

##### >>CSS选择器及优先级
###### <div class="headers">选择器</div>
* <div class="font_min">id 选择器 ( #myid )</div>
* <div class="font_min">class 选择器 ( .myclass )</div>
* <div class="font_min">属性选择器 ( a[rel = "external"] )</div>
* <div class="font_min">伪类选择器 ( a:hover, li:nth-child )</div>
* <div class="font_min">标签选择器 ( div, h1, p )</div>
* <div class="font_min">相邻选择器 ( h1 + p )</div>
* <div class="font_min">子选择器 ( ul > li )</div>
* <div class="font_min">后代选择器 ( li a )</div>
* <div class="font_min">通配符选择器 ( * )</div>

###### <div class="headers">优先级</div>
* <div class="font_min">!important</div>
* <div class="font_min">内联样式 (1000)</div>
* <div class="font_min">id选择器 (0100)</div>
* <div class="font_min">类选择器/属性选择器/伪类选择器 (0010)</div>
* <div class="font_min">元素选择器/微元素选择器 (0001)</div>
* <div class="font_min">关系选择器/通配符选择器 (0000)</div>
<div class="font_min">带 !important 标记的样式属性优先级最高，样式表的来源相同时：<span class="key_txt">!important > 行内样式 > ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性</span></div>

##### >>position 属性的值有哪些及区别
###### <div class="headers">固定定位 fixed</div>
* <div class="font_min">元素的位置相对于浏览器窗口的固定位置，即使窗口是滚动的它也不会移动。Fixed 定位使元素的位置与文档流无关，因此是不占据空间，Fixed 定位的元素和其他元素重叠</div>

###### <div class="headers">相对定位 relative</div>
* <div class="font_min">如果对一个元素进行相对定位，它将出现在它所出现的位置上，然后，可以通过设置垂直或水平位置，让这个元素 ‘相对于’ 它的起点进行移动。在使用相对定位时，无论是否进行移动，元素任然占据原来的空间。因此，移动元素会导致它覆盖其他框</div>

###### <div class="headers">绝对定位 absolute</div>
* <div class="font_min">绝对定位元素的位置相对于最近已定位的父元素，如果元素没有已定位的父元素，那么它的位置相对于。absolute定位使元素的位置与文档流无关，因此不占据空间，absolute定位的元素和其他元素重叠</div>

###### <div class="headers">粘性定位 sticky</div>
* <div class="font_min">元素先按照普通文档流定位，然后相对于该元素在流中的 flot root （BFC）和 containing block （最近的块级祖先元素）定位，而后，元素定位表现为在跨越特定阈值前为相对定 位，之后为固定定位。</div>

##### >>box-sizing 属性
<div class="font_min">box-sizing 规定两个并排的带边框的框，语法为 <span class="key_txt">box-sizing: content-box/border-box/inherit</span></div>
<div class="font_min"><span class="headers">content-box</span>：宽度和高度分别对应用到的元素内容框，在宽度和高度之外绘制元素的内边距和边框。【标准盒子模型】</div>
<div class="font_min"><span class="headers">border-box</span>：为元素设定的宽度和高度决定了元素的边框盒。【IE 盒子模型】</div>
<div class="font_min"><span class="headers">inherit</span>：继承父元素的box-sizing值。</div>

##### >>CSS 盒子模型
<div class="font_min">CSS 盒子模型本质上是一个盒子，它包括 边距，边框，填充和实际内容。CSS中的盒子模型包括 IE 盒子模型和标准的 W3C 盒子模型。</div>
<div class="font_min">在标准的盒子模型中 <span class="key_txt">width 指 content 部分的宽度</span></div>
<div class="font_min">在 IE 盒子模型中 <span class="key_txt">width 表示 content+padding+border 这三个部分的宽度</span></div>
<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2017/10/25/9cb491d4bd5d326aeb16632280411283~tplv-t2oaga2asx-watermark.awebp">

###### <div class="font_min">故在计算盒子的宽度时存在差异</div>
<div class="font_min"><span class="headers">标准盒子模型</span>：一个块的总宽度 = width + margin（左右）+ padding（左右）+ border（左右）</div>
<div class="font_min"><span class="headers">怪异盒子模型</span>：一个块的总宽度 = width + margin（左右）（即 width 已经包含了 padding 和 border 的值了）</div>

##### >>BFC (块级格式上下文)
###### <div class="headers">BFC 的概念</div>
<div class="font_min">BFC 是 block Formatting Context 的缩写，即块级格式上下文。BFC 是css布局的一个概念，是一个独立的渲染区域，规定了内部的box如何布局，并且这个区域的子元素不会影响到外面的元素，其中比较重要的布局
规则有内部 box 垂直放置，计算 BFC 的高度的时候，浮动元素也参与计算。</div>

###### <div class="headers">BFC 的原理布局规则</div>
* <div class="font_min">内部的 Box 会在垂直方向上，一个接一个的放置</div>
* <div class="font_min">Box 垂直方向的距离由 margin 决定。属于同一个 BFC 的两个相邻 box 的margin会重叠</div>
* <div class="font_min">每个元素的 margin box 的左边，与包含块 border box 的左边相邻接触（对于从左往右的格式化，否侧相反）</div>
* <div class="font_min">BFC 的区域不会与 flot 的元素区域 重叠</div>
* <div class="font_min">BFC是一个独立容器，容器里面的子元素不会影响到外面的元素</div>
* <div class="font_min">计算 BFC 的高度时，浮动元素也参与计算</div>
* <div class="font_min">元素的类型和 display 属性，决定了这个 box 的类型，不同类型的 BOX 会参与不同的 Formatting Context。</div>

###### <div class="headers">如何创建 BFC ？</div>
* <div class="font_min">根元素，即 HTML 元素</div>
* <div class="font_min">flot 的值不为 none</div>
* <div class="font_min">position 为 absolute 或 fixed</div>
* <div class="font_min">display 的值为 inline-block，table-cell，table-caption</div>
* <div class="font_min">overflow 的值不为 visible</div>

###### <div class="headers">BFC 的使用场景</div>
* <div class="font_min">去除边距重叠现象</div>
* <div class="font_min">清除浮动（让父元素的高度包含子浮动元素）</div>
* <div class="font_min">避免某元素被浮动元素覆盖</div>
* <div class="font_min">避免多列布局由于宽度计算四舍五入而自动换行</div>

##### >>让一个元素水平垂直居中
###### <div class="headers">水平居中</div>
* <div class="font_min">对于行内元素：text-align: center</div>
* <div class="font_min">对于确定宽度的块级元素</div>
* <div class="font_min">width 和 margin 实现，margin: 0 auto</div>
* <div class="font_min">绝对定位和 margin-left：（父 width - 子 width）/ 2，前提是父元素是相对定位 position: relative</div>

<div class="font_min">对于宽度未知的块级元素</div>

* <div class="font_min">table 标签配合 margin 左右 auto 实现水平居中。使用 table 标签（或直接将块级元素设置成 display: table），在通过给该标签添加左右 margin 为 auto</div>
* <div class="font_min">inline-block 实现水平居中方法，display: inline-block 和 text-align: center 实现水平居中</div>
* <div class="font_min">绝对定位 + transform，translateX 可以移动本身元素的50%</div>
* <div class="font_min">flex 布局使用 justify-content: center</div>

##### >>隐藏页面中某个元素的方法
* <div class="font_min">opacity: 0，将目标元素隐藏起来，但不会改变页面布局，并且，如果该元素已经绑定一些事件，那么点击该区域，能正常触发事件</div>
* <div class="font_min">visibility: hidden，将目标元素隐藏起来，但不会改变页面布局，但不会触发该元素已绑定的事件，隐藏对应元素，在文档流中任然保留空间（重绘）。</div>
* <div class="font_min">display: none，将目标元素隐藏起来，不占据文档流空间（回流 + 重绘），会改变页面布局，不显示对应的元素</div>

##### >>重排和重绘
<div class="markdown-body">
关键渲染路径是浏览器将 HTML，CSS 和 JavaScript 转换为屏幕上的像素所经历的步骤序列，优化关键路径渲染可提高渲染性
<div class="font_min">大致步骤：浏览器在解析 HTML 时会创建 DOM -> 解析 CSS 文件 -> 生成 CSSOM，CSSOM 包含了页面所有的样式 -> DOM 和 CSSOM 树结合为渲染树 -> 渲染树构建完成后，开始布局，取决于屏幕的尺寸 -> 布局完成，
像素就可以被绘制在屏幕上，之后，只有受影响的屏幕区域会被重绘</div>
</div>

* <div class="font_min">重排：元素的位置发送变动发生重排，也叫回流。此时在布局阶段，计算每一个元素在设备视口内的确切位置和大小。当一个元素位置发生变化时，其父元素及其后边的元素位置都有可能发生变化，代价高。</div>
* <div class="font_min">重绘：元素的样式发生变动，但是位置没有变化。此时在关键渲染路径的像素绘制阶段。</div>
* <div class="font_min key_txt">重排一定会重绘，重绘不一定会重排。</div>

##### >>页面布局
###### <div class="headers">flex 布局</div>
<div class="font_min">布局的传统解决方案，基于盒子模型，依赖 display 属性 + position 属性 + float 属性。flex 是 Flexible Box 的缩写，意为’弹性布局‘，用来为盒状模型提供最大的灵活性。指定容器为 display: flex 即可，
简单的分为容器属性和元素属性</div>
<div class="font_min">容器的属性</div>

* <div class="font_min">flex-direction：决定主轴的放心（即子 item 的排列方式），flex-direction: row | row-reverse| column | column-reverse</div>
* <div class="font_min">flex-wrap：决定换行规则。flex-wrap: nowrap | wrap | wrap-reverse</div>
* <div class="font_min">justify-content：对齐方式，水平主轴对齐方式</div>
* <div class="font_min">align-items：对齐方式，竖直轴线方向</div>
<div class="headers">Rem 布局</div>
<div class="headers">百分比 布局</div>
<div class="headers">浮动 布局</div>

##### >>清除浮动的方式
* <div class="font_min">添加而外的标签</div>
```js
<div class="parent">
    //添加额外标签并且添加clear属性
    <div style="clear:both"></div>
    //也可以加一个br标签
</div>

```
* <div class="font_min">给父级添加 overflow: hidden，或者设置高度，或者建立伪类选择器清除浮动</div>
```js
//在 css 中添加 :after 伪元素
.parent:after{
    /* 设置添加子元素的内容为空 */
    content: '';
    /* 设置添加子元素为块级元素 */
    display: block;
    /* 设置添加的子元素高度为0 */
    height: 0;
    /* 设置添加子元素看不见 */
    vidibility: hidden;
    /* 设置 clear: both */
    clear: both;
}
```

##### >>Url输入到页面展现发生了什么
* <div class="font_min">DNS 解析：将域名解析成 IP 地址</div>
* <div class="font_min">TCP 连接：TCP 三次握手</div>
* <div class="font_min">发送 HTTP 请求</div>
* <div class="font_min">服务器处理请求并返回 HTTP 报文</div>
* <div class="font_min">浏览器解析渲染页面</div>
* <div class="font_min">断开连接：TCP四次挥手</div>

##### >>三次握手
<div class="font_min">1. 第一次握手：客户端会给服务器发送一个 <span class="key_txt">SYN</span> 报文</div>
<div class="font_min">2. 第二次握手：服务器收到 <span class="key_txt">SYN</span> 报文后，会应答一个 <span class="key_txt">SYN + ACK</span> 报文</div>
<div class="font_min">3. 第三次握手：客户端收到 <span class="key_txt">SYN + ACK</span> 报文之后，会回应一个 <span class="key_txt">ACK</span> 报文</div>
<div class="font_min">4. 服务器收到 <span class="key_txt">ACK</span> 报文后，三次握手 <span class="key_text">建立完成</span></div>
<div class="font_min">作用是为了确认双方的接收与发送能力是否正常</div>

<div class="font_min markdown-body mar_top">为啥只要三次握手才能确认双方的接收与发送能力是否正常，而两次却不可以？  

* <div class="font_min">第一次握手：客户端发送网络包，服务端收到了。由此服务端能得出结论：客户端的发送能力，服务端的接收能力是正常的</div>
* <div class="font_min">第二次握手：服务端发包，客户端收到了。这样客户端就能得出结论：服务端的接收，发送能力，客户端的接收，发送能力是正常的。不过此时服务器并不能确认客户端的接收能力是否正常</div>
* <div class="font_min">第三次握手：客户端发包，服务端收到了。这样服务端就能得出结论：客户端的接收，发送能力是正常，服务器自己的发送，接收能力正常</div>

<div class="font_min">因此，需要三次握手才能确认双方的接收与发送能力是否正常</div>

</div>


### JS, TS, ES6
##### >>JS 中的8中数据类型及区别
<div class="font_min">包括值类型（基本对象类型）和引用类型（复杂对象类型）</div>
<div class="font_min"><span class="headers">基本类型（值类型）</span>：Number（数字类型），String（字符串类型），Boolean（布尔类型），Symbol（符号类型），null（空），undefined（未定义）</div>
<div class="font_min"><span class="headers">引用类型（复杂对象类型）</span>：Object（对象），Function（方法）。其他还有 Array（数组），Date（日期），RegExp（正则表达式）</div>

##### >>JS 中的数据类型检测方案
1. <div class="headers">typeof</div>
```js
console.log(typeof 1);  //number
console.log(typeof true);   //boolean
console.log(typeof 'mc');   //string
console.log(typeof Symbol); //function
console.log(typeof function(){});   //function
console.log(typeof console.log());  //function
console.log(typeof []); //object
console.log(typeof {}); //object
console.log(typeof null);   //object
console.log(typeof undefined);  //undefined                           
```
<div class="font_min">优点：能够快速区分基本数据类型</div>
<div class="font_min">缺点：不能将 object，array，null 区分，都返回 object</div>

2. <div class="headers">instanceof</div>
<div class="font_min"><span class="key_txt">instanceof</span> 可以正确的判断对象的类型，因为内部机制是通过判断对象的原型链中是不是能找到类型的 <span class="key_txt">prototype</span></div>

```js
console.log(1 instanceof Number);   //false
console.log(true instanceof Boolean);   //false
console.log('str' instanceof String);   //false
console.log([] instanceof Array);   //true
console.log(function(){} instanceof function); //true
console.log({} instanceof Object);  //true
```
<div class="font_min">优点：能够区分 Array，Object 和 Function，合适用来判断自定义的类实列对象，<span class="key_txt">主要用于检测某个构造函数的原型对象在不在某个对象的原型链上</span></div>
<div class="font_min">缺点：Number，Boolean 和 String 基本数据类型不能判断</div>
<div class="font_min">可以试着实现一下 instanceof</div>

```js
//首先获取类型的原型
//然后获取对象的原型
//然后一直循环判断对象的原型是否等于类型的原型，直到对象原型为 null，因为原型链最终为 null
function myInstanceof(left, right) {
    let prototype = right.prototype
    left = left.__proto__
    while (true) {      
        if(left === null || left === undefined) return false
        if(prototype === left) return true
        left = left.__proto__
    }
}
```


3. <div class="headers">Object.prototype.toString.call()</div>
```js
var toString = Object.prototype.toString
console.log(toString.call(1));  //[object number]
console.log(toString.call(true));   //[object Boolean]
console.log(toString.call('mc'));   //[object String]
console.log(toString.call([])); //[object Array]
console.log(toString.call({})); //[object Object]
console.log(toString.call(function(){}));   //[object function]
console.log(toString.call(undefined));  //[object undefined]
console.log(toString.call(null));   //[object Null]
```
<div class="font_min">优点：精准判断数据类型</div>
<div class="font_min">缺点：写法繁琐不容易记，推荐封装后进行使用</div>

##### >>var && let && const
<div class="font_min">ES6 之前创建变量用 var，之后创建变量用 let/const</div>
<div class="headers">三者区别</div>
<div class="font_min">1. var 定义的变量，没有块的概念，可以跨块访问，不能跨函数访问。let 定义的变量，只能在块作用域里访问，不能跨块访问，也不能跨函数访问。const 用来定义常量，
使用时必须初始化（即必须赋值），只能在块作用域里访问，且不能修改</div>
<div class="font_min">2. var 可以先使用，后声明，因为存在变量提升，let 必须先声明后使用</div>
<div class="font_min">3. var 是允许在相同作用域内重复声明同一个变量，而 let 和 const 是不允许这现象的</div>
<div class="font_min">4. 在全局上下文中，基于 let 声明的全局变量和全局对象 GO（window）没有任何关系，var 声明的变量会和 GO 有映射关系</div>
<div class="font_min">5. 会产生暂时性死区</div>
<div class="markdown-body">
暂时性死区是浏览器的 bug，检测到一个未被声明的变量类型时，不会报错，会返回 undefined</br>
如 console.log(typeof a)    //undefined</br>
而 console.log(typeof a)    //未声明之前不能使用</br>
let a
</div>
<div class="font_min">let /const/ function 会把当前所在的大括号（除函数外）作为一个全新的块级上下文，应用这个机制，在开发项目的时候，遇到循环事件绑定等类似的需求，无需再自己构建闭包来存储，
只要基于 let 的块级作用域特征即可解决</div>

##### >>JS 垃圾回收机制
<div class="font_min">1. 项目中，如果存在大量不被释放的内存（堆/栈/上下文），页面性能会变得很慢，当某些代码操作不能被合理释放，就会造成内存泄漏。我们尽可能的少使用闭包，因为它会消耗内存。</div>
<div class="font_min">2. 浏览器垃圾回收机制/内存回收机制:</div>
<div class="font_min markdown-body">浏览器的 javascript 具有自动垃圾回收机制（GC:Garbage Collecation），垃圾收集器会定期（周期性）找出那些不在使用的变量，然后释放其内存。</div>
<div class="font_min"><span class="headers">标记清除:</span>在 js 中，最常用的垃圾回收机制就是标记清除，当变量进入执行环境时，被标记为'进入环境'，当变量离开执行环境时，被标记为'离开环境'。
垃圾回收机制会摧毁那些带标记的值并回收他们所占用的内存空间。</div>
<div class="font_min">3. 优化手段：内存优化；手动释放：取消内存的占用即可</div>

* <div class="font_min">堆内存： fn = null 【null：空指针对象】</div>
* <div class="font_min">栈内存：把上下文中，被外部的堆的占用取消即可</div>

<div class="font_min">4. 内存泄漏</div>
<div class="font_min">在 js 中，常见的内存泄漏主要有四种：全局变量，闭包，DOM 元素的引用，定时器</div>

##### >>作用域 和 作用域链
<div class="font_min">定义：简单来说作用域就是变量和函数的可访问范围，<span class="key_txt">由当前环境与上层环境的一系列对象变量组成</span></div>

* <div class="font_min">全局作用域：代码在程序的任何地方都能被访问，window 对象的内置属性都拥有全局作用域</div>
* <div class="font_min">函数作用域：在固定的代码片段才能被访问</div>

<div class="font_min">作用：作用域最大的作用就是<span class="key_txt">隔离变量</span>，不同作用域下同名变量不会有冲突。</div>
<div class="font_min"><span class="headers">作用域链：</span>一般情况下，变量到 创建变量的函数作用域中取值，但如果在当前的作用域没有找到，就会像上级作用域去查，直到查到全局作用域，这一个查找过程
形成的链条就叫做作用域链。</div>

##### >>闭包
<div class="font_min"><span class="key_txt">闭包是指有权访问另一个函数作用域内变量的函数</span>，创建闭包最常见的方式就是在一个函数内创建另一个函数，创建的函数可以访问到当前函数的局部变量</div>
<div class="font_min">闭包有两个常用的用途</div>

* <div class="font_min">1. 使我们能在函数外部能够访问到函数内部的变量。通过使用闭包，我们可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量，可以使用这种方法来创建私有变量</div>
* <div class="font_min">2. 使已经运行结束的函数上下文中的变量对象继续留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会给回收</div>

<div class="font_min key_txt">闭包本质上就是上级作用域内变量的生命周期，因为被下级作用域内引用，而没有被释放。就导致上级作用域的变量，等到下级作用域执行完以后才正常得到释放。</div>

```js
function a() {
    var n = 0
    function add() {
        n++
        console.log(n)
    }
    return add
}
var a1 = a(); //注意：函数只是一个标识（指向函数的指针），而 () 才是执行函数
a1() // 1
a2() // 2 第二次调用n变量还在内存中
```

##### >>JS 中 this 的五种情况
<div class="font_min">1. 作为普通函数执行时<span class="key_txt">this 指向 window</span></div>
<div class="font_min">2. 当函数作为对象的方法被调用时，<span class="key_txt">this 就会指向该对象</span></div>
<div class="font_min">3. 构造器调用，<span class="key_txt">this 指向返回的这个对象</span></div>
<div class="font_min">4. 箭头函数，箭头函数的<span class="key_txt">this 绑定看的是 this 所在函数定义在哪个对象下</span>，就绑定哪个对象，如果有嵌套情况，则 this 绑定到最近的一层对象上。</div>
<div class="font_min">5. 基于 Function.prototype 上的<span class="key_txt">apply，call 和 bind 调用模式</span></div>

###### <div class="headers">apply, call, bind 的作用和区别</div>
* <div class="font_min">他们都是用来改变 this 指向的。<span class="key_txt">this 永远指向最后调用它的那个对象</span></div>
<div class="key_txt font_min">call: 接受的是一个参数列表</div>
<div class="key_txt font_min">apply: 接受的是一个数组</div>
<div class="key_txt font_min">bind: 方法返回的任然是一个函数，因此后面还需要 () 来进行调用才可以</div>
     * <span class="font_min">apply() 方法调用一个函数，其具有一个指定的 this 值，以及作为一个数组（或类似数组的对象）提供参数</span>
     ```js
     fun.apply(thisArg, [argsArray])
     ```
     * <span class="font_min">call 和 apply 基本类似，他们的区别只是传入的参数不同</span>
     ```js
     fun.call(thisArg,1,2,3)
     ```
     * <span class="font_min">bind()方法创建一个新的函数，当被调用时，将其this关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定的参数序列</span>

##### >>原型 && 原型链
<div class="headers">原型关系:</div>

* <div class="font_min">每个 class 都有显示原型的 prototype</div>
* <div class="font_min">每个实例都有隐式原型 _proto_</div>
* <div class="font_min">实例的 _proto_ 都指向对应的 class 的 prototype</div>

<div class="headers">原型:</div>
<span class="font_min">在 js 中，每当定义一个对象（函数也是对象）时，对象中都会包含一些预定义的属性。其中每个 函数对象 都有一个 <span class="key_txt">prototype</span>属性，这个属性指向函数的原型对象</span>
<div class="headers">原型链:</div>
<span class="font_min">使用 new 关键字创建一个实例对象时，实例对象会产生一个 <span class="key_txt">__proto__</span> 指针，该指针指向构造函数的原型对象，原型对象也有一个 <span class="key_txt">__proto__</span> 指针，
这个原型对象的指针指向父类型的原型对象，两个指针之前形成一种链条关系，也可以把这种关系叫做原型链</span>
<div class="key_txt font_min">任何一个 prototype 对象都有一个 constructor (构造函数) 属性，指向与它的构造函数。</div>

##### >>new 运算符的实现机制
<div class="font_min">1. 首先创建了一个新的空对象</div>
<div class="font_min">2. 继承构造函数的原型</div>
<div class="font_min">3. 绑定 this</div>
<div class="font_min">4. 如果该函数没有返回对象，则返回 this （返回新对象）</div>

##### >>setTimeout、Promise、Async/Await 的区别
* <div class="font_min">setTimeout</div>
<div class="font_min">setTimeout 的回调函数放到宏任务队列里，等到执行栈清空后执行。</div>

* <div class="font_min">Promise</div>
<div class="font_min">promise 本身是 <span class="key_txt">同步的立即执行函数</span>，当在 executor(执行者) 中执行 resolve 或者 reject 的时候，此时是异步操作，会执行 then/catch等，当主栈完成后，
才会去调用 resolve/reject 中存放的方法执行。</div>
```js
console.log('script start')
let promise1 = new Promise(function (resolve) {
    console.log('promise')
    resolve()
    console.log('promise1 end')
}).then(function () {
    console.log('promise2')
})
setTimeout(function(){
    console.log('settimeout')
})
console.log('script end')
//输出顺序 script start -> promise1 -> promise1 end w1 -> script end -> promise2 -> settimeout
```

* <div class="font_min">Async/await</div>

<div class="font_min">async 函数返回一个 promise 对象，当函数执行时，一旦遇到 await 就会先返回，等到触发的异步操作完成后，再执行函数体内后面的语句。可以理解为，是让出了线程，跳出了 async 函数体。</div>

```js
async function async1(){
   console.log('async1 start');
    await async2();
    console.log('async1 end')
}
async function async2(){
    console.log('async2')
}

console.log('script start');
async1();
console.log('script end')
//输出顺序 script start -> async1 start -> async2 -> script end-> async1 end
```

##### >>Async/Await 如何通过同步的方式实现异步
<div class="font_min">Async/Await 就是一个自执行的 generate 函数，利用 generate 函数的特性把异步的代码写成 '同步' 的形式，第一个请求的返回值作为后面请求的参数，其中每一个参数都是一个 promise 对象。</div>

##### >>Cookie, sessionStorage, localStorage 的区别
<div class="font_min markdown-body">
<div class="font_min">cookie 数据大小不能超过4k，sessionStorage和localStorage的存储比 cookic 大得多，可以达到 5M+</div>
<div class="font_min">cookie 在设置的过期时间之前一直有效，localStorage 永久存储，浏览器关闭后数据不会丢失，除非主动删除数据。sessionStorage 数据在当前浏览器关闭后自动删除</div>
</div>

##### >>介绍节流防抖原理，区别以及应用
<div class="font_min"><span class="key_txt">节流:</span> 事件触发后，规定时间内，事件处理函数不能再次被调用。也就是说在规定时间内，函数只能被调用一次，且是最先被触发调用的那一次</div>
<div class="font_min"><span class="key_txt">防抖:</span> 多次触发事件，事件处理函数只能执行一次，并且是在触发操作结束后执行。也就是说，当一个事件被触发准备执行函数前，会等待一定时间，（时间自定义）
如果时间内没有再次被触发，那么就执行，如果被触发了，那就本次作废，重新从新触发的时间计算，并再次等待定义的时间，直到能最终执行。</div>

* <div class="font_min key_txt">防抖就类似回城，打断就得重新回</div>
* <div class="font_min key_txt">节流就类似技能冷却需要冷却时间到了才能用</div>

 <div class="font_min key_txt">使用场景:</div>
  
* <div class="font_min">节流：滚动加载更多，搜索框的搜索，高频点击，表单重复提交</div>
* <div class="font_min">防抖：搜索框搜索输入，并在输入完成以后自动搜索，手机号，邮箱验证输入检测，窗口大小 resize 变化后，再重新渲染。</div>
```js
/*
     * 节流函数：一个函数执行一次后，只有大于设定的执行周期才会执行第二次，有个需要频繁触发的函数，由于优化性能的角度，在规定时间内，只让函数触发的第一次生效，后面的都不生效。
     * @param fn 要被节流的函数
     * @param delay 规定的时间
     */
    function throttle(fn, delay) {
        //记录上一次触发的时间
        var lastTime = 0
        return function () {
            //记录当前函数触发的时间
            var nowTime = Date.now()
            if (nowTime - lastTime > delay) {
                //修正 this 指向问题
                fn.call(this)
                //同步执行结束时间
                lastTime = nowTime
            }
        }
    }

    document.onscroll = throttle(function () {
        console.log('scroll 事件被触发了' + Date.now())
    }, 200)

    /*
    * 防抖函数：一个需要频繁触发的函数，在规定时间内，只让最后一次生效，前面的不生效
    * @param fn 要被防抖的函数
    * @param delay 规定的时间
    */
   function debounce(fn, delay) {
       //记录上一次的延时器
       var timer = null
       return function() {
           //清除上一次的延时器
           clearTimeout(timer)
           //重新设置新的延时器
           timer = setTimeout(() => {
               //修正 this 指向问题
               fn.apply(this,arguments)
           }, delay);
       }
   }

   document.getElementById('btn').onclick = debounce(function() {
       console.log('按钮被点击了' + Date.now())
   },1000)
```

### 简述 MVVM
<div class="font_min"><span class="headers">什么是 MVVM</span></div>
<div class="font_min"><span class="key_txt">视图模型双向绑定</span>，是 <span class="key_txt">model-view-viewModel</span>的缩写，也就是把<span class="key_txt">MVC</span>中的 controller 演变成 ViewModel。
model 层代表数据模型，view 代表视图模型，UI 组件，viewModer 是 view 和 model 层的桥梁，数据会绑定到 viewmodel 层并自动将数据渲染到页面中，视图变化时的时候会通知 viewModel 层更新数据，以前是操作 DOM 结构更新视图，
现在是 <span class="key_txt">数据驱动视图。</span></div>

<div class="font_min"><span class="headers">MVVM 的优点</span></div>
<div class="font_min">1. <span class="key_txt">低耦合。</span>视图（view）可以独立与 model 变化和修改，一个 model 可以绑定到不同的 view 上，当 view 变化的时候 model 可以不变化，当 model 变化的时候 view 也可以不变化</div>
<div class="font_min">2. <span class="key_txt">可重用性。</span>你可以把一下视图逻辑放在一个 model 里面，让很多 view 重用这段视图逻辑。</div>
<div class="font_min">3. <span class="key_txt">独立开发。</span>开发人员可以专注于业务逻辑和数据的开发（viewModel），设计人员可以专注于页面设计</div>
<div class="font_min">4. <span class="key_txt">可测试。</span></div>

##### >>Vue 2 双向绑定原理
<div class="font_min">vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过 Object.defineProperty() 来劫持各个属性的 setter 和 getter，在数据变动时发布消息给订阅者，触发相应的监听回调</div>
<div class="font_min">vue 是一个典型的 MVVM 框架，模型（Modle）只是普通的 Javascript 对象，修改它则视图（view）会自动更新。这种设计让状态管理变得非常简单而直观。</div>
<div class="font_min "><span class="headers">Observer （数据监听器）</span>：Observer 的核心是通过 Object.defineProperty() 来监听数据的变动，这个函数内部可以定义 setter 和 getter，每当数据发生变化，就会触发 setter。这时候 Observer 就要通知订阅者，订阅者就是 Watcher</div>
<div class="font_min "><span class="headers">Compile （指令解析器）</span>：Compile 主要做的是解析模板指令，将模板中变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加鉴定数据的订阅者，
一旦数据有变动，收到通知，调用更新函数进行数据更新。</div>
<div class="font_min "><span class="headers">Watcher（订阅者）</span>：Watcher 订阅者作为 Observer 和 Compile 之间通信的桥梁，主要任务就是订阅 Observer 中的属性值变化的消息，当收到属性值变化的消息时，触发解析器 Compile  中对应的更新函数。</div>
<div class="font_min "><span class="headers">Dep（订阅器）</span>：订阅器采用 发布 - 订阅 设计模式，用来收集订阅者 Watcher，对监听器 Observer 和 订阅者 Watcher 进行统一管理。</div>
<image src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/12/7/16ede3f4bc83638a~tplv-t2oaga2asx-watermark.awebp" />

##### >>谈谈对 Vue 生命周期的理解
<div class="font_min">每个 Vue 实例在创建时都会经过一系列的初始化过程，Vue 的生命周期钩子，就是说在达到某一阶段或条件时触发的函数，目的是为了完成一些动作或者事件。</div>

* <div class="font_min"><span class="key_txt">beforeCreate</span>：创建前，此时 data 和 methods 中的数据还未初始化</div>
* <div class="font_min"><span class="key_txt">created</span>：创建完毕，data 中有值，未挂载</div>
* <div class="font_min"><span class="key_txt">beforeMount</span>：Vue 实例被挂载到真实 DOM 节点，可以发起服务端请求，拿数据</div>
* <div class="font_min"><span class="key_txt">mounted</span>：此时可以操作 DOM</div>
* <div class="font_min"><span class="key_txt">beforeUpdate</span>：数据更新前</div>
* <div class="font_min"><span class="key_txt">updated</span>：数据更新后</div>
* <div class="font_min"><span class="key_txt">beforeDestroy</span>：实例被销毁前，此时可以手动销毁一些方法</div>
* <div class="font_min"><span class="key_txt">destroyed</span>：实例销毁后调用</div>

<div class="font_min headers">组件的生命周期</div>
<div class="font_min">生命周期（父子组件）：父组件 beforeCreate --> 父组件 created --> 父组件 beforeMount --> 子组件 beforeCreate --> 子组件 created --> 子组件 beforeMount --> 子组件 mounted -->
父组件 mounted --> 父组件 beforeUpdate --> 子组件 beforeDestroy --> 子组件 destroyed --> 父组件 updated</div>

##### >>computed(计算属性) 和 watch(监听属性)
<div class="font_min">通俗来讲，既能用 computed 实现又可以用 watch 监听来实现功能，推荐用 computed，重点在于 computed 的缓存功能，computed 计算属性是用来声明式的描述一个值依赖了其他的值，当所依赖的值或者变量改变时，
计算属性也会跟着改变，watch 监听的是已经在 data 中定义的变量，当该变量变化时，会触发 watch 中的方法。</div>

* <div class="font_min"><span class="headers">watch 监听属性</span>是一个对象，键是需要观察的属性，值是对应回调函数，主要用来监听某些特地数据的变化，从而进行某些具体的业务逻辑操作，监听属性的变化，需要在数据变化时
执行异步或开销较大的操作时使用</div>
* <div class="font_min"><span class="headers">computed 计算属性</span>的结果会被缓存，当 computed 中的函数所依赖的属性没有发生改变的时候，那么调用当前函数的时候结果会从缓存中读取。除非依赖的响应式属性变化时才会重新计算。
主要当做属性来使用 computed 中的函数必须用 <span class="key_txt">return</span> 返回最终的结果，computed 更高效，优先使用，data 不变，computed 不更新。</div>
* <div class="font_min"><span class="headers">使用场景</span>computed：当一个属性受多个属性影响的时候使用，例：购物车商品结算功能。watch：当一条数据影响多条数据的时候使用，例：搜索数据。</div>

##### >>组件中的 data 为什么是一个函数
<div class="font_min">1. 一个组件被多次复用的话，也就会创建多个实例。本质上，这些实例用的都是同一个构造函数。</div>
<div class="font_min">2. 如果 data 是对象的话，对象属于引用类型，会影响到所有实例。所以为了保证组件不同的实例之间 data 不冲突，data 必须是一个函数。</div>

##### >>为什么 v-for 和 v-if 不建议用在一起
<div class="font_min">1. 当 v-for 和 v-if 处于同一个节点时，v-for 的优先级会比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中，如果要遍历的数组很大，而真正要展示的数据很少时，这会造成很大的性能浪费</div>
<div class="font_min">2. 这种场景建议使用 computed，先对数据进行过滤。</div>
<div class="font_min"><span class="key_txt">注意</span>：3.x版本中 v-if 总是优先于 v-for 生效。由于语法存在歧义，建议避免在同一个元素上使用两者。</div>

##### >>Vue 组件的通信方式
* <div class="font_min">props / $emit 父子组件通信</div>
<div class="font_min">父 -> 子 <span class="key_txt">props</span>，子 -> 父 <span class="key_txt">$on、$eimt</span> 获取父子组件实例 <span class="key_txt">parent、children、Ref</span> 获取实例的方法调用组件的属性或者方法</div>

* <div class="font_min">$emit / $on 自定义事件 兄弟组件通信</div>
* <div class="font_min">provide/inject (祖先及其后代传值)</div>
* <div class="font_min">vuex 跨组件通信</div>

##### >>nextTick 的实现
<div class="font_min">1. nextTick 是 Vue 提供的一个全局 API，是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM。</div>
<div class="font_min">2. Vue 在更新 DOM 是异步执行的，只要侦听到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个 watcher 被多次触发，只会被推入到队列中一次。</div>
<div class="font_min">3. 比如，我在干什么的时候就会使用 $nextTick，传一个回调函数进去，在里面执行 DOM 操作即可。</div>
<div class="font_min">4. 简单来说，Vue 在修改数据后，视图不会立马更新，而是等 <span class="key_txt">统一事件循环</span> 中的所有数据变化完成后，再统一进行视图更新。</div>

##### >>nextTick 实现的原理是什么
<div class="font_min">在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后，立即使用 nextTick 来获取更新后的 DOM。nextTick 主要使用了宏任务和微任务。根据执行环境分别尝试采用 promise, MutationObserver, setlmmediate,
如果以上都不行则采用 setTimeout 定义了一个异步方法，多次调用 nextTick 会将方法存入队列中，通过这个异步方法清空当前队列。</div>

##### >>插槽，用的是 具名插槽 还是 匿名插槽 或者 作用域插槽 ( slot )
<div class="font_min">vue 的插槽是一个非常好用的东西，slot 说白了就是一个占位，在 vue 中插槽包含三种，1：<span class="key_txt">默认插槽（匿名插槽）</span>，2：<span class="key_txt">具名插槽</span>，3：<span class="key_txt">作用域插槽</span></div>

##### >>keep-alive 的实现
<div class="font_min"><span class="headers">作用</span>：实现组件的缓存，保持这些组件的状态。以避免反复渲染导致性能问题。需要缓存组件（切换频繁），不需要重复渲染</div>
<div class="font_min"><span class="headers">场景</span>：tabs 标签页，后台导航，Vue 性能优化。</div>
<div class="font_min"><span class="headers">原理</span>：Vue.js 内部将 <span class="key_txt">DOM</span>节点抽象成一个个  <span class="key_txt">VNode</span> 节点，<span class="key_txt">keep-alive</span> 组件的缓存也是基于 <span class="key_txt">VNode</span> 节点的，而不是直接存储 DOM 结构。</div>
<div class="font_min headers">钩子函数:</div>

* <div class="font_min"><span class="key_txt">activated</span>：组件渲染后调用</div>
* <div class="font_min"><span class="key_txt">deactivated</span>：组件销毁后调用</div>

##### >>mixin 混入 ( mixins:[mixin] )
<div class="font_min">mixin 项目变得复杂的时候，多个组件之间有重复的逻辑就会用到 mixin，多个组件有相同的逻辑，抽离出来。但 mixin 不是完美的解决方案，会有一些问题。</div>
<div class="font_min key_txt">劣势：</div>
<div class="font_min">1. 变量来源不明确，不利于阅读。</div>
<div class="font_min">2. 多 mixin 可能会造成命名冲突。</div>
<div class="font_min">3. mixin 和组件可能会出现多对多的关系，使得项目的复杂度变高。</div>

##### >>Vuex 的理解及使用场景
<div class="font_min">Vuex 是一个专门为 Vue 应用程序开发的状态管理模式。每一个 Vuex 应用的核心就是 Store（仓库）。</div>
<div class="font_min">1. Vuex 的状态存储是响应式的，当 Vue 组件从 store 中读取状态的时候，若 store 的状态发生变化，那么相应的组件也会得到高效的更新响应。</div>

<div class="font_min">2. 改变 store 中的状态唯一的途径就是提交 commit、mutation，这样使得我们可以方便的跟踪每一个状态的变化 Vuex 主要包括几个以下核心模块。</div>

* <div class="font_min"><span class="key_txt">State</span>：定义了应用的状态数据。</div>
* <div class="font_min"><span class="key_txt">Getter</span>：在 store 中定义 'getter'（可以认为是 store 的计算属性），就像是计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有它的依赖值发生了变化才会重新计算。</div>
* <div class="font_min"><span class="key_txt">Mutation</span>：是唯一更改 store 中状态的方法，且必须是同步函数。</div>
* <div class="font_min"><span class="key_txt">Action</span>：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作。可以获取异步数据后调用 mutation 提交最终数据。</div>
* <div class="font_min"><span class="key_txt">Module</span>：允许将单一的 store 拆分为多个 strore 且同时保存在单一的状态树中。</div>
<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a7249773a1634f779c48f3f0ffabf968~tplv-k3u1fbpfcp-watermark.awebp" width=700 height=550 />

##### >>项目优化
<div class="font_min headers">移除生产环境的控制台打印</div>
<div class="font_min">Vue-cli3版本以上，安装依赖。<span class="key_txt">npm install babel-plugin-transform-remove-console -S</span></div>
<div class="font_min">在 <span class="key_txt">babel.config.js</span> 配置</div>

```js
let transformRemoveConsolePlugin = []
//生产环境
if (process.env.NODE_ENV === 'production') {
  transformRemoveConsolePlugin = ['transform-remove-console']
}

module.exports = {
  "plugins": [ ...transformRemoveConsolePlugin ],
  presets: [ '@vue/cli-plugin-babel/preset' ],
  'env': {
    'development': { 
        'plugins': ['dynamic-import-node'] 
        }
  }
}
```
<div class="font_min"><span class="key_txt">第三方库的按需加载。</span>echarts，官方文档里是使用配置文件指定使用的模块，另一种使用 <span class="key_txt">babel-plugin-equire</span> 实现按需加载。element-ui 使用 <span class="key_txt">babel-plugin-component</span> 实现按需加载。</div>
<div class="font_min">减少请求次数。</div>
<div class="font_min">使用 CDN 加速。</div>

##### >>主流库
<div class="font_min"><span class="headers">async-validator</span>：校验库，基本主流的组件库都是基于它做的校验。</div>
