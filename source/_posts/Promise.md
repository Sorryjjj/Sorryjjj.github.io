---
title: Promise
date: 2020-03-20 09:52:30
tags: promise
categories: JavaScript
---

# Promise

## 概念

Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。

简单来说，Promise是一个保存异步操作的结果的容器。语法上，Promise是一个对象，获取异步操作的消息，提供统一的API。

## 特点

1. 对象状态不受外界影响，

   三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态

2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。

   相当于事件已经发生，无法再被改变。

## 用法

ES6 规定，`Promise`对象是一个构造函数，用来生成`Promise`实例。

```js
const promise = new Promise (function (resolve,reject) {
	if(success) {
		resolve()
	}
	else {
		reject();
	}
})
```

# 微任务

Promise的回调函数属于异步任务，会在同步任务之后执行

但是区别于一般的异步任务，Promise回调是微任务。一般任务追加到下一轮时间循环，微任务追加到本轮时间循环，因此，微任务的执行时间一定早于正常任务。

```js
setTimeout(function() {
  console.log(1);
}, 0);

new Promise(function (resolve, reject) {
  resolve(2);
}).then(console.log);

console.log(3);
// 3
// 2
// 1
```

//TODO
