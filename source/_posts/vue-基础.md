---
title: vue 基础
date: 2021-07-25 22:28:53
tags:
categories:
---

# 基础使用

## 自定义v-model

扩展v-model的功能

```vue
//child node
Vue.component('custom-input', {
  model: {
    prop: 'value',
    event: 'change'
  },
  props: {
    value: String
  },
  template: `
    <input  
     v-bind:value="value"  
     v-on:change="$emit('change', $event.target.value)" 
    >
})

//parent node
<template>
  <div>
	<custom-input v-model="inputValue"></custom-input>
  </div>
</template>

<script>
  export default {
    data() {
      inputValue: "hello"
    }
  }
</script>

```

## nextTick

vue是异步渲染

nextTick 是 Vue 提供的一个全局的API ，由于Vue的异步更新策略导致我们对数据的修改不会立马体现到都没变化上，此时如果想要立即获取更新后的dom的状态，就需要使用这个方法。

- vue 更新DOM是有策略的，不是同步更新
- data改变之后，DOM不会立刻渲染
- nextTick 可以接收一个函数做为入参
- nextTick 后能拿到最新的数据

## slot

Vue 实现了一套内容分发的 API，将`<slot>`元素作为承载分发内容的出口，这是vue文档上的说明。具体来说，slot就是可以让你在组件内添加内容的空间

具名插槽：根据slot的name属性进行对应的显示



```vue
//子组件 ： (假设名为：ebutton)
<template>
  <div class= 'button'>
      <button>  </button>
      <slot name= 'one'> 这就是默认值1</slot>
      <slot name='two'> 这就是默认值2 </slot>
      <slot name='three'> 这就是默认值3 </slot>
  </div>
</template>

//父组件：（引用子组件 ebutton）
<template>
  <div class= 'app'>
     <ebutton> 
        <template v-slot:one> 这是插入到one插槽的内容 </template>
        <template v-slot:two> 这是插入到two插槽的内容 </template>
        <template v-slot:three> 这是插入到three插槽的内容 </template>
     </ebutton>
  </div>
</template>
```

作用域插槽：父组件可以拿到子组件的data

```vue
//子组件 ： (假设名为：ebutton)
<template>
  <div class= 'button'>
      <button>  </button>
      <slot name= 'one' :value1='child1'> 这就是默认值1</slot>    //绑定child1的数据
      <slot :value2='child2'> 这就是默认值2 </slot>  //绑定child2的数据，这里我没有命名slot
  </div>           
</template>

new Vue({
  el:'.button',
  data:{
    child1:'数据1',
    child2:'数据2'
  }
})

//父组件：（引用子组件 ebutton）
<template>
  <div class= 'app'>
     <ebutton> 
        <template v-slot:one = 'slotone'>  
           {{ slotone.value1 }}    // 通过v-slot的语法 将子组件的value1值赋值给slotone 
        </template>
        <template v-slot:default = 'slottwo'> 
           {{ slottwo.value2 }}  // 同上，由于子组件没有给slot命名，默认值就为default
        </template>
     </ebutton>
  </div>
</template>
```

## 动态组件

:is="component-name"

适用于根据数据动态渲染组件，组件类型不确定

```vue
<component :is="currentTabComponent"></component>
```

每次切换时，都会重新创建对应的组件实例

因此，可以使用keep-alive缓存组件，频繁切换时不需要重新渲染

```vue
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

## 异步组件

注册组件时，使用import

在大型应用中，我们可能需要将应用分割成小一些的代码块，并且只在需要的时候才从服务器加载一个模块。为了简化，Vue 允许你以一个工厂函数的方式定义你的组件，这个工厂函数会异步解析你的组件定义。Vue 只有在这个组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染

```js
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})

```

## mixin

抽离公共逻辑

缺点：

- 变量来源不明确
- 多mixin 命名冲突
- mixin和组件多对多的关系，复杂度高

## vuex

## vue-router

hash模式，默认：路由在#之后

history模式：标准url

后者需要server端支持

动态路由：以冒号开头 '/uer/:id' 能命中'/user/10' 'user/20'等

路由配置懒加载：使用import

# Vue原理

## 组件化

MVVM：数据驱动视图

## 响应式

data数据改变，触发视图更新

Object.defineProperty，缺点需要递归，性能问题

proxy兼容性不好
