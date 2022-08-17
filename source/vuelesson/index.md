---
title: Vue知识点
---

##### >> Vue的优点？Vue的缺点？
<div class="font_min"><span class="key_txt">优点</span>：渐进式，组件化，轻量级，虚拟 dom，响应式，单页面路由，数据和视图分开</div>
<div class="font_min"><span class="key_txt">缺点</span>：单页面不利于 SEO 优化，不支持 IE8 以下，首屏加载时间长</div>

##### >> Vue 跟 React 的异同点？
<div class="font_min headers">相同点</div>

* <div class="font_min">1. 都使用了虚拟 DOM。</div>
* <div class="font_min">2. 组件化开发</div>
* <div class="font_min">3. 都是单向数据流。（父子组件之间，不建议子修改父传下来的数据）</div>
* <div class="font_min">4. 都支持服务端渲染</div>

<div class="font_min headers">不同点</div>

* <div class="font_min">1. React 的 JSX，Vue 的template。</div>
* <div class="font_min">2. 数据变化，React 手动（setState），Vue 自动（初始化已响应式处理，Object.defineProperty）。</div>
* <div class="font_min">3. React 是单向绑定，Vue 是双向绑定。</div>
* <div class="font_min">4. React 的状态管理工具是 Redux，Vue 的是 Vuex。</div>

##### >> 怎样理解 Vue 的单向数据流？
<div class="font_min">所有的 prop 都使其父子 prop 之间形成一个 单向下行绑定：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外改变父组件的状态，从而导致你的数据应用流向难以理解</div>
<div class="font_min mar_top">额外的，每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你不应该在一个子组件内部改变 prop，如果你这样做了，Vue 在浏览器的控制台中会发生警告。若子组件想要修改时，只能通过 $emit 派发一个自定义事件，父组件接受到后，由父组件修改</div>
<div class="font_min mar_top">有两种常见试图改变一个 prop 的情形：</div>

* <div class="font_min">这个 prop 用来传递一个初始值：这个子组件接下来希望将作为一个本地的 prop 数据来使用。在这种情况下，最好定义一个本地的 data 属性并将这个 prop 用作其初始值：</div>
```js
props: ['initialCounter'],
data() {
    return {
        counter: this.initialCounter
    }
}
```
* <div class="font_min">这个 prop 以一种原始的值传入且需要进行转换。在这种情况下，最好使用这个 prop 的值来定义一个计算属性</div>
```js
props: ['size'],
computed: {
    normalizedSize() {
        return this.size.trim().toLowerCase()
    }
}
```

##### >> 谈谈你对生命周期的理解
<div class="font_min headers">1. 生命周期时什么</div>
<div class="font_min">Vue 实例有一个完整的生命周期，也就是从开始创建，初始化数据，编译模板，挂载 DOM -> 渲染，更新 -> 渲染，卸载等一些列过程，我们称这是 Vue 的生命周期</div>
<div class="font_min headers mar_top">2. 各个生命周期的作用</div>

| 生命周期         | 描述             |
| --------------- | ---------------- |
| beforeCreate    | 组件实例被创建之初，组件的属性生效之前 |
| created         | 组件实例已经完全创建，属性也绑定，但真实 DOM 还没有生成，$el 还不可用，data 中有值 |
| beforeMount     | 在模板渲染之前执行的函数，实例被挂载到真实的 DOM 节点 |
| mounted         | 此时可以操作 DOM |
| beforeUpdate    | 组件数据更新之前调用 |
| updated          | 数据更新之后 |
| activited       | keep-alive 专属，组件激活时调用 |
| deactivated     | keep-alive 专属，组件被销毁时调用 |
| beforeDestory   | 实例被销毁前，此时可以手动销毁一些方法 |
| destoryed       | 实例销毁后调用 |

<div class="font_min headers mar_top">3. 生命周期示意图</div>
<img alt="1.png" src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/8/19/16ca74f183827f46~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp" loading="lazy" class="medium-zoom-image">

