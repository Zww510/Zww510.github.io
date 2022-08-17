---
title: 【掌握原理】实现简易的 Vue 响应式
---

#### 一个基础的响应式
##### 实现一个属性的响应式
<div class="font_min">首先封装一个响应式处理的方法 <span class="key_txt">defineReactive</span>，通过 <span class="key_txt">defineReactive</span> 这个方法重新定义对象属性的 <span class="key_txt">get</span> 和 <span class="key_txt">set</span> 描述符，来实现对数据的劫持，每次 <span class="key_text">读取数据</span> 的时候都会触发 <span class="key_txt">get</span>，每次 <span class="key_text">更新数据</span> 的时候都会触发 <span class="key_txt">set</span>，所以我们可以在 <span class="key_txt">set</span> 中触发更新视图的方法 <span class="key_txt">update</span> 来实现一个基本的响应式</div>
<div class="font_min key_txt">总结起来其实就一句话，在 getter 中收集依赖，在 setter 中触发依赖</div>

```js
/*
* obj 目标对象
* key 目标对象的一个属性
* val 目标对象的一个属性的初始值
*/
function defineReactive(obj, key, val) {
    //通过改方法拦截数据
    Object.defineProperty(obj, key, {
        //读取数据走这里
        get() {
            console.log('🚀🚀~ get:', key)
            return val
        },
        //更新数据走这里
        set(newVal) {
            //只有当新值和旧值不同的时候 才会重新触发赋值操作
            if(newVal !== val) {
                console.log('🚀🚀~ set:', key)
                val = newVal
                // 这里是触发视图更新的地方
                update()
            }
        }
    })
}
```

<div class="font_min">测试一下，每 1s 修改一次 obj.foo 的值，并定义一个 update 方法来修改 app 节点的内容。</div>

```js
//html
<div id="app"></div>

//js
//劫持 obj.foo 属性
const obj = {}
defineReactive(obj, 'foo', '')
// 给 obj.foo 一个初始值
obj.foo = new Date().toLocaleTimeString()

// 定时器修改 obj.foo
setInterval(() => {
  obj.foo = new Date().toLocaleTimeString()
}, 1000)

// 更新视图
function update() {
  app.innerHTML = obj.foo
}
```

<div class="font_min">可以看到，每次修改 <span class="key_txt">obj.foo</span> 的时候，都会触发我们定义的 <span class="key_txt">get</span> 和 <span class="key_txt">set</span>，并调用 <span class="key_txt">update</span> 方法更新了视图，到这里，一个简单的响应式处理就完成了。</div>
<image src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3980d844ff8148f8b1df6757edcb09a4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" />

##### 处理深层次的嵌套
<div class="font_min">一个对象下通常情况下不止一个属性，所以当我们要给每个属性添加响应式的时候，就需要遍历这个对象的所有属性，给每个 <span class="key_txt">key</span> 调用 <span class="key_txt">defineReactive</span> 进行处理</div>

```js
/*
* obj 目标对象
*/
function observe(obj) {
    //先判断类型，响应式处理的目标一定要是个对象类型
    if(typeof obj !== 'object' || obj === null ) return
    //遍历 obj，对 obj 的每个属性进行响应式处理
    Object.keys(obj).forEach(key => defineReactive(obj, key, obj[key]))
}

// 定义对象 obj
const obj = {
  foo: 'foo',
  bar: 'bar',
  friend: {
    name: 'aa'
  }
}

observe(obj)

// 访问 obj 的属性 , foo 和 bar 都被劫持到，就不在浏览器演示了。
obj.bar = 'barrrrrrrr' // => 🚀🚀~ set: bar
obj.foo = 'fooooooooo' // => 🚀🚀~ set: foo

// 访问 obj 的属性 obj.friend.name 
obj.friend.name = 'bb' // => 🚀🚀~ get: friend
```

