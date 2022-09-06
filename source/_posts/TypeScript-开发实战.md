---
title: TypeScript 开发实战
date: 2022-07-04 22:03:35
tags: TypeScript
categories: TypeScript开发实战
---

# 静态类型语言与动态类型语言

静态类型语言：在编译阶段确定所有变量的类型

动态类型语言：在执行阶段确定所有变量的类型

## JavaScript与C++对比

```javascript
class C {
	consttructor(x,y) {
		this.x = x
		this.y = y
	}
}

function add(a,b) {
  return a.x + a.y + b.x + b.y
}
```

```c++
class C {
	public:
		int x;
		int y;
}
int add(C a, C b) {
	return a.x + a.y + b.x + b.y;
}
```

内存分配

JavaScript：

- 在程序运行时，动态计算属性偏移量
- 需要额外的空间存储属性名
- 所有对象的偏移量信息各存一份

C++：

- 编译阶段确定属性偏移量
- 用偏移量访问代替属性名访问
- 偏移量信息共享

| 静态类型语言   | 动态类型语言            |
| -------------- | ----------------------- |
| 对类型极度严格 | 对类型非常宽松          |
| 立即发现错误   | Bug可能隐藏数月甚至数年 |
| 运行时性能好   | 运行时性能差            |
| 自文档化       | 可读性差                |

动态类型语言的支持者认为：

- 性能是可以改善的（V8引擎），而语言的灵活性更重要
- 隐藏的错误可以通过单元测试发现
- 文档可以通过工具生成

## 其他定义

强类型语言：不允许程序在发生错误后继续执行

# typescript数据类型

```typescript
// 原始类型
let bool: boolean = true;
let num: number = 123;
let str: string = "abc";

// 数组
let arr1: number[] = [1, 2, 3];
let arr2: Array<number> = [1, 2, 3];

// 元组: 用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同
let tuple: [number, string] = [0, "1"];
// 允许push，但是不允许访问
// tuple.push(2)
// tuple[2]

// 函数
let add = (x: number, y: number): number => x + y;

// 对象
let obj: { x: number; y: number } = { x: 1, y: 2 };

// symbol: 新的原始数据类型 Symbol ，表示独一无二的值
let s1: symbol = Symbol();
let s2 = Symbol();
console.log(s1 === s2); // false

// undefined null
let un: undefined = undefined;
let nu: null = null;

// void
let noReturn = () => {};

// any
let x;
x = 1;
x = [];
x = () => {};

// never
// 一个总是会抛出错误的函数
let error = () => {
  throw new Error("error");
};
// 一个从来不会有返回值的函数
let endless = () => {
  while (true) {}
};

```

# 枚举类型

```typescript
// 数组枚举
enum Role {
  Reporter,
  Developer,
  Maintainer,
  Owner,
  Guest,
}
console.log(Role[1]);
console.log(Role.Developer);

// 生成的代码中，枚举类型被编译成一个对象，它包含了正向映射（ name -> value）和反向映射（ value -> name）。
// 引用枚举成员总会生成为对属性访问并且永远也不会内联代码。
// 要注意的是 不会为字符串枚举成员生成反向映

// var Role;
// (function (Role) {
//     Role[Role["Reporter"] = 0] = "Reporter";
//     Role[Role["Developer"] = 1] = "Developer";
//     Role[Role["Maintainer"] = 2] = "Maintainer";
//     Role[Role["Owner"] = 3] = "Owner";
//     Role[Role["Guest"] = 4] = "Guest";
// })(Role || (Role = {}));

// 字符串枚举
enum Message {
  Success = "成功",
  Fail = "失败",
}

// var Message;
// (function (Message) {
//     Message["Success"] = "\u6210\u529F";
//     Message["Fail"] = "\u5931\u8D25";
// })(Message || (Message = {}));

// 异构枚举
enum Answer {
  N,
  Y = "Yes",
}

// 枚举成员
enum Char {
  // const，在编译时计算出结果，以常量形式出现在运行环境
  a,
  b = Char.a,
  c = 1 + 2,
  //   computed，非常量表达式，保留到程序执行阶段
  d = Math.random(),
  e = "123".length,
}
// "use strict";
// var Char;
// (function (Char) {
//     Char[Char["a"] = 0] = "a";
//     Char[Char["b"] = 0] = "b";
//     Char[Char["c"] = 3] = "c";
//     Char[Char["d"] = Math.random()] = "d";
//     Char[Char["e"] = "123".length] = "e";
// })(Char || (Char = {}));

// 常量枚举: 当访问枚举值时，为了避免生成多余的代码和间接引用，可以使用常数枚举。
const enum Month {
  Jan,
  Feb,
  Mar,
}
let month = [Month.Jan, Month.Feb, Month.Mar];

// "use strict";
// let month = [0 /* Month.Jan */, 1 /* Month.Feb */, 2 /* Month.Mar */];

// 枚举类型
enum E {
  a,
  b,
}
enum F {
  a = 0,
  b = 1,
}
enum G {
  a = "apple",
  b = "banana",
}

// 把任意number类型赋值给枚举类型，取值也可以超出枚举成员的定义
let e: E = 3;
let f: F = 3;
// 两种不同类型的枚举无法比较
// console.log(e === f); // 此条件将始终返回 "false"，因为类型 "E" 和 "F" 没有重叠

// 两种不同类型的枚举无法比较
let e1: E.a = 2;
let e2: E.b;
// console.log(e1 === e2); 无法比较

let e3: E.a = 1;
// console.log(e1 === e3);

// 取值只能是枚举成员的类型
let g1: G = G.a;
let g2: G.a = G.a;
```