##### >> Vue 的父组件和子组件生命周期钩子函数执行顺序？
<div class="font_min">Vue 的父组件和子组件生命周期钩子函数执行顺序可以归类为：</div>

* <div class="font_min">加载渲染过程</div>
<div class="font_min">父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted</div>

* <div class="font_min">子组件更新过程</div>
<div class="font_min">父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated</div>

* <div class="font_min">销毁过程</div>
<div class="font_min">父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed </div>

##### >> Vue的双向绑定原理
<div class="font_min">是采用数据劫持发布者 - 订阅者的模式，通过Object.defineProperty() 来劫持各个属性的 setter 和 getter，在数据变动时发布消息给订阅者，触发相应的监听回调。主要分为以下几个步骤</div>

* <div class="font_min"><span class="key_txt">Observer</span>：对数据对象进行递归遍历，这样可以确保子属性对象的属性也可以给劫持到，使用 Object.defineProperty() 劫持 set 和 get，数据变动，就会触发 setter，就能监听到了数据变化</div>
* <div class="font_min"><span class="key_txt">Compile</span>：解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据变动，收到通知，更新视图</div>
* <div class="font_min"><span class="key_txt">Watcher</span>：订阅者，是 Observer 和 Compile 之间的通行桥梁，主要做的事情是：</div>
* * <div class="font_min">1. 在自身实例化时往属性订阅器（dep）里面添加自己</div>
* * <div class="font_min">2. 自身必须有一个 update() 方法</div>
* * <div class="font_min">3. 待属性变动，也就是触发 set 方法时，dep.notify() 通知 deps 中所有的 Watcher 进行视图更新</div>

* <div class="font_min"><span class="key_txt">MVVM</span>：作为数据绑定的入口，整合 Observer，Compile，Watcher 三者，通过 Observer 来监听 model 数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭通 Observer 和 Compile 之间的通信桥梁，达到 数据变化 -> 视图更新</div>

##### >> 为什么 Data 是一个函数，且返回一个对象呢？
<div class="font_min">组件会被多次调用，如果 data 不是一个函数，组件之间的数据会出现数据污染</div>

##### >> 谈谈你对 keep-alive 的了解？
<div class="font_min">keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，避免重新渲染，其有一下特性：</div>

* <div class="font_min">一般结合路由和动态组件一起使用，用于缓存组件</div>
* <div class="font_min">提供 include 和 exclude 属性，两者都支持字符串或正则表达式，include 表示只有名称匹配的组件会被缓存，exclude 表示任何名称匹配的组件都不会被缓存，其中 exclude 比 include 优先级要高</div>
* <div class="font_min">对应两个钩子函数 activated 和 deactivated，当组件被激活时，触发钩子函数 activated，当组件被销毁时，触发钩子函数 deactivated</div>

##### >> Vue 的修饰符
<image src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5a1c911988f74cea91da79af3c6049c2~tplv-k3u1fbpfcp-watermark.awebp" />

##### >> Vue 的内部指令
<image src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d39d348e686b449e8931f5a85802e3c6~tplv-k3u1fbpfcp-watermark.awebp" />

##### >> v-if 和 v-show 有何区别？
* <div class="font_min">1. v-if 是通过控制 DOM 元素的删除和生成来实现删除和生成，类似 display: none</div>
* <div class="font_min">2. v-show 是通过控制 DOM 元素的 css 样式来实现隐藏和显示，不会销毁，类似 visibility: hidden</div>
* <div class="font_min">3. 频繁或者大数量显隐使用 v-show，否则使用 v-if</div>

##### >> Class 与 Style 如何动态绑定？
<div class="font_min">Class 可以通过对象语法和数组语法进行绑定</div>

* <div class="font_min">对象语法：</div>
```js
<div :class="{active: isActive, 'text-danger': hasError}"></div>

data: {
    isActive: true,
    hasError: false
}
```

