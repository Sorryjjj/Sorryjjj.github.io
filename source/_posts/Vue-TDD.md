---
title: Vue TDD
date: 2021-06-26 14:30:35
tags: [jest,Vue,vue]
categories: Jest
---

# TDD

Test Driver Development 

Red-Green Development

测试驱动开发

1. 编写测试用例
2. 运行测试用例，此时无法通过
3. 编写代码，使测试用例通过
4. 优化代码，完成开发

优势

- 长期减少回归bug
- 提高代码质量
- 测试覆盖率高
- 减少错误的测试代码

# Vue Test Utils

[Vue Test Utils](https://vue-test-utils.vuejs.org/zh/)

> 和 mount 一样，创建一个包含被挂载和渲染的 Vue 组件的 Wrapper，不同的是被存根的子组件。

> Vue Test Utils 允许你通过 shallowMount 方法只挂载一个组件而不渲染其子组件 (即保留它们的存根)

```js
import { shallowMount } from '@vue/test-utils'
import HelloWorld from '@/components/HelloWorld.vue'

describe('HelloWorld.vue', () => {
  it('renders props.msg when passed', () => {
    const msg = 'new message'
    const wrapper = shallowMount(HelloWorld, {
      propsData: { msg }
    })
    expect(wrapper.text()).toMatch(msg)
  })
})
```

