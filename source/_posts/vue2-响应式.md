---
title: vue2 initMixin
date: 2021-04-24 22:04:04
tags:
categories: Vue3
---

# 开发环境搭建

```sh
mkdir vue
npm init -y
npm install @babel/preset-env @babel/core rollup rollup-plugin-babel rollup-plugin-serve cross-env -D
```

- rollup：打包工具
- babel/core：babel核心模块
- babel/preset-env：js语法转换
- rollup-plugin-babel：在rollup中使用babel
- rollup-plugin-serve：启用静态服务
- cross-env：设置环境变量

# rollup

Rollup 是一个 JavaScript 模块打包器，可以将小块代码编译成大块复杂的代码

开发应用时使用webpack，开发库时使用rollup

```sh
npm install --global rollup
```

假设应用程序入口起点的名称为 main.js，并且你想要所有 import 的依赖(all imports)都编译到一个名为 bundle.js 的单个文件中

对于浏览器和 Node.js

```sh
rollup main.js --file bundle.js --format umd --name "myBundle"
```

### Tree-shaking

除了使用 ES6 模块之外，Rollup 还静态分析代码中的 import，并将排除任何未实际使用的代码(将无用代码从最终的生成文件中删去)

## rollup.config.js

rollup配置文件

```js
import babel from 'rollup-plugin-babel';
import serve from 'rollup-plugin-serve';

export default {
    input: './src/index.js',
    output: {
        format: 'umd', // amd commonjs规范  默认将打包后的结果挂载到window上
        file: 'dist/vue.js', // 打包出的vue.js 文件  new Vue
        name: 'Vue',
        sourcemap: true
    },
    plugins: [
        babel({
            exclude: "node_modules/**" // 排除文件
        }),
        serve({ // 开启本地服务
            open: true,
            openPage: '/public/index.html',
            port: 3000,
            contentBase: ''
        })
    ]
}
```

## .babelrc

```
{
    "presets": [
        "@babel/preset-env"
    ]
}
```

## package.json

```json
  "scripts": {
    "serve": "rollup -c -w",
    "build:dev": "rollup -c"
  }
```

# mini-vue

从零实现vue

## 目录树

目标结构，本文主要实现initMixin

```txt
├── dist
│   ├── vue.js
│   └── vue.js.map
├── note.md
├── package-lock.json
├── package.json
├── public
│   ├── backup.html
│   └── index.html
├── rollup.config.js
└── src
    ├── compiler
    │   ├── generator.js
    │   ├── index.js
    │   └── parser.js
    ├── global-api
    │   └── index.js
    ├── index.js
    ├── init.js
    ├── lifecycle.js
    ├── observer
    │   ├── array.js
    │   ├── dep.js
    │   ├── index.js
    │   ├── scheduler.js
    │   └── watcher.js
    ├── render.js
    ├── state.js
    ├── utils.js
    └── vdom
        ├── create-element.js
        └── patch.js
```

## src/index.js

入口文件

```js
// 只声明vue
import { initMixin } from "./init"

function Vue (options){
  this._init(options);  // vue初始化
}

initMixin(Vue); // 给vue原型扩展方法

export default Vue;

```

## src/init.js

给vue原型扩展`_init`等方法，从而在index.js中可以调用`_init`初始化vue

在`_init`方法中，所有的`options`被放到了`vm.$options`中，这不仅让之后代码中可以更方便的来获取用户传入的配置项，也可以让用户通过这个`api`来获取实例化时传入的一些自定义选项

```js
// init.js vue初始化
import { initState } from "./state";

// 在原型上扩展_init方法
export function initMixin(Vue) {
    Vue.prototype._init = function (options) {
        // Vue的内部 $options 就是创建vue实例传入的参数
        const vm = this;
        // 这个options 就包含了创建实例时传入的所有属性 Vue.options
        vm.$options = options;
        //初始化状态
        initState(vm);//分割代码
    }
}
```

## src/state.js

将构造vue实例传入的参数进行初始化

initState方法中会判断选项中传入的属性，来分别进行`props`、`methods`、`data`、`watch`、`computed` 等配置项的初始化操作

```js
// 初始化状态
import { observe } from "./observer/index";

export function initState(vm) {
    const opts = vm.$options;
    if(opts.props) {
        initProps(vm)
    }
    if(opts.methods) {
        initMethods(vm)
    }
    if(opts.data) {
        initData(vm)
    }
    if(opts.computed) {
        initComputed(vm)
    }
    if(opts.watch) {
        initWatch(vm)
    }
}

function initProps(){

}

function initMethods(){

}

function initData(vm){
    // 初始化数据
    let data = vm.$options.data;
    data = vm._data = typeof data === "function" ? data.call(vm) : data
    // 数据劫持
    observe(data); //响应式
}

function initComputed(){

}

function initWatch(){

}
```

