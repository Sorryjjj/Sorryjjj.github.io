---
title: leetcode 11 盛最多水的容器
date: 2021-05-24 10:21:03
tags:
categories: leetcode 
---

# 盛最多水的容器

[题目链接](https://leetcode-cn.com/problems/container-with-most-water/)

![image-20210524102222093](https://tva1.sinaimg.cn/large/008i3skNly1gqtaz4s1soj30u00wmwja.jpg)

# 题解

两层循环的时间复杂度O(n^2)会超时

因为面积 = 长 * 宽，长是两个高度中较小的值，所以，从左右两端开始，只有当高度增大时，面积才会增大

只需要选择正确的高度，指针前进即可

这样就减少了不必要的遍历

所以用双指针，将其降为O(n)

```js
/**
 * @param {number[]} height
 * @return {number}
 */
var maxArea = function(height) {
    let max = 0
    let len = height.length
    for(let i = 0, j = len - 1; i < j;) {
        let currHeight = height[i] < height[j] ? height[i++] : height[j--]
        max = Math.max(currHeight * (j-i+1),max)
    }
    return max
};
```

