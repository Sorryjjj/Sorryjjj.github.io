---
title: Javascript中的原型
date: 2020-03-18 21:53:00
tags: 原型
categories: JavaScript
---

# 对象

JavaScript 中的所有事物都是对象：字符串、数字、数组、日期，等等。在 JavaScript 中，对象是拥有属性和方法的数据。

# 构造函数

用new初始化对象（一般首字母大写）

1. 在内存中创建一个新的空对象

2. 让this指向新的对象

3. 执行构造函数里的代码（添加属性和方法）

4. 返回这个新对象（不需要return）

   ```js
   function Person (name,age) {
   	this.name = name;
   	this.age = age;//通过this添加的实例成员，可以通过this访问
       this.action = function(){
           console.log('action')
       }
   }
   var person = new Person('Xiaoming',18);
   Person.sex = 'female';//sex为静态成员，只能通过构造函数访问
   var person2 = new Person('Xiaohong',18);
   console.log(person2.action === person.action) //false,因为每实例化一个对象,都需要新开辟内存存放action方法，两者地址不同
   ```

   通过原型可以共享函数

# 原型

## prototype

每一个函数都有```prototype```属性，这个```prototype```是一个对象，可以添加属性等（将共有的方法直接定义在prototype对象上，从而所有实例可以共享方法），这个对象的所有属性和方法都会被构造函数所拥有。以这个函数为构造函数构造出来的实例的原型均指向这个对象```prototype```.

因此，一般情况下，公共属性定义到构造函数上，公共方法放到原型对象上

```js
function Person() {
}
var person = new Person();
Person.prototype.action = function() {
    
}
console.log(Person.prototype);
console.log(person);
```

![image.png](https://i.loli.net/2020/03/18/tHCDB5IKzbQwOTL.png)

## ```__proto__```

每一个对象都有```__proto__``` 属性,这个属性指向该对象的原型（构造函数的原型）。

方法的查找规则：首先看实例对象是否有该方法，有则执行，若没有，则去原型对象中去查找。

```__proto__```对象原型的意义在于为对象的查找机制提供方向（路线），是一个非标准属性，不可以直接进行赋值等操作。

```js
function Person() {
}
var person = new Person();
console.log(person.__proto__ === Person.prototype); // true
```

![image.png](https://i.loli.net/2020/03/18/s4hcAYaPLD8EZlr.png)

## constructor

每个原型都有一个constructor，指向关联的构造函数（指回构造函数本身）；

用于记录该对象引用于哪个构造函数，可以让原型对象重新指向原来的构造函数。

当给构造函数的原型赋值操作时，将会覆盖原来的原型，constructor不存在，因此需要手动指向原来的构造函数

```js
Person === Person.prototype.constructor === person._proto_.constructor;
Person.prototype = {
    func1:function () {
        
    },
    func2: function () {
        
    }
}
console.log(Person.prototype)//只有func1、func2
//手动添加constructor
Person.prototype = {
    constructor: Person,
    func1:function () {
        
    },
    func2: function () {
        
    }
}

```

## 三者之间的关系



## 原型的原型

原型是一个对象，所以原型是由```Object```构造函数构造出来的。

实例的原型的原型指向的是```Object```构造函数的原型

```js
person._proto_.__proto__ === Object.prototype
Object.prototype.__proto__ === null
```





## 实例与原型

当读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查不到，就去找原型的原型，一直找到最顶层为止。

```js
function Person() {

}

Person.prototype.name = 'Kevin';

var person = new Person();

person.name = 'Daisy';
console.log(person.name) // Daisy

delete person.name;
console.log(person.name) // Kevin
```

# 原型链

![image.png](https://i.loli.net/2020/03/18/iEb5wcasup6kIeV.png)

## Javascript的成员查找机制

1. 当访问一个对象的属性或者方法时，首先查找对象本身是否拥有
2. 如果没有就查找对象的原型（```__proto__```的```prototype```原型对象）
3. 如果还没有就查找原型对象的原型（Object的原型对象）
4. 依次类推到null

**就近原则**

