---
layout: post
title: 使用js实现堆排
categories: algorithm
description: 根据堆排原理自己实现堆排算法
keywords: 算法,堆排
---
# 堆排

## 原理

堆可以当作是一个不完整的二叉树，实现堆排并不用构建一个新的数据结构，只需要将数据当作完全二叉树处理就好了。例如：
数组[4,5,1,7,3,15,9,11,2,55,10,12]可以表示为
![TIM图片20180605180830.jpg](https://i.loli.net/2018/06/05/5b16614c7fe68.jpg)
可以从n/2开始处理每一个节点,大于n/2的节点是没有子节点的，例子中数组中有12个元素，可以看到从第7个开始便没有子节点了。
堆排序的循环比较简单。

1.第n/2的节点开始，如果子节点大于父节点，就将数据中两个值互换。
![图片2.png](https://i.loli.net/2018/06/05/5b16634d9d910.png)
  
2.循环完之后，可得到这个数组中的最大值，也就是数组的第一个值。和最后一个值互换，然后继续将数组堆化，每次获得的最大值与数组的最后一个值互换。

## 代码

``` javascript
var headSort = function (arr) {
  var swap = function (i, j) {
    [arr[i], arr[j]] = [arr[j], arr[i]]
  }
  var max_heap = function (start, end) {
    let dad = start
    let son = start * 2 + 1
    if (son >= end) {
      return
    }
    if (son + 1 < end && arr[son] < arr[son + 1]) {
      son++
    }
    if (arr[dad] < arr[son]) {
      swap(dad, son)
      max_heap(son, end)
    }
  }
  for (let i = ~~(arr.length / 2) - 1; i >= 0; i--) {
    max_heap(i, arr.length)
  }
  for (let i = arr.length - 1; i >= 0; i--) {
    swap(0, i)
    max_heap(0, i)
  }
}
var a = [3, 1, 2, 4]
headSort(a)
console.log(a)
```
