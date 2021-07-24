---
title: leetcode 21 合并两个有序链表
date: 2021-04-25 18:10:47
tags: [链表]
categories: leetcode
---

# 题目

[合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

![image-20210425181150836](https://tva1.sinaimg.cn/large/008i3skNly1gpw5kprdyhj30yl0u0n6x.jpg)

# 思路

## 迭代

使用一个指针迭代查找下一个较小的节点

注意需要保留头节点用于最后返回，以及有一个链表遍历完了，要将指针指向剩余所有

## 递归

递归合并

每两个节点进行对比，将其中较小的作为返回值，之后比较其next节点与较大的节点

# 代码

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
    let head = new ListNode()
    let pre = head
    while(l1 != null && l2 != null) {
        if(l1.val <= l2.val) {
            pre.next = l1
            l1 = l1.next
        }else {
            pre.next = l2
            l2 = l2.next
        }
        pre = pre.next
    }
    pre.next = l1 == null ? l2 : l1
    return head.next
};
```



```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
    if(l1 == null) return l2;
    if(l2 == null) return l1;
    if(l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next,l2);
        return l1;
    }
    else {
        l2.next = mergeTwoLists(l2.next,l1);
        return l2;
    }
};
```