<div class="font_min">当我们访问 <span class="key_txt">obj.friend.nam</span> 的时候，也只是打印出来 <span class="key_txt">get: friend</span>，而不是 <span class="key_txt">friend.name</span>，所以我们还要进行 <span class="key_txtt">递归</span> 处理，把 <span class="key_txtt">深层次的属性</span> 同样也做响应式处理。</div>

```js
function defineReactive(obj, key, val) {
    //递归
    observe(val)
    //继续执行 Object.defineProperty...
    Object.defineProperty(boj, key, {
        ... ...
    })
}

// 再次访问 obj.friend.name
obj.friend.name = 'bb' // => 🚀🚀~ set: name
```

<div class="font_min">递归的时机在 <span class="key_txt">defineReactive</span> 这个方法中，如果 <span class="key_txt">value</span> 是对象就进行递归，如果不是对象直接返回，继续执行下面的代码，保证 <span class="key_txt">obj</span> 中嵌套的属性都会进行响应式的处理，所以当我们再次访问 <span class="key_txt">obj.friend.name</span> 的时候，就打印出了 <span class="key_txt">set: name</span>。</div>

##### 处理直接赋值一个对象
<div class="font_min">上面已经实现了对深层属性的响应式处理，那么如果直接给一个属性赋值一个对象呢？</div>

```js
const obj = {
    friend: {
        name: 'aa'
    }
}
obj.friend = {              // => 🚀🚀~ set: friend
    name: 'bb'
}
obj.friend.name = 'cc'      // => 🚀🚀~ get: friend
```

<div class="font_min">这种赋值方式还只是打印了 <span class="key_txt">get: friend</span>，并没有劫持到 <span class="key_txt">obj.friend.name</span>，那该如何处理？我们只需要在触发 <span class="key_txt">set</span> 的时候判断一下 <span class="key_txt">value</span> 的类型，如果它是个对象类型，我们就对它执行 <span class="key_txt">observe</span> 方法。</div>

```js
function defineReactive(obj, key, val) {
    Object.defineProperty(obj, key, {
        ... ...
        set(newVal) {
            // 新值和旧值不同，才会触发赋值操作
            if(newVal !== val) {
                console.log('🚀🚀~ set:', key)
                //如果 newVal 是一个对象类型，再次做响应式处理
                if(typeof obj === 'object' && obj !== null) {
                    observe(newVal)
                }
                val = newVal
            }
        }
    })
}
// 再次给 obj.friend 赋值一个对象
obj.friend = {
  name: 'bb'
}
// 再次访问 obj.friend.name , 这个时候就成功的劫持到了 name 属性
obj.friend.name = 'cc'  //=> 🚀~ set: name
```

#### Vue 中的数据响应式
##### 实现简易的 Vue
<div class="font_min">这是 Vue 最基本的使用方式，创建一个 Vue 的实例，然后就可以在模板中使用 <span class="key_txt">data</span> 中定义的响应式数据。</div>

```html
<div id="app">
<p>{{counter}}</p>
<p my-text="counter"></p>
<p my-html="desc"></p>
<button @click='add'>点击增加</button>
<p>{{name}}</p>
<input type="text" my-model='name'>
</div>

<script>
    const app = new MyVue({
        el: '#app',
        data: {
            counter: 1,
            desc: `<span style='color:red' >Vue 响应式</span>`,
            name: ''
        },
        methods: {
            add() {
                this.counter++
            }
        }
    })
</script>
```
##### 原理
<image src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/796a19355cad4c8eb7013cdbff9de6b8~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" />

###### 设计类型介绍

* <div class="font_min"><span class="headers">MyVue</span>：框架构造函数</div>
* <div class="font_min"><span class="headers">Observer</span>：执行数据响应化（区分数据是对象还是数组）</div>
* <div class="font_min"><span class="headers">Compile</span>：编译模板，初始化视图，收集依赖（更新函数，创建 <span class="key_txt">watcher</span>）</div>
* <div class="font_min"><span class="headers">Watcher</span>：执行更新函数（更新 <span class="key_txt">dom</span>）</div>
* <div class="font_min"><span class="headers">Dep</span>：管理多个 <span class="key_txt">Watcher</span> 批量更新</div>