# 接口

```typescript
interface List {
  readonly id: number; // 只读属性，不允许修改
  name: string;
  age?: number; // 可选属性
}

interface Result {
  data: List[];
}

function render(result: Result) {
  result.data.forEach((value) => console.log(value.id, value.name));
}

let result = {
  data: [
    { id: 1, name: "A", sex: "male" },
    { id: 2, name: "B" },
  ],
};

// 传入对象满足约束条件即可，即使多了sex属性
render(result);
// 若传入对象字面量的话,ts就会对额外的字段进行检查
// 不能将类型“{ id: number; name: string; sex: string; }”分配给类型“List”。
//   对象文字可以只指定已知属性，并且“sex”不在类型“List”中
// render({
//   data: [
//     { id: 1, name: "A", sex: "male" },
//     { id: 2, name: "B" },
//   ],
// });

// 解决方法： 类型断言 或者 字符串索引签名
// 类型断言
render({
  data: [
    { id: 1, name: "A", sex: "male" },
    { id: 2, name: "B" },
  ],
} as Result);

render(<Result>{
  data: [
    { id: 1, name: "A", sex: "male" },
    { id: 2, name: "B" },
  ],
});

// 字符串索引签名
// interface List {
//   id: number;
//   name: string;
//   [x: string]: any;
// }

// 数字索引接口
interface StringArray {
  [index: number]: string;
}
let chars: StringArray = ["A", "B"];

// 字符串索引接口
interface Names {
  [x: string]: string;
  //   y: number // 不允许
  //   [z: number]: number; // 不允许
  [z: number]: string;
}
```

## 函数接口

```typescript
// interface Add {
//   (x: number, y: number): number;
// }

// 类型别名
type Add = (x: number, y: number) => number;

let add: Add = (a, b) => a + b;

// 混合类型: 一个对象可以同时做为函数和对象使用，并带有额外的属性
interface Lib {
  (): void;
  version: string;
  doSomething(): void;
}

function getLib() {
  let lib: Lib = (() => {}) as Lib;
  lib.version = "1.0";
  lib.doSomething = () => {};
  return lib;
}

```

## 函数重载

```typescript
function add3(...rest: number[]): number;
function add3(...rest: string[]): string;
function add3(...rest: any[]): any {
  let first = rest[0];
  if (typeof first === "string") {
    return rest.join();
  }
  if (typeof first === "number") {
    return rest.reduce((pre, cur) => pre + cur);
  }
}
console.log(add3(1, 2, 3));
console.log(add3("a", "b", "c"));
```

# 类

```typescript
class Dog {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  run() {}
  private pri() {} // 只能在类中访问
  protected pro() {} // 只能在类或者子类种调用，不能被实例调用
  readonly legs: number = 4; // 只读属性必须被初始化
  static food: string = "bones"; // 只能通过类名调用
}
console.log(Dog.prototype); // {run: ƒ, constructor: ƒ} name属性只在实例上，不再原型上

let dog = new Dog("wangwang");
// dog.pri() 属性“pri”为私有属性，只能在类“Dog”中访问
console.log(dog);

// 继承
class Husky extends Dog {
  //   color: string;
  // 构造函数参数可以添加修饰符，自动变成实例的属性，省略在类中的定义，public color
  constructor(name: string, public color: string) {
    super(name); // 派生类的构造函数必须包含 "super" 调用
    this.color = color;
  }
}

```

# 抽象类与多态

抽象类：只能被继承，不能被实例化的类

