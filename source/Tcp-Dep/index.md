---
title: Tcp-Dep
---

##### UDP
<div class="warning">UDP 于 TCP 的区别是什么？</div>
<div class="font_min key_txt">可靠传输协议 TCP + 不可靠传输协议 UDP</div>
<div class="font_min">首先 UDP 协议是面向无连接的，也就是说不需要在正式传递数据之前先连接双方。然后 UDP 协议只是数据报文的搬运工，不保证有序且不丢失的传递到对端，并且 UDP 协议也没有任何控制流量的算法，总的来说 DUP 协议相较于TCP 更加的轻便。</div>

###### 面向无连接
<div class="font_min">首先 UDP 是不需要和 TCP 一样在数据发送前进行三次握手建立连接，想发数据就可以开始发送了。</div>
<div class="font_min mar_top">并且也只是数据报文的搬运工，不会对数据报文进行任何拆分和拼接操作。</div>
<div class="font_min mar_top">具体来说就是：</div>

* <div class="font_min">在发送端，应用层将数据传递给传输层的 UDP 协议，UDP 只会给数据增加一个 UDP 头标识下是 UDP 协议，然后就传递给网络层了</div>
* <div class="font_min">在接收端，网络层将数据传递给传输层，UDP 只去除 IP 报文头就传递给应用层，不会任何拼接操作</div>

###### 不可靠性
<div class="font_min">首先不可靠性是体现在无连接上，通信都不需要建立连接，想发就发，这样的情况肯定不可靠。</div>
<div class="font_min mar_top">并且收到什么数据就传递什么数据，也不会备份数据，发送数据也不会关心对方是否已经正确接受到数据了。</div>
<div class="font_min mar_top">再者网络环境时好时坏，但是 UDP 因为没有拥塞控制，一直会以恒定的速度发送数据。即时网络条件不好，也不会对发送速率进行调整。这样实现的弊端就是在网络条件不好的情况下可能会导致丢包。但是优点也明显，在某些实时性要求非常高的场景（例如：电话会议）就需要使用 UDP 而不是 TCP。</div>

###### 高效
<div class="font_min">虽然 UDP 协议不是那么的可靠，但正是因为它不那么的可靠，所以也就没有 TCP 那么的复杂，需要保证数据不丢失且有序到达。</div>

###### 传输方式
<div class="font_min">UDP 不仅支持一对一的传输方式，同意支持一对多，多对一的方式，也就是说 UDP 提供了单播，多播和广播功能。</div>

###### 适合使用的场景
<div class="font_min">在很多实时性要求很高的地方都可以看到 UDP 的身影。</div>
<div class="font_min mar_top">直播，王者荣耀疑似也用了 UDP</div>

###### 总结
* <div class="font_min">UDP 相比 TCP 简单的多，不需要建立连接，不需要验证数据报文，不需要流量控制，只会把想发   的数据报文一股脑的丢给对端</div>
* <div class="font_min">虽然 UDP 没有 TCP 传输来的准确，但是也在很多实时性要求高的地方有所作为</div>

##### TCP
<div class="warning">UDP 和 TCP 的区别是什么？</div>
<div class="font_min mar_top">TCP 建立连接和断开连接都先需要进行握手。在传输数据过程中，通过各种算法保证数据的可靠性，当然带来的问题就是相比 UDP 来说不是那么的高效。</div>

###### 头部
<div class="font_min">对于 TCP 头部来说，一下几个字段是很重要的</div>

* <div class="font_min"><span class="key_txt">Sequence number</span>，这个序号保证了 TCP 传输的报文都是有序的，对端可以通过序号顺序的拼接报文</div>
* <div class="font_min">标识符</div>
* * <div class="font_min"><span class="key_txt">ACK = 1</span>：该字段为一表示确认号字段有效。此外，TCP 规定在连接建立后传送的所有报文段都必须吧 ACK 设置为 一</div>
* * <div class="font_min"><span class="key_txt">SYN = 1</span>：当 SYN 为1，ACK = 0时，表示当前报文段是一个连接请求报文。当 SYN = 1，ACK = 1时，表示当前报文段是一个同意建立连接的应答报文</div>
* * <div class="font_min"><span class="key_txt">FIN = 1</span>：该字段为一表示此报文段是一个释放连接的请求报文。</div>

