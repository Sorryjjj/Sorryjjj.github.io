---
title: TypeScript
date: 2021-04-02 16:25:45
tags: TypeScript
categories: Vue3
---

# TypeScript

TypeScript是一种由微软开发的开源、跨平台的编程语言。它是JavaScript的超集，最终会被编译为JavaScript代码

特点：

- TypeScript 可以编译出纯净、 简洁的 JavaScript 代码，并且可以运行在任何浏览器上、Node.js 环境中和任何支持 ECMAScript 3（或更高版本）的JavaScript 引擎中
- 类型系统允许 JavaScript 开发者在开发 JavaScript 应用程序时使用高效的开发工具和常用操作比如静态检查和代码重构
- TypeScript 提供最新的和不断发展的 JavaScript 特性，包括那些来自 2015 年的 ECMAScript 和未来的提案中的特性，比如异步功能和 Decorators，以帮助建立健壮的组件

# 基础类型

## 布尔值

```typescript
let isDone: boolean = false
```

## 数字

支持二进制和八进制字面量

```typescript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

## 字符串

```typescript
let name: string = "bob";
```

## 数组

```typescript
let list: number[] = [1,2,3]
let list2: Array<number> = [1,2,3]
```

## 元组

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同

```typescript
let x: [string, number];
x = ['hello', 10]; // OK
x = [10, 'hello']; // Error
```

## 枚举

`enum`类型是对JavaScript标准数据类型的一个补充，默认情况下，从`0`开始为元素编号。 你也可以手动的指定成员的数值

```typescript
enum Color {Red,Green,Blue}
//enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];
let c: Color = Color.Green;
```

## Any

不进行类型检查

```typescript
let notSure: any = 4;
let list: any[] = [1, true, "free"];
```

## Void

表示没有任何类型，当一个函数没有返回值时，一般为void

**void类型变量只能为undefined和null**

```typescript
function warnUser(): void {
    console.log("This is my warning message");
}
let unusable: void = undefined;
```

## Null Undefined

TypeScript里，`undefined`和`null`两者各自有自己的类型分别叫做`undefined`和`null`

默认情况下**`null`和`undefined`是所有类型的子类型**。 就是说可以把 `null`和`undefined`赋值给`number`类型的变量。

## Never

表示的是那些永不存在的值的类型

`never`类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型

## Object

表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型

## 类型断言

类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用

# 变量声明

# 接口

```typescript
interface Person {
  firstName: String
  lastName: String
  readonly x: number
}
```

readonly：属性名前用`readonly`来指定只读属性

TypeScript具有`ReadonlyArray<T>`类型，无法修改数组

# 类

```typescript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
```

# 函数

```typescript
function add(x: number, y: number): number {
  return x + y
}

let myAdd = function(x: number, y: number): number { 
  return x + y
}
```

# 泛型

## 函数泛型

指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定具体类型的一种特性。

```typescript
function createArray2 <T> (value: T, count: number) {
  const arr: Array<T> = []
  for (let index = 0; index < count; index++) {
    arr.push(value)
  }
  return arr
}
```

## 泛型接口

在定义接口时, 为接口中的属性或方法定义泛型类型，在使用接口时, 再指定具体的泛型类型

```typescript
interface IbaseCRUD <T> {
  data: T[]
  add: (t: T) => void
  getById: (id: number) => T
}

```

## 泛型类

在定义类时, 为类中的属性或方法定义泛型类型，在创建类的实例时, 再指定特定的泛型类型

```typescript
class GenericNumber<T> {
  zeroValue: T
  add: (x: T, y: T) => T
}
```

