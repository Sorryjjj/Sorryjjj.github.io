---
title: js运行原理
date: 2021-11-03 22:41:30
tags:
categories:
---

# 浏览器内核

浏览器内核分为两部分：渲染引擎；JS引擎

JS引擎逐渐独立，内核更倾向于渲染引擎

- Gecko：早期被NetScape和Firefox使用
- Trident：微软开发，IE浏览器
- Webkit：苹果基于KHTML开发、开源的，用于Safari，Google Chrome之前也在使用
- Blink：Google基于Webkit开发，用于Chrome、Edge、Opera

# JS引擎

将JavaScript代码翻译成机器代码

- SpiderMonkey：Brendan Eich开发，第一个js引擎
- Chakra：微软开发，IE
- JavascriptCore：Webkit
- V8：Google

内核：负责HTML解析、布局、渲染等等相关的工作

js引擎：解析、执行JavaScript代码

# V8引擎

- V8是用C ++编写的Google开源高性能JavaScript和WebAssembly引擎，它用于Chrome和Node.js等
- 它实现ECMAScript和WebAssembly，并在Windows 7或更高版本，macOS 10.12+和使用x64，IA-32， ARM或MIPS处理器的Linux系统上运行
- V8可以独立运行，也可以嵌入到任何C ++应用程序中

## 架构

源码：https://github.com/v8/v8

文档：https://v8.dev/

- Parse：由js代码生成AST（抽象语法树）
- Ignition：解释器，将AST转化为字节码ByteCode
  - 同时会收集TurboFan优化所需要的信息(比如函数参数的类型信息，有了类型才能进行真实的运算)
  - 如果函数只调用一次，Ignition会执行解释执行ByteCode
- TurboFan：编译器，可以将字节码编译为CPU可以直接执行的机器码
  - 如果一个函数被多次调用，那么就会被标记为热点函数，那么就会经过TurboFan转换成优化的机器码，提高代码的执行性能;
  - 但是，机器码实际上也会被还原为ByteCode，这是因为如果后续执行函数的过程中，类型发生了变化(比如sum函数原来执行的是 number类型，后来执行变成了string类型)，之前优化的机器码并不能正确的处理运算，就会逆向的转换成字节码

![](https://filescdn.proginn.com/3ab7eb5b3f88a24aeb98a19f43059261/fae32b4d579da90b03cc9ebe13c7a86a.webp)

## 执行细节

1. Blink将源码交给V8引擎，Stream获取到源码并且进行**编码转换**
2. Scanner会进行**词法分析**(lexical analysis)，词法分析会将代码转换成tokens
3. 接下来tokens会被转换成AST树，经过Parser和PreParser
   1. Parser就是直接将tokens转成AST树架构
   2. **PreParser**：
      1. 并不是所有的JavaScript代码，在一开始时就会被执行。那么对所有的JavaScript代码进行解析，必然会 影响网页的运行效率
      2. 所以V8引擎就实现了Lazy Parsing(延迟解析)的方案，它的作用是将不必要的函数进行预解析，也就是只解析暂 时需要的内容，而对函数的全量解析是在函数被调用时才会进行
4. 生成AST树后，会被Ignition转成字节码(bytecode)，之后的过程就是代码的执行过程

## 代码执行过程

1. 在执行代码之前，会在堆内存中创建一个全局对象Global Object（GO）
   1. 所有作用域都可以访问GO
   2. 包含一些全局方法和属性，如Date、Array、String、Number、setTimeout、setInterval
   3. 有一个window属性指向自己
2. 创建执行上下文栈（**Execution Context Stack**，ECS）：执行代码的调用栈
   1. 构建**Global Execution Context**（GEC），用于执行全局代码块
   2. GEC会被放入到ECS中执行（入栈）
      1. 在代码执行前，在parser转成AST的过程中，会将全局定义的变量、函数等加入到GlobalObject中，但是并不会赋值（作用域提升hoisting）
      2. 在代码执行中，对变量赋值，或者执行其他的函数
3. 
