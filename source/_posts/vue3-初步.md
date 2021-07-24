---
title: vue3 初步
date: 2021-04-11 13:28:44
tags: [vue3,Composition API,响应式]
categories: [Vue3]
---

# Vue3

# 创建Vue3项目

- 使用vue-cli
- 使用vite

## vue-cli

版本在4.5以上

```
npm install -g @vue/cli
vue --version
vue create my-project
```

![image-20210411134133725](https://tva1.sinaimg.cn/large/008eGmZEly1gpfr33ygtqj31ft0u0wmm.jpg)

## vite

[vite文档](https://cn.vitejs.dev/guide/)

Vite (法语意为 "快速的"，发音 `/vit/`) 是一种新型前端构建工具，能够显著提升前端开发体验，包括

- 一个开发服务器，它基于 [原生 ES 模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) 提供了 [丰富的内建功能](https://cn.vitejs.dev/guide/features.html)，如速度快到惊人的 [模块热更新（HMR）](https://cn.vitejs.dev/guide/features.html#hot-module-replacement)。
- 一套构建指令，它使用 [Rollup](https://rollupjs.org/) 打包你的代码，并且它是预配置的，可以输出用于生产环境的优化过的静态资源。

Vite 意在提供开箱即用的配置，同时它的 [插件 API](https://cn.vitejs.dev/guide/api-plugin.html) 和 [JavaScript API](https://cn.vitejs.dev/guide/api-javascript.html) 带来了高度的可扩展性，并有完整的类型支持

```
npm init vite-app <project-name>
cd <project-name>
npm install
npm run dev
```

# Composition API

组合API

## setup

新的option，所有的组合API函数都在此使用，只在初始化时执行一次

函数如果返回对象，则可以直接在html模版中使用

## ref

- 定义一个数据的响应式
- 语法: const xxx = ref(initValue):
- 创建一个包含响应式数据的引用(reference)对象
  - js中操作数据: xxx.value
  - 模板中操作数据: 不需要.value
- 一般用来定义一个基本类型的响应式数据

```javascript
  setup () {
    const count = ref(1)
    function update () {
      count.value = count.value + 1
    }
    return {
      count,
      update
    }
  }
```

## reactive

- 作用: 定义多个数据的响应式
- const proxy = reactive(obj): 接收一个普通对象然后返回该普通对象的响应式代理器对象
- 响应式转换是“深层的”：会影响对象内部所有嵌套的属性
- 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据都是响应式的

```javascript
  setup () {
    /* 
    定义响应式数据对象
    */
    const state = reactive({
      name: 'tom',
      age: 25,
      wife: {
        name: 'marry',
        age: 22
      },
    })
    console.log(state, state.wife)

    const update = () => {
      state.name += '--'
      state.age += 1
      state.wife.name += '++'
      state.wife.age += 2
    }

    return {
      state,
      update,
    }
  }
```

# 响应式

- 通过Proxy(代理): 拦截对data任意属性的任意(13种)操作, 包括属性值的读写, 属性的添加, 属性的删除等...
- 通过 Reflect(反射): 动态对被代理对象的相应属性进行特定的操作

```javascript
const dinner = {
  meal: 'tacos'
}

const handler = {
  get(target, prop, receiver) {
    track(target, prop)
    return Reflect.get(...arguments)
  },
  set(target, key, value, receiver) {
    trigger(target, key)
    return Reflect.set(...arguments)
  }
}

const proxy = new Proxy(dinner, handler)
console.log(proxy.meal)
```

# setup

**执行时机：**

- 在beforeCreate之前执行(一次), 此时组件对象还没有创建
- this是undefined, 不能通过this来访问data/computed/methods / props
- 所有的composition API相关回调函数中也都不可以

**返回值：**

- 一般都返回一个对象: 为模板提供数据, 也就是模板中可以直接使用此对象中的所有属性/方法
- 返回对象中的属性会与data函数返回对象的属性合并成为组件对象的属性
- 返回对象中的方法会与methods中的方法合并成功组件对象的方法
- 如果有重名, setup优先

# 生命周期

在setup内调用

| 选项式 API        | Hook inside `setup` |
| ----------------- | ------------------- |
| `beforeCreate`    | Not needed*         |
| `created`         | Not needed*         |
| `beforeMount`     | `onBeforeMount`     |
| `mounted`         | `onMounted`         |
| `beforeUpdate`    | `onBeforeUpdate`    |
| `updated`         | `onUpdated`         |
| `beforeUnmount`   | `onBeforeUnmount`   |
| `unmounted`       | `onUnmounted`       |
| `errorCaptured`   | `onErrorCaptured`   |
| `renderTracked`   | `onRenderTracked`   |
| `renderTriggered` | `onRenderTriggered` |

# 自定义hook函数

- 使用Vue3的组合API封装的可复用的功能函数
- 自定义hook的作用类似于vue2中的mixin技术
- 自定义Hook的优势: 很清楚复用功能代码的来源, 更清楚易懂

## toRefs

把一个响应式对象转换成普通对象，该普通对象的每个 property 都是一个 ref

reactive 对象取出的所有属性值都是非响应式的

应用: 当从合成函数返回响应式对象时，toRefs 非常有用，这样消费组件就可以在不丢失响应式的情况下对返回的对象进行分解使用

```javascript
  setup () {
    const state = reactive({
      foo: 'a',
      bar: 'b',
    })
    const stateAsRefs = toRefs(state)
    return {
      // ...state,
      ...stateAsRefs,
    }
  }
```