###### 流程解析

* <div class="font_min">初始化时通过 <span class="key_txt">Observer</span> 对数据进行响应式处理，在 <span class="key_txt">Observer</span> 的 <span class="key_txt">get</span> 的时候创建一个 <span class="key_txt">Dep</span> 实例，用来通知更新。</div>
* <div class="font_min">初始化通过 <span class="key_txt">Compile</span> 进行编译，解析模板语法，找到其中动态绑定的数据，从 <span class="key_txt">data</span> 中获取数据并初始化视图，把模板语法替换成数据。</div>
* <div class="font_min">同时进行一次订阅，创建一个 <span class="key_txt">Watcher</span>，定义一个更新函数，将来数据发生变化时，<span class="key_txt">Watcher</span> 会调用更新函数，把 <span class="key_txt">Watcher</span> 添加到 <span class="key_txt">Dep</span> 中。</div>
* <div class="font_min"><span class="key_txt">Watcher</span> 是<span class="key_text">一对一</span>负责每个具体的元素，<span class="key_txt">Data</span> 的某个属性可能在一个视图中出现<span class="key_text">多次</span>，也就是会创建多个 <span class="key_txt">Watcher</span>，所以一个 <span class="key_txt">Dep</span> 中会管理多个 <span class="key_txt">Watcher。</span></div>
* <div class="font_min">当 <span class="key_txt">Observer</span> 监听到数据发生变化时，<span class="key_txt">Dep</span> 通知所有的 <span class="key_txt">Watcher</span> 进行视图更新。</div>

##### 代码实现- 第一回合 数据响应式
<div class="font_min headers">observe</div>
<div class="font_min"><span class="key_txt">observe</span>方法相对于上面，做了一点小改动，不是直接遍历调用 <span class="key_txt">difineReactive</span> 了，而是创建一个 <span class="key_txt">Observer</span> 类的实例</div>

```js
function observe(obj) {
    // 先判断类型, 响应式处理的目标一定要是个对象类型
    if(typeof obj !== 'object' || obj === null) return
    new Observer(obj)
}
```

<div class="font_min headers">Observer</div>
<div class="font_min"><span class="key_txt">Observer</span>它就是用来做 <span class="key_text">数据响应式</span> 的，在它的内部分别区分了 <span class="key_text">数组</span> 还是 <span class="key_text">对象</span>，然后执行不同的响应式方案。</div>

```js
// 根据传入的value的类型做对应的响应式处理
class Observer {
    constructor(value) {
        this.value = value
        if(Array.isArray(value)) {
            // todo 这个分支主要针对与数组的响应式处理方案，不是重点，暂时忽略
        }else {
            //这个分支是对对象的响应式处理方案
            this.walk(value)
        }
    }

    walk(obj) {
        //遍历 obj，对 obj 的每个属性进行响应式处理
        Object.keys(obj).forEach(key => {
            defineReactive(obj, key, obj[key])
        })
    }
}
```

<div class="font_min headers">MvvM 类 (MyVue)</div>
<div class="font_min">我们现在实例初始化的时候，对 <span class="key_txt">data</span> 进行响应式处理，为了能用 <span class="key_txt">this.key</span> 的方式访问 <span class="key_txt">this.$data.key</span>，需要做一层<span class="key_text">代理</span>。</div>

```js
class MyVue {
    constructor(options) {
        //把数据存下来
        this.$options = options
        this.$data = options.data

        //data 响应式处理
        observe(this.$data)

        //代理，把 this.$data 上的属性全部挂载到 Vue 的实例上，可以通过 this.key 访问 thsi.$data.key
        proxy(this)

        new Compile(options.el, this)
    }
}
```

<div class="font_min"><span class="key_txt">proxy</span>代理就是通过 <span class="key_txt">Object.defineProperty</span> 改变一下引用。</div>

