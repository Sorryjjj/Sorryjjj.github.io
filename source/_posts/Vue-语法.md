---
title: Vue 语法
date: 2020-03-20 21:11:38
tags: vue
categories: Vue
---

# 过滤器

过滤器是对即将显示的数据做进一步的筛选处理，然后进行显示

```javascript
//公有过滤器
new Vue({       
 filters: {      
    filterName: function (value1[,value2,...]) { 
       // 逻辑代码     
    } 
   }    
})

<div>{{数据属性名称 | 过滤器名称}}</div>

//私有过滤器，写在vue实例中
var vm = new Vue({
    filters: {
        filterName: function (value1[,value2,...]) {
			 // 逻辑代码   
        }
    }
})
```

过滤器调用遵循就近原则

# 按键修饰符

监听按键事件

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

```
<div @keyup.enter='func'></div>
```

可以通过全局 `config.keyCodes` 对象自定义按键修饰符

```
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

可用于input框回车搜索等

# 自定义全局指令

钩子函数：一个指令定义对象可以提供如下几个钩子函数

- bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
- inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
- updated：所在组件的 VNode 更新时调用。

和样式相关的可以写在bind里，和js操作相关的一般写在inserted里

- `el`：指令所绑定的元素，可以用来直接操作 DOM。
- `binding`：一个对象，包含以下属性：
  - `name`：指令名，不包括 `v-` 前缀。
  - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。

```javascript
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el,binding) {
    // 聚焦元素
    el.focus();
    
  }
})


Vue.directive('demo', {
  bind: function (el, binding) {
      console.log(binding.value);//获取指令传入的参数
  }
})
<div v-demo="value"></div>//
```

# 生命周期函数

![image.png](https://i.loli.net/2020/03/20/bvuW7zrVQLqelNf.png)

`beforeCreate`：在Vue实例完全创建之前有一个`beforeCreate`函数，此时，Vue中的data、methods数据都未初始化。

`created`:：data和methods已被初始化完成

`模板编译`：表示Vue开始编译模板，执行Vue中的指令，在内存中生成编译好的模板字符串，然后渲染到内存的DOM中，并没有将模板挂载到惠html中

`bedoreMount`：此时页面中的元素还没被替换，只是之前写的字符串模板

`mounted`：此时内存中的模板已被挂载到页面中，可以看到渲染好的页面





