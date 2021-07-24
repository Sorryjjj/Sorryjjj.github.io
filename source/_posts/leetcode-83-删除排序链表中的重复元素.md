---
title: leetcode 83 删除排序链表中的重复元素
date: 2021-04-25 18:44:52
tags: [链表]
categories: leetcode
---

# 题目

[链接](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

![image-20210425184526776](https://tva1.sinaimg.cn/large/008i3skNgy1gpw6jmc2xdj31320py0ws.jpg)

# 思路

边遍历边删除，使用标记来记录已存在的值，保存前一个节点，需要删除的时候，将前一个节点的next指向当前节点的next即可

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
 * @param {ListNode} head
 * @return {ListNode}
 */
var deleteDuplicates = function(head) {
    let m = {};
    let curr = head;
    let pre = new ListNode();;
    while(curr) {
        if(m[curr.val]) {
            pre.next = curr.next;
            curr = curr.next;
        }
        else {
            pre = curr;
            m[curr.val] = 1;
            curr = curr.next;
        }
    }
    return head
};
```