```js
/**
 * 代理 把 this.$data 上的属性 全部挂载到 vue实例上 可以通过 this.key 访问 this.$data.key
 * @param {*} vm vue 实例
 */
function proxy(vm) {
    Object.keys(vm.$data).forEach(key => {
        // 通过  Object.defineProperty 方法进行代理 这样访问 this.key 等价于访问 this.$data.key
        Object.defineProperty(vm, key, {
            get() {
                return vm.$data[key]
            },
            set(newValue) {
                vm.$data[key] = newValue
            }
        })
    })
}
```

##### 代码实现- 第二回合 模板编译
<image src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c186c2023b1a4f24a735c50181427297~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" />

```js
// 解析模板语法
// 1.处理插值表达式{{}}
// 2.处理指令和事件
// 3.以上两者初始化和更新
class Compile {
    /**
    * @param {*} el 宿主元素
    * @param {*} vm vue实例
    */
    constructor(el, vm) {
        this.$vm = vm
        this.$el = document.querySelector(el)

        //如果元素存在，执行编译
        if(this.$el) {
            this.compile(this.$el)
        }
    }

    //编译
    compile(el) {
        //获取 el 的子节点，判断他们的类型做相应的处理
        const childNodes = el.childNodes

        childNodes.forEach(node => {
            /* 
            * 判断节点的类型 本文已元素和文本为主要内容 不考虑其他类型
            * 获得 body 元素的节点类型
            * 1 为元素
            * 2 为属性
            * 3 代表元素或属性中的文本内容
            */
            if(node.nodeType === 1) {//这个分支代表节点是 元素 类型
                //获取到元素上的数组
                const attrs = node.attributes
                //把 attrs 转换成真实的数组
                Array.from(attrs).forEach(attr => {
                    //指令是 my-xxx = 'abc' 这样子
                    //获取节点属性名
                    const attrName = attr.name
                    //获取节点属性值
                    const exp = attr.value
                    //判断节点属性是不是一个指令
                    if(attrName.startsWith('my-')){
                        //获取具体的指令类型 也就是 my-xxx 后面的 xxx 部分
                        const dir = attrName.substring(3)
                        //如果 this[xxx] 指令存在 执行这个命令
                        this[dir] && this[dir](node, exp)
                    }
                })
            }else if(this.isInter(node)) {// 这个分支代表节点的类型是文本 并且是个差值语法 {{}}
                //文本的初始化
                this.compileText(node)
            }
            //递归遍历 DOM 树
            if(node.childNodes) {
                this.compile(node)
            }
        })
    }

    //编译文本
    compileText(node) {
        //可以通过 RegExp.$1 来获取到 插值表达中间的内容 {{key}}
        //this.$vm[RegExp.$1] 等价与 this.$vm[key]
        //然后把这个 this.$vm[key] 的值赋值给文本就完成了 文本的初始化
        node.textContent = this.$vm[RegExp.$1]
    }

    //my-text 指令对应的方法
    text(node, exp) {
        //这个指令用来修改节点的文本，长这样 my-text = 'key'
        //把 this.$vm[key] 赋值给文本即可
        node.textContent = this.$vm[exp]
    }

    //my-html 指令对应的方法
    html(node, exp) {
        // 这个指令用来修改节点的文本,这个指令长这样子 my-html = 'key'
        // 把 this.$vm[key] 赋值给innerHTML 即可
        node.innerHTML = this.$vm[exp]
    }

    //是否是差值表达式{{}}
    isInter(node) {
        return node.nodeType === 3 && /\{\{(.*)\}\}/.test(node.textContent)
    }
}
```

