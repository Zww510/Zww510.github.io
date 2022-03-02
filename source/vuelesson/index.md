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

##### >> 为什么 Data 是一个函数，且返回一个对象呢？
<div class="font_min">组件会被多次调用，如果 data 不是一个函数，组件之间的数据会出现数据污染</div>

##### >> Vue 的修饰符
<image src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5a1c911988f74cea91da79af3c6049c2~tplv-k3u1fbpfcp-watermark.awebp" />

##### >> Vue 的内部指令
<image src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d39d348e686b449e8931f5a85802e3c6~tplv-k3u1fbpfcp-watermark.awebp" />

##### >> v-if 和 v-show 有何区别？
* <div class="font_min">1. v-if 是通过控制 DOM 元素的删除和生成来实现删除和生成，类似 display: none</div>
* <div class="font_min">2. v-show 是通过控制 DOM 元素的 css 样式来实现隐藏和显示，不会销毁，类似 visibility: hidden</div>
* <div class="font_min">3. 频繁或者大数量显隐使用 v-show，否则使用 v-if</div>

##### >> 为什么 v-if 和 v-for 不建议用在同一标签？
<div class="font_min">1. v-for 的优先级大于 v-if，如果要遍历的数组很大，但展示的数据很少，会造成性能浪费。</div>
<div class="font_min">2. 建议使用 computed 过滤数据先</div>

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