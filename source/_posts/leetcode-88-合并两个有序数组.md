---
title: leetcode 88 合并两个有序数组
date: 2021-04-27 17:58:43
tags: [数组]
categories: leetcode
---

# 题目

[链接](https://leetcode-cn.com/problems/merge-sorted-array/)

![image-20210427175910693](https://tva1.sinaimg.cn/large/008i3skNly1gpygg2vqrkj30zc0koq5l.jpg)

# 题解

两个数组归并，使用双指针，从数组末尾开始填充

```js
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function (nums1, m, nums2, n) {
    let index = nums1.length - 1;
    m--;
    n--;
    while (m >= 0 && n >= 0) {
        if (nums1[m] > nums2[n]) {
            nums1[index] = nums1[m--];
        }
        else {
            nums1[index] = nums2[n--];
        }
        index--;
    }
    if (m >= 0) {
        for (let i = m; i >= 0; i--) {
            nums1[index--] = nums1[i];
        }
    }
    if (n >= 0) {
        for (let i = n; i >= 0; i--) {
            nums1[index--] = nums2[i];
        }
    }
};
```

也可以合并数组之后排序

```js
var merge = function(nums1, m, nums2, n) {
    nums1.splice(m, nums1.length - m, ...nums2);
    nums1.sort((a, b) => a - b);
};
```