##### 代码实现- 第三回合 收集依赖
<div class="font_min">视图会用到 <span class="key_txt">data</span> 中的属性 <span class="key_txt">key</span> 的地方，都可以被称为一个 <span class="key_text">依赖</span>，同一个 <span class="key_txt">key</span> 可能会出现 <span class="key_text">多次</span>，每次出现都会创建一个 <span class="key_txt">Watcher</span> 进行维护，这些 <span class="key_txt">Watcher</span> 需要收集起来统一进行管理，这个过程叫做 <span class="key_text">收集依赖</span>。</div>
<div class="font_min">同一个 <span class="key_txt">key</span> 创建多个 <span class="key_txt">Watcher</span> 需要一个 <span class="key_txt">Dep</span> 来管理，需要更新时由 <span class="key_txt">Dep</span> 统一进行通知。</div>
<image src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/db289f3fbd7f4c67b25dc7d57dbe39de~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" />

<div class="font_min mar_top">上述图片代码中，<span class="key_txt">name1</span> 用到了两次，创建了两个 <span class="key_txt">Watcher</span>，<span class="key_txt">Dep1</span> 收集了这两个 <span class="key_txt">Watcher</span>，<span class="key_txt">name2</span> 用到了一次，创建了一个 <span class="key_txt">Watcher</span>，<span class="key_txt">Dep2</span> 收集这一个 <span class="key_txt">Watcher</span>。</div>
<image src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ebe7fbb834354ecca1ac415bf09b1561~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" />

###### 收集依赖的思路

* <div class="font_min"><span class="key_txt"></span>defineReactive 时为每个 <span class="key_txt">key</span> 创建一个 <span class="key_txt">Dep</span> 实例</div>
* <div class="font_min">初始化视图时，读取某个 <span class="key_txt">key</span>，例如 <span class="key_txt">name1</span>，创建一个 <span class="key_txt">Watcher1</span></div>
* <div class="font_min">由于触发 <span class="key_txt">name1</span> 的 <span class="key_txt">getter</span> 方法，便将 <span class="key_txt">Watcher1</span> 添加到 <span class="key_txt">name1</span> 对应的 <span class="key_txt">Dep</span> 中</div>
* <div class="font_min">当 <span class="key_txt">name1</span> 发生更新时，会触发 <span class="key_txt">setter</span>，便可通过对应的 <span class="key_txt">Dep</span> 通知其管理所有的 <span class="key_txt">Watcher</span> 进行视图的更新</div>

###### Watcher 类
<div class="font_min">收集依赖的过程中，在 <span class="key_txt">Watcher</span> 创建实例的时候，首先把实例赋值给 <span class="key_txt">Dep.target</span>，手动读一下 <span class="key_txt">data.key</span> 的值，触发 <span class="key_txt">defineReactive</span> 的 <span class="key_txt">get</span>，把当前的 <span class="key_txt">Watcher</span> 实例添加到 <span class="key_txt">Dep</span> 中进行管理，然后再把 <span class="key_txt">Dep.target</span> 赋值为 <span class="key_txt">null</span>。</div>
<div class="font_min">订阅者 <span class="key_txt">Watcher</span> 在初始化的时候需要将自己添加进 订阅器 <span class="key_txt">Dep</span> 中，已知监听器 <span class="key_txt">Observer</span> 是在 <span class="key_txt">get</span> 函数执行时添加订阅者 <span class="key_txt">Watcher</span> 操作的，所以我们只要在订阅者 <span class="key_txt">Watcher</span> 初始化的时候触发对应的 <span class="key_txt">get</span> 函数执行添加订阅者即可，那该如何触发 <span class="key_txt">get</span> 的函数，手动读一下 <span class="key_txt">data.key</span> 的值，核心原因是我们使用了 <span class="key_txt">Object.deinfeProperty()</span> 进行数据监听。</div>

