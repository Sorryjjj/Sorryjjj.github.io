---
title: MVC MVVM
date: 2020-03-19 21:32:46
tags: Vue
categories: JavaScript
---

# MVC

MVC是后端分层开发思想

- M层: model 数据模型层(处理数据的增删改查) 提供数据

- V层: Views 视图层 (数据展示) 渲染数据

- C层: controller 控制层(处理业务逻辑) 调用数据渲染视图

# MVVM

MVVM是前端视图层的开发思想，主要关注于视图层，将每个页面分成了M、V、VM

- M：Model，保存每个页面中单独的数据。
- V：View，每个页面的html结构。
- VM：view Model，调度者，分割M和V，每当V层想要获取保存后的数据，都由VM做中间处理。在vue中指的是vue实例化对象，提供数据双向绑定。



```

//html,View
<div id="app">
	<p>{{msg}}</p>
</div>

//js,VM
var vm = new Vue({
	el: 'app',
	data: {
		msg: 'hahaha'
	}
})
```

