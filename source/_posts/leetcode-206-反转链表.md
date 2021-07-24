---
title: leetcode 206 反转链表
date: 2021-04-27 10:26:58
tags: [链表]
categories: leetcode
---

# 题目

[反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

![image-20210427102918414](https://tva1.sinaimg.cn/large/008i3skNgy1gpy3g1otndj31320na43g.jpg)

# 题解

## 递归

假设除第一个节点，链表的其余部分已经被反转，那么只需要处理如何反转第一个节点

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
    if(head == null) return head;
    if(head.next == null) return head;
    let newHead = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return newHead;
};
```

## 迭代

边遍历边修改链表，需要记录前一节点与当前节点，每次反转前一节点与当前节点，之后更新两个节点

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
    let pre = null;
    let curr = head;
    let tmp;
    while(curr) {
        tmp = curr.next; // 缓存下一个节点
        curr.next = pre; // 当前节点指向前一节点，反转当前节点
        pre = curr; // 更新前一节点为当前节点
        curr  = tmp; // 更新当前节点为下一个节点
    }
    return pre
};
```

