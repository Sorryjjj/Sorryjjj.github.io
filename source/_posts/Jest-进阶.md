---
title: Jest 进阶
date: 2021-06-18 17:28:43
tags: Jest
categories: Jest
---

# 快照

toMatchSnapshot：将参数与快照进行匹配

```js
export const generateConfig = () => {
    return {
        server: 'http://localhost',
        port: 8000
    }
}
```

```js
import { generateConfig } from "./demo";

test('test generateConfig', () => {
    // expect(generateConfig()).toEqual({
    //     server: 'http://localhost',
    //     port: 8000
    // })
    expect(generateConfig()).toMatchSnapshot()
})
```

执行测试时，如果与上一次执行时记录的快照不匹配则会失败

可以通过更新快照解决

常用于配置文件与UI组件的测试

# 定时器

正常调用定时器，不会执行到callback的调用

因为测试代码已经执行完了，callback已经交给了环境

## done

正常执行定时器，done表示

```js
export const timer = (cb) => {
    setTimeout(() => {
        cb()
    }, 3000);
}
```

```js
test('test timer', (done) => {
    timer(() => {
        expect(1).toBe(1)
        done()
    })
})
```

加上done 表示等待，jest 在执行单元测试的时候，如果它看到一个测试用例(test) 的第二个函数参数有一个done参数， 它就知道，这个测试用例，执行完最后一行代码的时候，还不算测试完成，还要等待，等待done函数的执行，只有done函数执行了，这个测试用例才算完成

## useFakeTimers + runAllTimers

模拟定时器，不需要等待

- 使用`jest.useFakeTimers()`声明使用虚拟的时间
- 再使用 `jest.runAllTimers()` 将全部定时器立即运行结束
- 如果内部有多个定时器，只想运行一个定时器可以使用`jest.runOnlyPendingTimers()`

```js
jest.useFakeTimers();

test('test timer', () => {
    const fn = jest.fn()
    timer(fn)
    jest.runAllTimers()
    expect(fn).toHaveBeenCalledTimes(1)
})
```

## useFakeTimers + advanceTimersByTime

立即设置推后多少毫秒

```js
jest.useFakeTimers();

test('test timer', () => {
    const fn = jest.fn()
    timer(fn)
    // jest.runAllTimers()
    jest.advanceTimersByTime(3000)
    expect(fn).toHaveBeenCalledTimes(1)
})
```

# 对Class的测试

```js
// util.js
class Util {
    a(){
        return 1
    }
}

export default Util
```

```js
// util.test.js
import Util from "./util";
let util = null
beforeAll(() => {
    util = new Util()
})

test('test util function a', () => {
    expect(util.a()).toBe(1)
})
```

在单元测试中 我们更应该关注函数是否被调用 而不是函数中的复杂方法

因此使用 mock 方法来进行测试

```js
// classDemo.js
import Util from "./util";
const demoFunction = (a,b) => {
    const util = new Util()
    util.a()
}
export default demoFunction
```

```js
// classDemo.test.js
jest.mock('./util') // jest.mock 类的话，会自动把类的构造函数和方法变成jest.fn() 不是真正地需要执行类中复杂的方法
import Util from "./util";
import demoFunction from "./classDemo"

test('test demoFunction',()=> {
    demoFunction()
    expect(Util).toHaveBeenCalled()
    // console.log(Util.mock.instances[0])
    expect(Util.mock.instances[0].a).toHaveBeenCalled()
})
```



```js
// 手动模拟类，自定义类的方法实现 /__mocks__/util.js
const Util = jest.fn()
Util.prototype.a = jest.fn(()=> {
    console.log('a');
})
Util.prototype.b = jest.fn()
export default Util
```

# 对DOM的测试

jest在node环境下模拟了一套DOM的api，可以直接使用

```js
// demo.js
import $ from 'jquery'

export default () => {
  $('body').append('<div></div>')
}

// demo.test.js
import appendDiv from './demo'
import $ from 'jquery'

test('在 body 中添加 1 个 div', () => {
  addDivToBody()
  expect($('body').find('div').length).toBe(1)
})
```