```js
class Watcher {
    /**
      * @param {*} vm vue 实例
      * @param {*} key Watcher实例对应的 data.key 如 v-model="name"，key 就是name;
      * @param {*} cb 绑定的更新函数 
      */
    constructor(vm, key, updateFn) {
        this.vm = vm
        this.key = key
        this.updateFn = updateFn

        //触发依赖收集 把当前 Watcher赋值给 Dep 的静态属性 target - 全局变量 订阅者 赋值
        Dep.target = this
        //故意读一下 date.key 的值 为了触发 defineReactive 中的 get
        this.vm[this.key]
        //收集依赖后 在重置为null - 全局变量 订阅者 赋值
        Dep.target = null
    }

    //更新方法 未来被 Dep 调用
    update() {
        //执行实际的更新操作
        this.updateFn.call(this.vm, this.vm[this.key])
    }
}
```

###### Dep 类
<div class="font_min"><span class="key_txt">addDep</span> 方法把 <span class="key_txt">Watcher</span> 收集起来，放在 <span class="key_txt">deps</span> 中进行管理，<span class="key_txt">notify</span> 方法通知 <span class="key_txt">deps</span> 中的所有 <span class="key_txt">Watcher</span> 进行视图更新</div>

```js
class Dep {
    constructor() {
        this.deps = [] //存放 Watchers
    }
    
    //收集 Watchers
    addDep(dep) {
        this.deps.push(dep)
    }

    //通知所有的 Watchers 进行更新 这里的 dep 指的就是收集起来的 Watcher
    notify() {
        this.deps.forEach(dep => dep.update())
    }
}
```

###### 升级 Compile
<div class="font_min">在第二回合中，我们的 <span class="key_txt">Compile</span> 类只实现了视图的初始化，所以在第三回合要把它升级一下，支持视图更新。</div>
<div class="font_min"><span class="key_txt">Watcher</span> 实例就是在初始化后创建的，用来监听更新。</div>

```js
class Compile {
... ... //省略号的地方都没有发生改变
// 下面是发生改变的代码
/**
* 根据指令的类型操作 dom 节点
* @param {*} node dom节点
* @param {*} exp 表达式 this.$vm[key]
* @param {*} dir 指令
*/
    update(node, exp, dir) {
        // 1.初始化 获取到指令对应的实操函数
        const fn = this[dir + 'Updater']
        //如果函数存在就执行
        fn && fn(node, this.$vm[exp])
        // 2.更新 再次调用指令对应的实操函数 值由外部传入
        new Watcher(this.$vm, exp, function(val) {
            fn && fn(node, val)
        })
    }
    
    // 编译文本 {{xxx}}
    compileText(node) {
        // 可以通过 RegExp.$1 来获取到 插值表达式中间的内容 {{key}}
        // this.$vm[RegExp.$1] 等价于 this.$vm[key]
        // 然后把这个 this.$vm[key] 的值 赋值给文本 就完成了 文本的初始化
        this.update(node, RegExp.$1, 'text')
    }

    //my-text 指令对应的方法
    text(node, exp) {
        //这个指令用来修改节点的文本，长这样 my-text = 'key'
        //把 this.$vm[key] 赋值给文本即可
        this.update(node, exp, 'text')
    }

    // my-text 指令对应的实操
    textUpdater(node, value) {
        // 这个指令用来修改节点的文本,这个指令长这样子 my-text = 'key'
        // 把 this.$vm[key] 赋值给文本 即可
        node.textContent = value
    }

    // my-html 指令
    html(node, exp) {
        this.update(node, exp, 'html')
    }

    // my-html 指令对应的实操
    htmlUpdater(node, value) {
        // 这个指令用来修改节点的文本,这个指令长这样子 my-html = 'key'
        // 把 this.$vm[key] 赋值给innerHTML 即可
        node.innerHTML = value
    }

    // 是否是插值表达式{{}}
    isInter(node) {
        return node.nodeType === 3 && /\{\{(.*)\}\}/.test(node.textContent)
    }

}
```

###### Watcher 和 Dep 建立关联
<div class="font_min">首先在 <span class="key_txt">defineReactive</span> 中创建 <span class="key_txt">Dep</span> 的实例，与 <span class="key_txt">data.key</span> 是一一对应的关系，然后再 <span class="key_txt">get</span> 中调用 <span class="key_txt">Dep.addDep</span> 进行依赖的收集，<span class="key_txt">Dep.target</span> 就是一个 <span class="key_txt">Watcher</span>。在 <span class="key_txt">set</span> 中调用 <span class="key_txt">Dep.notify()</span> 通知所有的 <span class="key_txt">Watchers</span> 更新视图。</div>