```typescript
abstract class Animal {
  eat() {
    console.log("eat");
  }
  // 抽象方法
  abstract sleep(): void;
}
// let animal = new Animal() 无法创建抽象类的实例
class Dog1 extends Animal {
  name: string;
  constructor(name: string) {
    super();
    this.name = name;
  }
  run() {}
  sleep(): void {
    console.log("dog sleep");
  }
}

let dog1 = new Dog1("wangwang");
dog1.eat();

// 多态
class Cat extends Animal {
  sleep(): void {
    console.log("Cat sleep");
  }
}

let cat = new Cat();

let animals: Animal[] = [dog1, cat];
animals.forEach((item) => {
  item.sleep();
});

// 链式调用
class WorkFlow {
  step1() {
    return this;
  }
  step2() {
    return this;
  }
}
let workflow = new WorkFlow();
workflow.step1().step2();

// 子类的链式调用
class MyFlow extends WorkFlow {
  next() {
    return this;
  }
}
let myflow = new MyFlow();
myflow.next().step1().step2();

```

# 类与接口

- 接口之间可以相互继承
- 类之间可以继承
- 接口可以通过类实现，接口只能约束类的公有成员
- 接口可以抽离出类的成员（public,private,protected）

```typescript
interface Human {
  //   new (name: string): void; 会提示错误实现了接口
  name: string;
  eat(): void;
}

// 类实现接口，必须实现接口的所有属性
// 接口只能约束类的公有成员，不能约束构造函数
class Asian implements Human {
  name: string;
  //   private name: string; 不可以

  constructor(name: string) {
    this.name = name;
  }
  eat() {}
  sleep() {}
}

// 接口继承
interface Man extends Human {
  run(): void;
}

interface Child {
  cry(): void;
}

interface Boy extends Man, Child {}

// 需要实现Human Man Child的全部属性: run, name, eat, cry
let boy: Boy = {
  name: "",
  run() {},
  eat() {},
  cry() {},
};

// 接口继承类
class Auto {
  state = 1;
  //   private state2 = 0;
}

interface AutoInterface extends Auto {
  // 已经包含了state属性
}

class C implements AutoInterface {
  state = 2;
}
class Bus extends Auto implements AutoInterface {
  // 不必实现state，已经继承了state
}

```

# 泛型

- 函数和类可以轻松地支持多种类型，增强程序的扩展性
- 不必写多条函数重载，冗长的联合声明，增强代码可读性
- 灵活控制类型之间的约束

```typescript
function log<T>(value: T): T {
  console.log(value);
  return value;
}

log<string[]>(["a", "b"]);
log(["a", "b"]);

// 泛型函数类型
type Log = <T>(value: T) => T;
// 泛型函数实现
let myLog: Log = log;

// 接口定义泛型函数
interface Log1<T = string> {
  (value: T): T;
}
// 泛型约束了整个接口时，实现必须指定类型
let myLog1: Log1<number> = log;
myLog1(1);
// myLog1('a'); 必须为number

```

## 泛型与类

```typescript
class Log2<T> {
  run(value: T) {
    console.log(value);
    return value;
  }
}

// 指定类型参数时，参数必须符合类型
let log1 = new Log2<number>();
log1.run(1);

// 当不指定类型参数时，可以传任意类型
let log2 = new Log2();
log2.run("a");

// 泛型约束
interface Length {
  length: number;
}
function log3<T extends Length>(value: T): T {
  console.log(value, value.length);
  return value;
}

log3([]);
log3("111");
log3({ length: 1 });

```

# 类型检查机制

Typescript编译器在做类型检查时，所秉承的一些原则，以及表现出的一些行为

辅助开发，提高开发效率

- 类型推断
- 类型兼容性
- 类型保护

## 类型推断

不需要指定变量的类型（函数的返回值类型），Typescript可以根据某些规则自动推断出一个类型

- 基础类型推断
- 最佳通用类型推断
- 上下文类型推断

```typescript
let a = 1; // number
let b = [1, null]; // (number | null)[]

let c = (x = 1) => x + 1; // x: number, c+ number

window.onkeydown = (event) => {
  // event: KeyboardEvent
  console.log(event);
};

// 类型断言
interface Foo {
  bar: number;
}
let foo = {} as Foo;
foo.bar = 1;

```

## 类型兼容性

当一个类型Y可以被赋值给另一个类型X时，则类型X兼容类型Y

