---
title: leetcode 283 移动零
date: 2021-04-25 00:55:49
tags: [数组]
categories: leetcode
---

# 题目

[移动零](https://leetcode-cn.com/problems/move-zeroes/)

![image-20210425005649844](https://tva1.sinaimg.cn/large/008i3skNly1gpvbnqco35j310u0hmdi1.jpg)

# 思路

两种思路，重写与删除

## 重写

需要注意必须在原数组上操作，以及保持相对顺序

一趟遍历，重写数组，使用index标记当前是第几个非0元素，遇到非0时，将数组的第index位赋值为此非0元素，index++

最后将末尾全置为0

## 删除

一趟遍历，遇到0时删除当前元素，最后将0写入数组末尾

# 代码

```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function(nums) {
    let index = 0;
    for(let i = 0; i < nums.length; i++) {
        if(nums[i] != 0) {
            nums[index] = nums[i]
            index++;
        }
    }
    for(let i = index; i< nums.length; i++) {
        nums[i] = 0
    }
    return nums
};
```

```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function(nums) {
    let count = 0;
    let i = 0;
    while(i != nums.length) {
        if(nums[i] == 0) {
            nums.splice(i,1);
            count++;
            i--
        }
        i++
    }
    for(let i = 0; i < count; i++) {
        nums.push(0)
    }
    return nums
};
```