```js
function defineReactive(obj, key, val) {
    ... ...
    //创建 Dep 实例，与 key 一一对应
    const dep = new Dep()

    //拦截数据
    Object.defineProperty(obj, key, {
        //数据读取
        get() {
            console.log('🚀🚀~ get:', key)
            //依赖收集 Dep.target 就是一个 Watcher
            Dep.target && dep.addDep(Dep.target)

            return val
        },
        //更新数据
        set(newVal) {
            //新值和旧值不同，重新触发赋值操作
            if(newVal !== val) {
                console.log('🚀🚀~ set:', key)
                //如果 newVal 是一个对象类型，再次做响应式处理
                if(typeof newVal === 'object' && newVal !== null) {
                    observe(newVal)
                }
                val = newVal

                //通知更新
                dep.notify()
            }
        }
    })
}
```

##### 代码实现- 第四回合 事件和双向绑定
###### 事件绑定
<div class="font_min">首先判断节点的属性是不是已 <span class="key_txt">@</span> 开头的，然后拿到事件的类型，也就是例子中的 <span class="key_txt">click</span>，再根据函数名找到 <span class="key_txt">methods</span> 中定义的函数体，最后添加事件监听就行了。</div>

```js
class Compile {
    ... ... //都没有发送变化
    compile(el) {
        //判断节点是不是一个事件
        if(this.isEvent(attrName)) {
            // @click = 'onClick'
            const dir = attrName.substring(1) //click
            //事件监听
            this.eventHandler(node, exp, dir)
        }
    }
    ... ...
    //判断节点是不是一个事件，就是已 @ 开头的
    isEvent(event) {
        return event.indexOf('@') === 0
    }
    eventHandler(node, exp, dir) {
        //根据函数名字在配置项中获取函数体
        const fn = this.$vm.$options.methods && this.$vm.$options.methods[exp]
        //添加事件监听
        node.addEventListener(dir, fn.bind(this.$vm))
    }
}
```

###### 双向绑定
<div class="font_min"><span class="key_txt">my-model</span> 其实也是一个指令，走的也是指令相关的处理逻辑，所以我们只需要添加一个 <span class="key_txt">model</span> 指令和对应的 <span class="key_txt">modelUpdater</span> 处理函数就可以了。</div>
<div class="font_min"><span class="key_txt">my-model</span> 双向绑定其实就是 <span class="key_text">事件绑定</span> 和修改 <span class="key_txt">value</span> 的一个语法糖，这里以 <span class="key_txt">input</span> 为例，其他的表单元素绑定的事件会有不同，但是道理一样。</div>

```js
class Compile {
    //my-model 指令 my-model='xxx'
    model(node, exp) {
        //update 方法只完成赋值和更新
        this.update(node, exp, 'model')
        //事件监听
        node.addEventListener('input', e => {
            //将新的值赋值给 data.key 即可
            this.$vm[exp] = e.target.value
        })
    }

    modelUpdater(node, value) {
        //给表单元素赋值
        node.value = value
    }
}
```

<div class="font_min">现在可以更新一下模板编译的流程图啦。</div>
<image src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c9a46fe73e2b4b4ea5bf1fd8abd6b317~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" />

#### 后语
<image src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6f8adc9923e1470382bc73fc1238aa1f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" />
<div class="font_min">到这里一个简易版的 <span class="key_txt">Vue 数据响应式</span>就完成了，整套流程从头到尾都是自己手写的，还怕不懂原理么？</div>

<div class="font_min mar_top">本文的完整示例代码地址如下👉👉<a class="headers" href="https://github.com/Zww510/vue-model">完整代码</a></div>