## src/observer

将options中的data通过Observer类转为响应式

- class Observer：根据数据类型执⾏对应的响应化操作
- observe()：返回⼀个Observer实例
- defineReactive()：定义对象属性的getter/setter，getter负责添加依赖，setter负责通知更新

`Observer`类中会为对象和数组都添加`__ob__`属性，之后便可以直接通过`data`中的对象和数组`vm.value.__ob__`来获取到`Observer`实例，**`__ob__`属性要设置为不可枚举，否则之后在对象遍历时可能会引发死循环**

- 对象：递归的为对象的每个属性都设置`get/set`方法
- 数组：修改数组的原型方法，对于会修改原数组的方法进行了重写

### **src/observer/index.js**

```js
import { isObject, def } from "../util/index"
import { arrayMethods } from './array.js'

class Observer {
    constructor(value) {
        // value.__ob__ = this;
        def(value, '__ob__', this); //  __ob__是响应式标记，指向本身
        // 观察数组
        if (Array.isArray(value)) {
            this.observeArray(value);
        }
        else {
            //观察对象
            this.walk(value);
        }
    }

    observeArray(value) {
      data.__proto__ = arrayMethods; 
      this.observeArray(data);
    }

    walk(data) {
      	// 循环处理每个属性
        Object.keys(data).forEach(key => {
            let value = data[key];
            defineReactive(data, key, value);
        })
    }

}
// 递归重写get和set
function defineReactive(data, key, value) {
    observe(value); //递归深度检测
    Object.defineProperty(data, key, {
        get() {
            return value
        },
        set(newValue) {
            if (newValue === value) return;
            observe(newValue); // 观察新设置的值
            value = newValue;
        }
    })
}
// 把data中的数据都用Object.defineProperty重新定义
export function observe(data) {
    if (!isObject(data)) {
        return
    }
    return new Observer(data); //观测数据
}
```

## src/observer/array.js

对数组进行响应式转化，重写数组原型方法

```js
let oldArrayMethods = Array.prototype; // 获取数组原型上的方法

// 创建一个全新的对象 可以找到数组原型上的方法，而且修改对象时不会影响原数组的原型方法
export let arrayMethods = Object.create(oldArrayMethods);

let methods = [ // 这七个方法都可以改变原数组
    'push',
    'pop',
    'shift',
    'unshift',
    'sort',
    'reverse',
    'splice' 
]
methods.forEach(method=>{
    arrayMethods[method] = function (...args) { // 函数劫持 AOP
        // 当用户调用数组方法时 会先执行改造的逻辑 再执行数组默认的逻辑
        const ob = this.__ob__;
        let result  = oldArrayMethods[method].apply(this,args);
        let inserted;
        // push unshift splice 都可以新增属性  （新增的属性可能是一个对象类型）
        // 内部还对数组中引用类型也做了一次劫持  [].push({name:'zf'})
        switch (method) {
            case 'push':
            case 'unshift':
                inserted = args
                break;
            case 'splice': // 也是新增属性  可以修改 可以删除  [].splice(arr,1,'div')
                inserted = args.slice(2);
                break;
            default:
                break;
        }
        inserted && ob.observeArray(inserted);
        return result;
    }
})
```

## src/util/index.js

所有的工具方法

```js
export function isObject(obj) {
    return typeof obj === 'object' && obj !== null
}

export function def (data,key,value) {
    Object.defineProperty(data,key, {
        enumerable: false,
        configurable: false,
        value
    })
}
```

# 调试

## src/public/index.html

引入打包后的vue.js，并且构造Vue实例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src="../dist/vue.js"></script>
    <script>
        let vm = new Vue({
            el: '#app',
            data(){
                return {
                    foo: "bar"
                }
            }
        })
        vm._data.foo = "aaa"
    </script>
</body>
</html>
```

## 开启静态服务

```sh
npm run serve
```

![image-20210424235144107](https://tva1.sinaimg.cn/large/008i3skNly1gpv9s02xunj30qw08cta5.jpg)

## 打包

```sh
npm run build:dev
```

![image-20210424235241424](https://tva1.sinaimg.cn/large/008i3skNly1gpv9szoblcj30qq0a2dhn.jpg)