###### 建立三次握手
<div class="font_min">首先假设主动发起请求的一端称为客户端，被动连接的一端称为服务端。不管是客户端还是服务端，TCP 连接建立完后都能发送数据和接收数据，所以 TCP 是一个全双工的协议。</div>
<div class="headers mar_top">第一次握手</div>
<div class="font_min">客户端会给服务端发送一个 SYN 报文。</div>

<div class="headers mar_top">第二次握手</div>
<div class="font_min">服务端收到连接请求报文段后，会应答一个 SYN + ACK 报文。</div>

<div class="headers mar_top">第三次握手</div>
<div class="font_min">客户端收到 SYN + ACK 报文后，还需想服务端发送一个确认报文（发送 ACK 报文段给服务端），服务端收到报文段后，此时连接建立成功。</div>

<div class="warning mar_top">为什么 TCP 建立连接需要三次握手，明明两次就可以建立连接</div>
<div class="font_min">因为这是为了防止出现失效的连接请求报文段被服务端接收的情况，从而产生错误。</div>
<div class="font_min">可以想象如下场景。客户端发送了一个 <span class="key_txt">连接请求 A</span>，但是因为网络原因造成 <span class="key_txt">超时</span>，这时 TCP 会启动超时重传的机制再次发送一个 <span class="key_txt">连接请求 B</span>。此时请求顺利到达服务端，服务端应答完建立请求，然后接收数据后释放连接。</div>
<div class="font_min">假设这时候连接请求 A 在两端关闭后抵达服务端，那么此时服务端会认为客户端 <span class="key_txt">又需要建立 TCP 连接</span>，从而应答请求，但客户端此时是 <span class="key_txt">关闭状态</span>，那么就会导致服务端一直等待，造成资源的浪费。</div>
<div class="font_min"><span class="key_txt">PS</span>：在建立连接中，任意一端掉线，TCP 都会重发 SYN 包，一般会重试五次，在建立连接中可能会遇到 SYN Flood 攻击。遇到这种情况你可以选择调低重试次数或者干脆在不能处理的情况下拒绝请求。</div>

###### 断开链接四次握手
<div class="headers mar_top">第一次握手</div>
<div class="font_min">若客户端 A 认为数据发送完成，则它需要向服务端 B 发送连接释放请求。</div>

<div class="headers mar_top">第二次握手</div>
<div class="font_min">B 收到连接释放请求后，会告诉应用层要释放 TCP 链接。然后会发送 ACK 包，并进入 CLOSE_WAIT 状态，此时表明 A 到 B 的连接已经释放，不再接收 A 发的数据了。但是因为 TCP 连接是双向的，所以 B 仍旧可以发送数据给 A。</div>

<div class="headers mar_top">第三次握手</div>
<div class="font_min">B 如果此时还有没发完的数据会继续发送，完毕后会向 A 发送连接释放请求，然后 B 便进入 LAST-ACK 状态。</div>

<div class="headers mar_top">第四次握手</div>
<div class="font_min">A 收到释放请求后，向 B 发送确认应答，此时 A 进入 TIME-WAIT 状态。该状态会持续 2MSL（最大段生存期，指报文段在网络中生存的时间，超时会被抛弃） 时间，若该时间段内没有 B 的重发请求的话，就进入 CLOSED 状态。当 B 收到确认应答后，也便进入 CLOSED 状态。</div>

#### 浏览器缓存策略
<div class="font_min">通常浏览器缓存策略分为两种：<span class="key_txt">强缓存</span> 和 <span class="key_txt">协商缓存</span>，并且缓存策略都是通过设置 <span class="key_txt">HTTP Header （响应头）</span> 来实现的。</div>
<div class="headers">强缓存：可以由这两个字段其中一个决定</div>

