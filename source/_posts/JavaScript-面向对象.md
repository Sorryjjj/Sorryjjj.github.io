---
title: JavaScript 面向对象
date: 2020-03-19 13:47:35
tags: OOP
categories: JavaScript
---

# 继承

JavaScript 的继承是通过“原型对象”（prototype）实现，ES6中引入class，可以通过类实现。

## 构造函数的继承

分为两个步骤

1. 在子类中的构造函数调用父类的构造函数。
2. 让子类的原型指向父类的原型

```js
function Father (name) {
	this.name = name;
}
Father.prototype.func1 = function () {
	console.log('func1')
}
//第一步
function Son (name) {
	Father.call(this);
	this.name = name;
}
//另外一种写法
//function Son (name) {
	//this.base = Father;
	//this.base();
//}

//第二步
Son.prototype = Object.create(Father.prototype);
Son.prototype.constructor = Father;
var son = new Son('小明');
son.func1();

```

## 多重继承

Javascript不提供多重继承，即不允许一个对象同时继承多个对象。但是，可以通过变通方法，实现这个功能。 这种模式称为Mixin。

 ```js
function M1() {
  this.hello = 'hello';
}

function M2() {
  this.world = 'world';
}

function S() {
  M1.call(this);
  M2.call(this);
}

// 继承 M1
S.prototype = Object.create(M1.prototype);
// 继承链上加入 M2
Object.assign(S.prototype, M2.prototype);

// 指定构造函数
S.prototype.constructor = S;

var s = new S();
s.hello // 'hello'
s.world // 'world'
 ```

# 模块

## 基本写法

模块是实现特定功能的一组属性和方法的封装。简单的做法是把模块写成一个对象，所有的模块成员都放到这个对象里面。

```js
var module = new Object({
	value: 0,
	f1: function () {},
	f2: function () {},
})
```

这样写的缺点：暴露模块成员，内部状态可以被外部改写。

## 立即执行函数（IIFE）

## 概念

声明函数的同时立即调用这个函数。

IIFE的出现是为了弥补JS在scope方面的缺陷：JS只有全局作用域、函数作用域，从ES6开始才有块级作用域。

ES5在没有块级作用域下的缺点

- 在if或者for循环中声明的变量会泄露成全局变量
- 内层变量可能会覆盖外层变量

## 通过IIFE封装私有变量

将相关的属性和方法封装在一个函数作用域里面，可以达到不暴露私有成员的目的。

```js
var module = (function () {
	var value = 0;
	var f1 = function () {};
	var f2 = function () {};
	return {
		f1: f1,
		f2: f2
	}
})();
console.log(module.value);//undefined
```

由于函数只返回了f1、f2，所以外部无法读取value。

当需要继承多个模块时，需要采用宽放大模式。

```javascript
var module = (function (mod) {
	mod.f3 = function () {};
	return mod;
})(window.module || {});//无法知道哪个部分会先加载,可能会加载空对象

```

# ES6中的class

TODO