---
title: Jest 入门
date: 2021-06-18 14:30:02
tags: Jest
categories: Jest
---

# 开始

[Jest](https://www.jestjs.cn/docs/getting-started)

安装`Jest`以及`babel`

`Jest`配置项

```sh
npm install --save-dev jest
npm install @babel/core @babel/preset-env
npx jest --init
```

```js
// sum.js
export function sum(a, b) {
  return a + b;
}
```

```js
// sum.test.js
import { sum } from "./sum";
test('adds 1 + 2 to equal 3', () => {
    expect(sum(1, 2)).toBe(3);
});
```

```sh
npm run test
```

![image-20210618155426491](https://tva1.sinaimg.cn/large/008i3skNly1grmh2c20yyj311k0f4jtz.jpg)



# matchers

Jest使用匹配器以不同的方式测试数据是否相等

在sum.test.js中

- `test` ：测试用例，接受两个参数，第一个是测试的名称，第二个是函数
- `expect`：期望
- `toBe`：匹配器，用于匹配expect的值是否和匹配器中的值相等

## Common Matchers

- `toBe`：相对于 `===` 和 `Object.is`，匹配值（不能用于测试浮点数）
- `toEqual`：匹配值，可以用于引用类型的匹配

```js
test('two plus two is four', () => {
  expect(2 + 2).toBe(4);
});
test('object assignment', () => {
  const data = {one: 1};
  data['two'] = 2;
  expect(data).toEqual({one: 1, two: 2});
});
```

## Truthiness

当需要判断`undefined`，`null`和`false`时

- `toBeNull` ：只匹配`null`
- `toBeUndefined` ：只匹配`undefined`
- `toBeDefined` ：匹配已定义的值，与`toBeUndefined`相反
- `toBeTruthy`：匹配所有结果为`true`的值，相对于 `==`
- `toBeFalsy` ：匹配所有结果为false的值，相对于 `==`
- `not`：对后续的匹配取反

## Numbers

- `toBeGreaterThan`
- `toBeGreaterThanOrEqual`
- `toBeLessThan`
- `toBeLessThanOrEqual`
- `toBe`
- `toEqual`
- `toBeCloseTo`

## Strings

- `toMatch`：检查字符串是否匹配

```js
test('test toMatch', () => {
  let str = 'abcdefg'
  expect(str).toMatch('ab')
  expect(str).toMatch(/[a-z]/)
})
```

## Arrays and iterables

- `toContain`：匹配数组/Set/字符串中是否包含item

```js
const shoppingList = [
  'diapers',
  'kleenex',
  'trash bags',
  'paper towels',
  'milk',
];

test('the shopping list has milk on it', () => {
  expect(shoppingList).toContain('milk');
  expect(new Set(shoppingList)).toContain('milk');
});
```

## More

[https://www.jestjs.cn/docs/expect](https://www.jestjs.cn/docs/expect)

# 钩子函数

- `beforeAll`：所有测试之前执行
- `afterAll`：所有测试执行完之后
- `beforeEach`：每个测试实例之前执行
- `afterEach`：每个测试实例完成之后执行

## describe

describe创造一个块，可以将多个测试用例放在一个块中

每个块中也可以使用钩子函数

```js
beforeAll(() => console.log('1 - beforeAll'));
afterAll(() => console.log('1 - afterAll'));
beforeEach(() => console.log('1 - beforeEach'));
afterEach(() => console.log('1 - afterEach'));
test('', () => console.log('1 - test'));
describe('Scoped / Nested block', () => {
  beforeAll(() => console.log('2 - beforeAll'));
  afterAll(() => console.log('2 - afterAll'));
  beforeEach(() => console.log('2 - beforeEach'));
  afterEach(() => console.log('2 - afterEach'));
  test('', () => console.log('2 - test'));
});
```

## 注意

不在钩子函数或者测试用例中的代码，会被最先执行

```js
describe('outer', () => {
  console.log('describe outer-a');

  describe('describe inner 1', () => {
    console.log('describe inner 1');
    test('test 1', () => {
      console.log('test for describe inner 1');
      expect(true).toEqual(true);
    });
  });

  console.log('describe outer-b');

  test('test 1', () => {
    console.log('test for describe outer');
    expect(true).toEqual(true);
  });

  describe('describe inner 2', () => {
    console.log('describe inner 2');
    test('test for describe inner 2', () => {
      console.log('test for describe inner 2');
      expect(false).toEqual(false);
    });
  });

  console.log('describe outer-c');
});

// describe outer-a
// describe inner 1
// describe outer-b
// describe inner 2
// describe outer-c
// test for describe inner 1
// test for describe outer
// test for describe inner 2
```

## test.only

只执行单个测试用例

```js
test.only('this will be the only test that runs', () => {
  expect(true).toBe(false);
});

test('this test will not run', () => {
  expect('A').toBe('A');
});
```

# Mock

```js
// runCallback.js
export function runCallback(callback) {
  callback()
}
```

```js
// runCallback.test.js
import {runCallback} from './runCallback.js'
test('test runCallback', () => {
  const func = jest.fn() // mock函数，捕获函数的调用
  func.mockReturnValueOnce('dell')
  runCallback(func)
  runCallback(func)
  runCallback(func)
  expect(func).toBeCalled()
  expect(func.mock.calls.length).toBe(3)
  expect(func.mock.results[0].value).toBe('dell')
})
```

- jest.fn()：这里使用jest模拟一个函数，可以传入一个函数生成带逻辑的函数
- mockReturnValueOnce(：设置调用一次函数的返回值，设置几个就是几次
- mockReturnValue：设置调用函数的返回值
- mock 每个jest生成的函数都有mock属性，包含
  - calls：每次调用函数的参数
  - instances：每次调用函数的this
  - invocationCallOrder：函数执行顺序
  - results：每次调用的返回值

# 模拟接口

jest.mock('axios')模拟axios请求数据，不会发送真实请求

```js
import axios from 'axios'
jest.mock('axios')

test('测试 getData，使用mock', async() => {
  // 模拟第一次接收到的数据
  axios.get.mockResolvedValueOnce({
    data: '123'
  })
  // 模拟每一次接收到的数据
  axios.get.mockResolvedValue({
    data: '456'
  })

  const data1 = await getData()
  const data2 = await getData()
  expect(data1).toBe('123')
  expect(data2).toBe('456')
  
})
```