```typescript
// 类型兼容性
let s: string = "a";
a = null;

// 接口兼容性
interface X {
  a: any;
  b: any;
}
interface Y {
  a: any;
  b: any;
  c: any;
}

let x1: X = { a: 1, b: 2 };
let y1: Y = { a: 1, b: 2, c: 1 };

x1 = y1;
// y1 = x1; 无法赋值

// 函数兼容性
type Handler = (a: number, b: number) => void;
function hof(handler: Handler) {
  return handler;
}

// 需要同时满足三个条件：参数个数、参数类型、返回值类型

// 1 参数个数
let handler1 = (a: number) => {};
hof(handler1);
let handler2 = (a: number, b: number, c: number) => {};
// hof(handler2) 错误

// 可选参数和剩余参数
let a1 = (p1: number, p2: number) => {};
let b1 = (p1?: number, p2?: number) => {};
let c1 = (...agrs: number[]) => {};

// 固定参数兼容可选参数和剩余参数
a1 = b1;
a1 = c1;

// 可选参数 不兼容固定参数和剩余参数???
b1 = c1;
b1 = a1;

//剩余参数 ，可以兼容固定参数和可选参数
c1 = a1;
c1 = b1;

// 2 参数类型
let handler3 = (a: string) => {};
// hof(handler3)

interface Point3D {
  x: number;
  y: number;
  z: number;
}

interface Point2D {
  x: number;
  y: number;
}

let p3d = (point: Point3D) => {};
let p2d = (point: Point2D) => {};
p3d = p2d;
// p2d = p3d // 不兼容，类似于参数多的兼容参数少的，关闭strictFunctionTypes可以兼容

// 3 返回值类型
let f1 = () => () => ({ name: "Alice" });
let f2 = () => () => ({ name: "Alice", location: "Beijing" });
f1 = f2;
// f2 = f1 // 不兼容

// 函数重载，重载列表与函数实现
// 运行时编译器会查找重载列表，使用第一个匹配的定义来执行函数
function overload(a: number, b: number): number;
function overload(a: string, b: string): string;
function overload(a: any, b: any): any {}
// function overload(a: any, b: any, c: any): any {} // 不兼容

// 枚举兼容性
enum Fruit {
  Apple,
  Banana,
}
enum Color {
  Red,
  Yellow,
}

let fruit: Fruit.Apple = 3;
let no: number = Fruit.Apple;
// let color: Color.Red = Fruit.Apple //枚举之间不兼容

// 类兼容性
class A {
  constructor(p: number, q: number) {}
  id: number = 1;
  //   private name: string = ''
}
class B {
  static s = 1;
  constructor(p: number) {}
  id: number = 2;
  //   private name: string = ''
}

let aa = new A(1, 2);
let bb = new B(1);
// 两个实例互相兼容，静态成员不做比较
aa = bb;
bb = aa;
// 若A中有私有成员,则不兼容
// 而父类与子类是兼容的
class C1 extends A {}
let cc = new C1(1, 2);
aa = cc;
cc = aa;

// 泛型兼容性
interface Empty<T> {
  // value: T
}
let obj1: Empty<number> = {};
let obj2: Empty<string> = {};
// obj1 = obj2; // 泛型接口中没有成员时兼容

// 两个泛型函数定义相同，但是没有指定类型参数，则兼容
let log4 = <T>(x: T): T => {
  console.log("x");
  return x;
};
let log5 = <U>(y: U): U => {
  console.log("y");
  return y;
};

log4 = log5; // 兼容

```

## 类型保护

```typescript
// 编辑器：补全，自动提示
//
// ts类型保护机制：
// 类型推断，兼容性，保护
//
// 创建类型保护区块
//
enum Type {
  Strong,
  Week,
}

class Java {
  helloJava() {
    console.log("Hello Java");
  }
  java: any;
}

class JavaScript {
  helloJavaScript() {
    console.log("Hello JavaScript");
  }
  js: any;
}

// 类型保护区块
// lang is Java 类型谓词 表明返回值为true的话，lang是java类型
function isJava(lang: Java | JavaScript): lang is Java {
  return (lang as Java).helloJava !== undefined;
}

function getLanguage(type: Type, x: string | number) {
  let lang = type === Type.Strong ? new Java() : new JavaScript();

  // 类型保护区块
  if (isJava(lang)) {
    lang.helloJava();
  } else {
    lang.helloJavaScript();
  }

  // if ((lang as Java).helloJava) {
  //     (lang as Java).helloJava();
  // } else {
  //     (lang as JavaScript).helloJavaScript();
  // }

  // instanceof 实例
  // if (lang instanceof Java) {
  //     lang.helloJava()
  //     // lang.helloJavaScript()
  // } else {
  //     lang.helloJavaScript()
  // }

  // in 对象中是有包含某个属性
  // if ('java' in lang) {
  //     lang.helloJava()
  // } else {
  //     lang.helloJavaScript()
  // }

  // typeof 判断基本的类型
  // if (typeof x === 'string') {
  //     console.log(x.length)
  // } else {
  //     console.log(x.toFixed(2))
  // }

  return lang;
}

getLanguage(Type.Week, 1);

```

