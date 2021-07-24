---
title: leetcode 27 移除元素
date: 2021-04-25 01:07:06
tags: [数组]
categories: leetcode
---

# 题目

[移除元素](https://leetcode-cn.com/problems/remove-element/)

![image-20210425010813246](https://tva1.sinaimg.cn/large/008i3skNly1gpvbzkog9oj310q0catat.jpg)

# 思路

同leetcode 283

# 代码

```js
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    let index = 0;
    for(let i = 0; i < nums.length; i++) {
        if(nums[i]!=val) {
            nums[index] = nums[i];
            index++;
        }
    }
    return nums.slice(0,index).length
};
```

