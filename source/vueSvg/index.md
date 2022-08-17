---
title: Svg 在 Vue 项目中的使用方法及步骤
---

##### 1. 安装 svg-sprite-loader
<div class="font_min key_txt">npm install -D svg-sprite-loader</div>

##### 2. 配置 vue.config.js
```js
'use strict'
const path = require('path')
function resolve (dir) {
    return path.join(__dirname, dir)
}

module.exports = {
    chainWebpack (config) {
        const svgRule = config.module.rule('svg') //找到svg-loader
        svgRule.uses.clear() //清除已有的loader
        svgRule.exclude.add(/node_modules/) //正则匹配排除node_modules目录
        svgRule //添加svg新的loader处理
            .test(/\.svg$/)
            .use('svg-sprite-loader')
            .loader('svg-sprite-loader')
            .options({
                symbolId: 'icon-[name]'
            })
            .end()
    }
}
```

##### 3. componetns 中创建 SvgIcon 组件
<div><image src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1d1b1900ba1a416fbd4fb4064df66526~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp" /></div>

```js
<template>
    <svg :class="svgClass" aria-hidden="true">
        <use :xlink:href="iconName" />
    </svg>
</template>

<script>
export default {
    name: 'svg-icon',
    props: {
        iconClass: {
            type: String,
            required: true
        },
        className: {
            type: String
        }
    },
    computed: {
        iconName() {
            return `#icon-${this.iconClass}`
        },
        svgClass() {
            if(this.className) {
                return 'svg-icon' + this.classNmae
            } else {
                return 'svg-icon'
            }
        }
    }
}
</script>

<style scoped>
.svg-icon {
  width: 1.2em;
  height: 1.2em;
  vertical-align: -0.18em;
  fill: currentColor;
  overflow: hidden;
}
</style>
```

##### 4. 在 src 目录创建 icons 文件夹，svg 存放后缀为 .svg 的图片，index.js 配置如下
```js
import Vue from 'vue'
import SvgIcon from '@/components/SvgIcon' //导入svg组件

Vue.component('svg-icon', SvgIcon) //注册全局组件

const requireAll = requireContext => requireContext.keys().map(requireContext)
const req = require.context('./svg', false, /\.svg$/)
requireAll(req)
```

<image src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/962a23eca227496ebb0c833a2834d474~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp" />

##### 5. 在 main.js 中导入
<image src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/51713707b29d471f985334c5c3221ac0~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp" />

##### 6. 在组件中使用
```js
<template>
  <div id="app">
    <svg-icon icon-class="yebb" />
  </div>
</template>
```