# 高级类型

## 交叉类型 & 联合类型

```typescript
// 高阶类型：为了灵活性而引入的一些特性
// 复杂多变的开发环境

interface DogInterface {
  run(): void;
}
interface CatInterface {
  jump(): void;
}
// 交叉类型 &，混入
let pet: DogInterface & CatInterface = {
  run() {},
  jump() {},
};

// 联合类型 ｜ ：声明的类型可以为多个类型中的一个
// 不确定性，灵活性

let aaa: number | string = 1;
let bbb: "a" | "b" | "c"; // 字面量类型，类型和取值
let ccc: 1 | 2 | 3;

class Dog2 implements DogInterface {
  run() {}
  eat() {}
}
class Cat1 implements CatInterface {
  jump() {}
  eat() {}
}
enum Master {
  Boy,
  Girl,
}
function getPet(master: Master) {
  let pet = master === Master.Boy ? new Dog2() : new Cat1();
  // 只能访问公共成员
  // pet.run()
  // pet.jump()
  pet.eat();
  return pet;
}

// 可区分的联合类型
// 联合类型 + 字面量类型
interface Square {
  kind: "square";
  size: number;
}
interface Rectangle {
  kind: "rectangle";
  width: number;
  height: number;
}
interface Circle {
  kind: "circle";
  radius: number;
}
type Shape = Square | Rectangle | Circle;
function area(s: Shape) {
  switch (s.kind) {
    case "square":
      return s.size * s.size; // 使用 return 就不用加 break 了
    case "rectangle":
      return s.height * s.width;
    case "circle":
      return Math.PI * s.radius ** 2;
    default:
      // never
      // 上面的分支都不匹配
      return ((e: never) => {
        throw new Error(e);
      })(s);
  }
}
console.log(area({ kind: "circle", radius: 1 }));

```

## 索引类型

```typescript
// ts 中的索引类型
let obj12 = {
  a: 1,
  b: 2,
  c: 3,
};

// 如果keys是不存在的属性，也不会报错
// function getValues(obj: any, keys: string[]) {
//     return keys.map(key => obj[key])
// }

// 索引查询操作符
// keyof T ，表示类型T的所有公共属性字面量的联合类型
function getValues<T, K extends keyof T>(obj: T, keys: K[]): T[K][] {
  return keys.map((key) => obj[key]);
}
console.log(getValues(obj12, ["a", "b"]));
// console.log(getValues(obj, ['d', 'e']))

interface Obj {
  a: number;
  b: string;
}

// 查询操作符
// keyof T
let key: keyof Obj;
// let key: "a" | "b";

// 索引访问操作符
// T[K]
let value: Obj["a"];

// 泛型约束
// T extends U

```

## 条件类型

```typescript
// 条件类型
// 由条件表达式决定的类型
//
// T extends U ? X : Y

// T extends string 泛型 T 约束
//
type TypeName<T> = T extends string
  ? "string"
  : T extends number
  ? "number"
  : T extends boolean
  ? "boolean"
  : T extends undefined
  ? "undefined"
  : T extends Function
  ? "function"
  : "object"; 
type T1 = TypeName<string>;
type T2 = TypeName<string[]>;

// 分布式的条件类型
// (A | B) extends U ? X : Y
// (A extends U ? X : Y) | (B extends U ? X : Y)
type T3 = TypeName<string | string[]>;

// 类型的过滤
// T extends U 的含义，T可以被赋值给U
//
type Diff<T, U> = T extends U ? never : T;
// 过滤掉第2个类型中含有的a
type T4 = Diff<"a" | "b" | "c", "a" | "e">;
// Diff<"a", "a" | "e"> | Diff<"b", "a" | "e"> | Diff<"c", "a" | "e">
// never | "b" | "c"
// "b" | "c"

// 从类型中过滤掉 null 和 undefined
type NotNull<T> = Diff<T, null | undefined>;
type T5 = NotNull<string | number | undefined | null>;

// 官方内置的条件类型
// Exclude<T, U>
// NonNullable<T>

// Extract<T, U>
type T6 = Extract<"a" | "b" | "c", "a" | "e">;

/**
 * Obtain the return type of a function type
 */
// ReturnType<T>
type T8 = ReturnType<() => string>;

// type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;
// infer 表示代推断或者延迟推断


```

