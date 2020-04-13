---  
layout: post  
title: leetcode 合并 K 个有序链表
categories: [mergeSort, algorithm] 
description:  使用分治的方式合并链表
keywords: 算法  
---  

合并 k 个有序链表我用了两种方法去解题，一种是每次都遍历 k 个链表的头结点，找出里面的最小值，然后放在一个新的链表里，这种方法比较耗时，每次需要 O(kN) 的时间复杂度。另一种是使用分治的方式，像归并一样两两合并，这样每合完一次就会减小 1/2K 次循环，最终只用 log2K 的时间复杂度。
## 合并 k个有序链表

### 描述

```json
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```

### 解法1 直接遍历


```js
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function (lists) {
  let res, minIndex, head
  // 过滤空链表
  lists = lists.filter(v => v)

  if (!lists.length) {
    return null
  } else if (lists.length === 1) {
    return lists[0]
  }
  // 当链表里还有值的时候一直循环
  while (lists.length) {
    let min = new ListNode(lists[0].val)
    let minIndex = 0
    // 遍历列表
    for (let i = 1; i < lists.length; i++) {
      const item = lists[i]
      if (!item) {
        continue
      }
      if (item.val <= min.val) {
        min = item
        minIndex = i
      }
    }
    // 每次将最小值放入新的链表里
    if (res === undefined) {
      res = new ListNode(min.val)
      head = res
    } else {
      res.next = new ListNode(min.val)
      res = res.next
    }
    lists[minIndex] = lists[minIndex].next
    // 如果链表指向null, 则移除到数组之外
    if (!lists[minIndex]) {
      lists.splice(minIndex, 1)
    }
  }
  return head
};
```

### 解法2 分治

核心是每次都要合并两个链表，将合并 k 个链表变成合并多次两个有序的链表。
```js
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function (lists) {
  let res, minIndex, head
  // 过滤空的链表
  lists = lists.filter(v => v)

  if (!lists.length) {
    return null
  } else if (lists.length === 1) {
    return lists[0]
  }
  // 当数组里的链表里只剩下1个时就是结果了, 也可以设置一个变量来记录。
  while (lists.length !== 1) {
    for (let i = 0; i < lists.length / 2; i++) {
      // 假设有五个 则合并 12、34、5
      lists[i] = mergeTwoList(lists[2 * i], lists[2 * i + 1])
    }
    lists.length = Math.ceil(lists.length / 2)
  }
  return lists[0]
};
// 合并两个有序链表
function mergeTwoList(a, b) {
  let head, newList
  head = newList = new ListNode(0)
  while (a && b) {
    if (a.val < b.val) {
      newList.next = a
      a = a.next
    } else {
      newList.next = b
      b = b.next
    }
    newList = newList.next
  }
  if (a) {
    newList.next = a
  }
  if (b) {
    newList.next = b
  }
  return head.next
}
```