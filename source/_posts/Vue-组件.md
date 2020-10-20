---
title: Vue 组件
date: 2020-03-22 16:15:41
tags: vue
categories: Vue
---

# 概念

组件是可复用的Vue实例，可以扩展 HTML 元素，封装可重用的代码，通过不同的组件划分不同的功能模块

- 模块化：从代码逻辑划分，方便分层开发，保证每个功能模块的职能单一
- 组件化：从UI界面划分，方便复用

# 定义组件

## 全局组件

### vue.extend

```javascript
var com = Vue.extend({
	template: '<p>hahahah</p>', // 指定组件的html结构
})
Vue.component('mycom',com)

//在html中以html标签引入,若以驼峰形式命名，则引用时改为小写，使用-连接
<mycom></mycom>

```

### vue.component

```
Vue.component('mycom',{
	template: '<p>hahahah</p>'
})
```

### #

```javascript
//在vue实例外使用template元素，定义html模板结构
<template id='tmp'>
	<p>hahahah</p>
</template>


Vue.component('mycom',{
	template: '#tmp'
})
```

## 私有组件

```javascript
var Child = {
  template: '<div>A custom component!</div>'
}

new Vue({
  // ...
  components: {
    // <my-component> 将只在父模板可用
    'my-component': Child
  }
})
```

# data

组件可以有自己的data数据，组件中的data必须是一个方法，且返回一个对象,用法与vue实例中的data一致。

将data定义为function是为了变量私有化，组件复用时不会互相影响，保证数据的独立性。

```javascript
Vue.component('mycom',{
	template: '<p>hahahah {{msg}}</p>',
	data: function () {
		return {
            msg: 'wwuwu'
        }
	}
})
```

# 组件切换

## v-if

## is属性

```javascript
//is属性，用来指定要展示的组件id
//组件在 vm.componentId 变化时改变！
<component :is='componentId'></component>
```



# 组件间传值

```javascript
 <div id='app'>
     <com1 v-bind: parentMsg='msg' @func='getMsgFromSon'></com1>
 </div>

 <template id="'tmp1'">
     <div>
         <h1>子元素 --- {{parentMsg}}</h1>
     </div>
 </template>

 <script>
     var com1 = {
         template: '#tmp1',
         data () {
 			return {
                msg: 'childMsg'
            }            
         },
         props: ['parentMsg'],
         methods: {
             sendMsg(){
                 this.$emit('func',this.msg)
             }
         }
     }
     var vm = new Vue({
         el: '#app',
         data: {
			msg: 'hahha',
             childMsg: 'xixiixi'
         },
         methods: {
			getMsgFromSon(data) {
                this.childMsg = data;
            }
         },
         components: {
             com1
         }
     })
 </script>
```