* <div class="font_min">数组语法</div>
```js
<div :class="[isActive ? activeClass : '', errorClass]"></div>

data: {
    activeClass: 'active',
    errorClass: 'text-danger'
}
```

<div class="font_min">Style 也可以通过对象语法和数组语法进行动态绑定</div>

##### >> 为什么 v-if 和 v-for 不建议用在同一标签？
<div class="font_min">1. v-for 的优先级大于 v-if，如果要遍历的数组很大，但展示的数据很少，会造成性能浪费。</div>
<div class="font_min">2. 建议使用 computed 过滤数据先</div>

##### >> Vuex 的理解
<div class="font_min">Vuex 是一个专门为 Vue 应用程序开发的状态管理工具，每一个 Vuex 应用的核心就是 store（仓库）</div>
<div class="markdown-body">
<div class="font_min">1. Vuex 的状态是响应式的，当 Vue 组件从 store 中读取状态的时候，若 store 的状态发生变化，那么相应的组件也会得到更新响应</div>
<div class="font_min">2. 改变 store 中的状态唯一途径就是提交 commit，mutation</div>
</div>

* <div class="font_min"><span class="key_txt">State</span>：定义了应用状态的数据结构，可以在这里设置默认的初始值</div>
* <div class="font_min"><span class="key_txt">Getter</span>：允许组件从 store 中获取数据，mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部的计算属性</div>
* <div class="font_min"><span class="key_txt">Mutation</span>：是唯一更改 store 中状态的方法，且必须是同步函数，搭配 commit</div>
* <div class="font_min"><span class="key_txt">Action</span>：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作。搭配 dispatch</div>
* <div class="font_min"><span class="key_txt">Module</span>：允许将单一的 store 拆分为多个 sotre 且同时保存在单一的状态树中</div>
<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a7249773a1634f779c48f3f0ffabf968~tplv-k3u1fbpfcp-watermark.awebp" width=700 height=550 />

<h6 class="font_min">搭配四个 map 辅助函数：mapState, mapGetters, mapMutations, mapActions 来使用</h6>
<div class="font_min"><span class="key_txt">mapState</span> 方法：用于帮助映射 state 中的数据为计算属性</div>

```js
computed: {
    //借助 mapState 生成计算属性（对象写法）
    ...mapState({sum: 'sum', school: 'school'})

    //借助 mapState 生成计算属性（数组写法）
    ...mapState(['sum', 'school'])
}
```

<div class="font_min"><span class="key_txt">mapGetters</span> 方法：用于帮助映射 getters 中的数据为计算属性</div>

```js
computed: {
    //借助 mapGetters 生成计算属性（对象写法）
    ...mapGetters({bigSum: 'bigSum'})

    //借助 mapGetters 生成计算属性（数组写法）
    ...mapGetters(['bigSum'])
}
```

<div class="font_min"><span class="key_txt">mapActions</span> 方法：用于帮助生成与 actions 对话的方法，即包含 $store.dispatch(xxx) 函数</div>

```js
methods: {
    //靠 mapActions 生成：incrementOdd（对象形式）
    //incrementOdd 当前组件的事件名 
    //jiaOdd Vuex的 actions 方法
    ...mapActions({incrementOdd: 'jiaOdd'})

    //靠 mapActions 生成：incrementOdd（数组形式）
    ...mapActions(['jiaOdd'])
}
```

<div class="font_min"><span class="key_txt">mapMutations</span> 方法：用于帮助生成与 mutations 对话的方法，即包含 $store.commit(xxx) 函数</div>

```js
methods: {
    //靠 mapMutations 生成：increment（对象形式）
    //increment 当前组件的事件名
    //JIA Vuex的 mutations 方法
    ...mapMutations({increment: 'JIA'})

    //靠 mapMutations 生成：JIA（数组形式）
    ...mapMutations(['JIA'])
}
```

