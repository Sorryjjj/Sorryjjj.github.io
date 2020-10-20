---
title: Vue 语法-2
date: 2020-03-22 15:35:19
tags: vue
categories: Vue
---

# vue-resource

Vue 要实现异步加载需要使用到 vue-resource 库。

```javascript
<script src="https://cdn.staticfile.org/vue-resource/1.5.1/vue-resource.min.js"></script>
```

## Get

```javascript
this.$http.get(url).then(function (result) {
    console.log(result)
})
```

## Post

```javascript
this.$http.post(url,data:1}, {emulateJSON: true}).then(function (result) {
                    console.log(result.body)
 })
```

## JSONP

```javascript
this.$http.jsonp(url).then(result => {
                    console.log(result.body)
})
```

由于script标签不存在跨域限制，可以通过动态创建script标签，将script标签的src属性指向接口地址。

具体过程：

1. 在客户端定义回调方法，预定义对数据的草走
2. 将回调方法的名称，通过get方式传到接口
3. 接口将数据和回调方法名称拼接为字符串返回
4. 客户端拿到字符串当作script标签解析执行



# vue-router



```javascript
//页面中的容器
 <div id='app'>
     <router-view></router-view>
 </div>

//创建组件
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
//创建路由对象
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/',redirect: '/user' },
    { path: '/user/:id', component: User }
  ]
})

var vm = new Vue({
    el: '#app'
    router: router
})

```



# watch监视路由变化

```javascript
var vm = new Vue({
    el: '#app'
    router: router,
    watch: {
    	'$router.patch': unction (newVal,oldValue) {
    		console.log(newValue)
    	}
    }
})
```

