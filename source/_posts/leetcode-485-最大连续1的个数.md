---
title: leetcode 485 最大连续1的个数
date: 2021-04-25 00:44:13
tags: [数组]
categories: leetcode
---

# 题目

[485 最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

![image-20210425004639272](https://tva1.sinaimg.cn/large/008i3skNly1gpvbd5dz5dj310a0hkmyv.jpg)

# 思路

一趟遍历，当值为1时记录当前连续1的个数curr，遇到0时，将max与curr比大小，并将curr置为0，当遇到下一个1时重新开始记录连续1的个数

注意，到达数组最后一个元素时，还要进行一次对比，否则当最后一个元素为1时会漏掉最后一次的max

# 代码

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMaxConsecutiveOnes = function(nums) {
    let max = 0
    let curr = 0
    nums.forEach((item,index) => {
       if(item == 1) {
            curr++
        }
        else if(item == 0 && index != nums.length - 1) {
            max = Math.max(max,curr)
            curr = 0
        }
        max = Math.max(max,curr)
    })
    return max
};
```

