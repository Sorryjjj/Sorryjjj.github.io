---
title: leetcode 26 删除有序数组中的重复项
date: 2021-04-27 17:50:14
tags: [数组]
categories: leetcode
---

# 题目

[链接](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

![image-20210427175143022](https://tva1.sinaimg.cn/large/008i3skNgy1gpyg8dnt2gj30zq08idhf.jpg)

## 题解

注意给定的是有序数组，所以相同元素是连续的

快慢指针，遇到不同值则修改慢指针对应位置的值

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function (nums) {
    let index = 0;
    let curr = 0;
    while(curr < nums.length) {
        if(nums[curr] != nums[index]) {
            index++;
            nums[index] = nums[curr];
        }
        curr++;
    }
    return index + 1;
};
```

一开始没注意到有序，暴力解法，两层循环删除

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    let i = 0;
    while (i < nums.length) {
        let j = i + 1;
        while (j < nums.length) {
            if (nums[j] == nums[i]) {
                nums.splice(j, 1);
            }
            else {
                j++;
            }
        }
        i++;
    }
    return nums.length;
};
```