##### >> Vuex 页面刷新数据丢失如何解决
<div class="font_min">需要做 vuex 数据持久化，一般使用本地存储的方案来保存数据，也可以使用第三方插件</div>
<div class="font_min">推荐使用 <span class="key_txt">vuex-persist</span> 插件，它就是为 vuex 持久化存储而生的一个插件，不需要你手动存取 store，而是直接将状态保存到 cookie 或者 localStorage 中</div>

##### >> 不需要响应式的数据应该如何处理
<div class="font_min">在开发中，会有一些数据，从始至终都未曾变过，这种死数据，既然不会改变，那也就不需要对它做响应式处理，不然只会消耗性能。</div>

```bash
//1. 将数据定义在data之外
data() {
    this.list1 = { xxxxxxxxx }
    return { }
}

//2. 使用 boject.freeze() 方法
data() {
    return {
        list1: object.freeze({ xxxxxxxxx })
    }
}
```

##### >>watch 有哪些属性，分别的作用
<div class="font_min">当我们监听一个基本数据类型的时候</div>

```bash
watch: {
    value() {
        //do something
    }
}
```

<div class="font_min">当我们监听一个引用数据类型的时候。 <span class="key_txt">handler</span> - <span class="key_txt">deep</span> - <span class="key_txt">immediate</span></div>

```bash
watch: {
    obj: {
        handler() { //执行回调
            //do something
        },
        deep: true, //是否进行深度监听
        immediate: true //是否初始执行 handler 函数
    }
}
```

##### >>vue 的 hook 使用
<div class="font_min headers">同一组件使用</div>
<div class="markdown-body">这是我们常用的定时器方式</div>

```bash
export default{
    data() {
        return {
            timer: null
        }
    },
    mounted() {
        this.timer = setInterval(() => {
            //具体内容
            console.log('1')
        },1000)
    },
    beforeDestory() {
        clearInterval(this.timer)
        this.timer = null
    }
}
```

<div class="markdown-body">上面的做法不好的地方在于，得全局定义一个 timer 变量，可以使用 hook 这么实现</div>

```bash
export default {
    methods: {
        fn() {
            let timer = setInterval(() => {
                //具体内容
                console.log('1')
            },1000);
            this.$once('hook:beforeDestory', () => {
                clearInterval(timer)
                timer = null
            })
        }
    }
}
```

<div class="font_min headers">父子组件使用</div>
<div class="markdown-body">如果子组件需要在 mounted 时触发父组件的某一个函数，平时都这么写</div>

```bash
//父组件
<rl-child @childMounted="childMounted" />
methods: {
    childMounted() {
        // do something
    }
}

//子组件
mounted() {
    this.$emit('childMounted')
}
```

<div class="markdown-body">使用 hook 的话可以更方便</div>

```bash
//这里不一定是 mounted，子组件中的任意一个生命周期都可以被监听到
<rl-child @hook:mounted="childMounted" />
methods: {
    childMounted() {
        // do something
    }
}

//子组件什么也不需要操作
```

##### >>Vue 自定义指令
<div class="font_min">在 Vue，除了核心功能默认设置的内置指令（v-module, v-show, v-if, v-for 等）,Vue 也允许注册自定义指令。它的作用价值在于当开发人员在某些场景下需要对普通 DOM 元素进行操作</div>
<div class="font_min">Vue 自定义指令有全局注册和局部注册两种方式。通过 <span class="key_txt">Vue.directive(id, [definition])</span> 方法注册全局指令，然后在入口文件中进行 Vue.use() 调用</div>
<div class="font_min">局部注册在需要使用的页面 import 引入，使用 <span class="key_txt">directive: {name}</span> </div>

<div class="font_min">指令定义函数提供了几个<span class="key_txt"></span>钩子函数（可选）：</div>

