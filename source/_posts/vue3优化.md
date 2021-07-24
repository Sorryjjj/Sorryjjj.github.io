---
title: vue3优化
date: 2021-04-19 01:09:59
tags: [vue3,dom diff]
categories: Vue3
---

# Vue3

## Options

vue2如果多个功能组合，只能使用mixin

导致：

- this是个黑盒，不知道里面的数据
- 数据来源不清晰

## Composition

用函数来组织代码，所有的变量，方法来源很清晰

思想来自于react hooks，但是实现不一样

setup只执行一次，之后由响应式来驱动

**script-setup**：

在编译运行时把代码放到了 setup 函数中运行，然后把导出的变量定义到上下文中，并包含在返回的对象中

```vue
<script setup lang="ts">
  // ...
</script>
```

```javascript
//useMouse.js
import {ref,onMounted,onUnmounted,computed} from 'vue'

export default function useMouse(){
  let x =ref(0)
  function update(e){
    x.value = e.pageX
  }
  onMounted(()=>{
    window.addEventListener('mousemove',update)
  })
  onUnmounted(()=>{
    window.removeEventListener('mousemove',update)
  })
  let count = computed(()=>x.value*2)
  return {x,count}
}
```

```vue
<template>
  <div>
    <h1>{{x}}:{{double}}</h1>
  </div>
</template>

<script setup lang="ts">
import {ref} from 'vue'
import useMouse from './useMouse'
let {x, count:double} = useMouse()
</script>
```

![image-20210419111554172](https://tva1.sinaimg.cn/large/008eGmZEgy1gpovu2uxf0j31480u0ti4.jpg)

# dom diff 

## 对比

- angular1脏检查
- vue1采用的是发布订阅模式，通过拦截操作，修改一个数据的同时收集依赖，然后数据修改的时候去通知更新 dom，没有vdom，不存在diff的问题
- react15引入了vdom，但是dom diff非常粗暴，递归，通过计算新老数据的 diff，去决定操作那些 dom，所以每次修改数据，需要生成一份新的数据
- vue2引入vdom ，组件之间用响应式，组件内部用vdom diff
- Vue3 通过静态标记 + 响应式 + 虚拟 dom 的方式，控制了 diff 的颗粒度，让 diff 的时间不会超过 16ms
- react里解决vdom的方式：既然dom diff会卡顿，dom diff从树换成链表， diff过程就可以中断了。如果有优先级高的任务来了，可以暂停diff

## 传统vdom的性能瓶颈

传统vdom的性能跟模版大小正相关，跟动态结点的数量无关，在一些组件整个模版内只有少量动态节点的情况下，造成性能浪费

## 关键点

vdom中新老数据都是数组，diff的算法会影响性能

- Vue2：参考了Snabbdom，双端对比（开头对比，结尾对比，开头与结尾，结尾与开头，四次预判）
- react：正常的遍历，优化了tree diff，component diff，element diff
- Vue3：借鉴了ivi算法和 inferno算法，双端对比 + 最长递增子序列

## 静态标记

在创建虚拟DOM树的时候，会根据DOM中的内容会不会发生变化，添加一个静态标记。那么之后在与上次虚拟节点进行对比的时候，就只会对比这些带有静态标记的节点

Vue3 中仅对静态标记标记对象进行比较（PatchFlag）

- 只比对带有 PF 的节点
- 并且通过 Flag 的信息得知当前节点要比对的具体内容
- vue2中无论元素是否参与更新，每次都会重新创建，然后在渲染
- vue3中对于不参与更新的元素，会做静态提升，只会被创建一次，在渲染时直接复用即可

```javascript
//静态标记枚举
export const enum PatchFlags {
  TEXT = 1,// 动态的文本节点
  CLASS = 1 << 1,  // 2 动态的 class
  STYLE = 1 << 2,  // 4 动态的 style
  PROPS = 1 << 3,  // 8 动态属性，不包括类名和样式
  FULL_PROPS = 1 << 4,  // 16 动态 key，当 key 变化时需要完整的 diff 算法做比较
  HYDRATE_EVENTS = 1 << 5,  // 32 表示带有事件监听器的节点
  STABLE_FRAGMENT = 1 << 6,   // 64 一个不会改变子节点顺序的 Fragment
  KEYED_FRAGMENT = 1 << 7, // 128 带有 key 属性的 Fragment
  UNKEYED_FRAGMENT = 1 << 8, // 256 子节点没有 key 的 Fragment
  NEED_PATCH = 1 << 9,   // 512
  DYNAMIC_SLOTS = 1 << 10,  // 动态 slot
  HOISTED = -1,  // 特殊标志是负整数表示永远不会用作 diff
  BAIL = -2 // 一个特殊的标志，指代差异算法
}
```

![image-20210419020139267](https://tva1.sinaimg.cn/large/008eGmZEly1gpoftc22pqj31an0u0ws2.jpg)