* <div class="font_min key_txt">expires</div>
* <div class="font_min key_txt">cache-control (优先级更高)</div>

<div class="headers">协商缓存：可以由这两对字段中的一对决定</div>

* <div class="font_min key_txt">Last-Modified, If-Modified-Since</div>
* <div class="font_min key_txt">Etag, If-None-Match (优先级更高)</div>

##### 强缓存
<div class="font_min">强缓存可以设置两种 HTTP Header 实现：<span class="key_txt">Expires</span> 和 <span class="key_txt">Cache-Control</span>。强缓存表示在缓存期间不需要请求，<span class="key_txt">state code</span>（状态码）为200。</div>

###### Expires
<div class="font_min">我们只需要设置相应头里 <span class="key_txt">expires</span> 的时间为 <span class="key_txt">当前时间 + 30s</span> 就行了</div>
```js
// 设置 Expires 响应头
const time = new Date(Date.now() + 30000).toUTCString()
Expires: web, 07 Apr 2022 07:04:25 GMT
```
<div class="font_min">然后在我们前端页面刷新，可以看到请求的资源的响应头里多了一个 <span class="key_txt">expires</span> 的字段</div>
<image src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e43e2b365de54e7cb0d43733e2e49739~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp" />

<div class="font_min mar_top">并且在 30s 内，我们刷新之后，看到请求都是走 <span class="key_txt">memory</span>，这意味着，通过 expires 设置强缓存的时效是 30s，这 30s 之内，资源都会走本地缓存，而不会重新请求</div>
<image src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7b1b088b04b14abeaef24f03d819321a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp" />

###### cache-control
<div class="font_min">其实 <span class="key_txt">cache-control</span> 跟 <span class="key_txt">expires</span> 效果差不多，只不过这两个字段设置的值不一样而已，expires 设置的是<span class="key_txt">秒数</span>，cache-control 设置的是<span class="key_txt">毫秒数</span></div>
```js
// 设置 Cache-Control 响应头
ctx.set('Cache-Control', 'max-age=30')
```
<div class="font_min">页面响应头多了 <span class="key_txt">Cache-Control</span> 这个字段，且 30s 内都走本地缓存，不会去请求服务端</div>
<image src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ad5f4a1d0254cd2b3e0464ff6ef18f2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp" />

##### 协商缓存
<div class="font_min">如果缓存过期，就需要发起请求验证资源是否有更新。协商缓存可以通过设置两种 HTTP Header 实现：<span class="key_txt">Last-Modified</span> 和 <span class="key_txt">ETag</span></div>
<div class="font_min">当浏览器发起请求验证资源时，如果资源没有做改变，那么服务端会返回 <span class="key_txt">304</span> 状态码，并且更新浏览器缓存有效期</div>

###### Last-Modified, If-Modified-Since
<div class="font_min"><span class="key_txt">Last-Modified</span> 表示本地文件最后修改日期，<span class="key_txt">If-Modified-Since</span> 会将 <span class="key_txt">Last-Modified</span> 的值发送给服务器，询问服务器在该日期后资源是否有更新，有更新的话就将新的资源发送回来，否则返回 <span class="key_txt">403</span> 状态码。</div>

###### ETag, If-None-Match
<div class="font_min"><span class="key_txt">ETag</span> 类似文件指纹，<span class="key_txt">If-None-Match</span> 会将当前的 <span class="key_txt">ETag</span> 发送给服务器，询问该资源 <span class="key_txt">ETag</span> 是否变动，有变动的话就将新的资源发送回来。并且 <span class="key_txt">ETag</span> 优先级比 <span class="key_txt">Last-Modified</span> 高。</div>

##### 总结
<image src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9130c2a31bde41deb268fbfb2da44b85~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp" />