* <div class="font_min"><span class="key_txt">bind</span>：只调用一次，指令第一次绑定到元素时调用，可以定义一个在绑定时执行一次的初始化动作。</div>
* <div class="font_min"><span class="key_txt">inserted</span>：被绑定元素插入父节点时调用（父节点存在即可调用，不必存在与 document 中）。</div>
* <div class="font_min"><span class="key_txt">update</span>：被绑定元素所在的模板更新时调用，而不论绑定值是否变化，通过比较更新前后的绑定值。</div>
* <div class="font_min"><span class="key_txt">componentUpdated</span>：被绑定的元素所在模块完成一次更新周期时调用。</div>
* <div class="font_min"><span class="key_txt">unbind</span>：只调用一次，指令与元素解绑时调用。</div>

###### v-permission (权限校验指令)
<div class="font_min">背景：在一些后台管理系统，我们可能需要根据用户角色进行一些操作权限的判断，很多时候我们都是粗暴地给一个元素添加 <span class="key_txt">v-if / v-show</span> 来进行显示隐藏，但如果判断条件繁琐且多个地方需要判断，这种方式的代码不仅不优雅而且冗余。针对这种情况，我们可以通过全局自定义指令来处理。</div>
<div class="font_min">1. 自定义一个权限数组</div>
<div class="font_min">2. 判断用户的权限是否在这个数组内，如果是则显示，否则就移除 DOM</div>

```bash
function inserted(el, binding, vnode) {
    const { value } = binding //获取到v-permission 的值
    const roles = store.getters && store.getters.roles && [localStorage.getItem("roleName")]//用户的权限
    if(value && value instanceof Array && value.length > 0) {
        const permissionRoles = value
        const hasPermission = roles.some(role => {
            return permissionRoles.includes(role)
        })
        if(!hasPermission) { 
            //没有权限 移除 DOM 元素
            el.parentNode && el.parentNode.removeChild(el)
        }
    } else {
        throw new Error(`需要的角色!像v-permission = "['超级管理员','管理员']"`)
    }
}

export default inserted
```

###### v-debounce (输入框防抖指令)
<div class="font_min">在开发中，有些提交保存按钮有时候会在短时间内被点击多次，这样就会多次重复请求后端接口，造成数据的混乱，比如新增表单的提交按钮，多次点击就会新增多条重复的数据。</div>
<div class="font_min">1. 定义一个延迟执行的方法，如果在延迟时间内再次触发该方法，则重新计算执行时间。</div>
<div class="font_min">2. 将事件绑定在 click 方法上。</div>

```bash
export const debounce = (el, binding) => {
    const { value } = binding
    //没绑定函数直接返回
    el._timer = null
    //监听点击事件，限定事件内如果再次点击则清空定时器并重新定时
    el.addEventListener('click', () => {
        if (el._timer !== null) {
            clearTimeout(el._timer)
            el._timer = null
        }
        el._timer = setTimeout(() => {
            value()
        }, 1000)
    })
}

export default debounce
```

##### Vue 的SSR是什么？有什么好处
* <div class="font_min">SSR就是服务端渲染</div>
* <div class="font_min">基于 nodejs serve 服务环境开发，所有 html 代码在服务端渲染</div>
* <div class="font_min">SSR 首次加载更快，有更好的用户体验，有更好的 seo 优化，因为爬虫能看到整个页面的内容</div>

##### Vue移动端 / PC端适配解决方案
<div class="font_min">插件：postcss-px-to-viewport</div>

```js
//权限路由这块主要是是使用router.beforeEach这钩子函数来实现，里面处理逻辑，判断用户是否登录，在请求接口获取用户当前的权限菜单，后台返回字符串，1代表显示，0代表隐藏，根据顺序来表示那个菜单显示和隐藏
//获取到后将本地的树结构的路由表扁平化,将两个数组合并，增加show类型来表示是否显示，利用递归和循环，每个路由都由srvName字段和show字段来决定路由表的hidden显示还是隐藏，最后交由addRoutes
//对于权限这一块采用的自定义指令来实现，登录的时候获取到数组权限，判断用户的权限是否在这个数组内，是就显示，没有的话就移除DOM
```
