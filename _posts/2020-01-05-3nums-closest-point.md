---  
layout: post  
title: leetcode 最接近三数之和
categories: [two-pointers, algorithm] 
description:  React Context 的使用
keywords: 算法  
---  
今年要多做一些算法题了，算法、数据结构、设计模式之类的基础都还比较薄弱，希望今年自己可以在算法方面提高一些。

## 最接近三数之和

### 描述
给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).

### 解法1 三重循环（居然没有超时。。）

直接嵌套三层循环去穷举出所有可能出现的情况，同时记录下最优的结果。本来以为三重循环会超时，结果居然通过了。
```javascript

var threeSumClosest = function (nums, target) {
  let res = nums[0] + nums[1] + nums[2]
  for (let i = 0; i < nums.length - 2; i++) {
    for (let j = i + 1; j < nums.length - 1; j++) {
      for (let k = j + 1; k < nums.length; k++) {
        // 本次循环的值
        const total = nums[i] + nums[j] + nums[k]
        // 最小值与target的距离
        const tmpres = Math.abs(target - res)
        // 本次循环与target的距离
        const tmpTotal = Math.abs(target - total)
        // 已经是距离为0直接结束了
        if (tmpTotal === 0) {
          return total
        }
        // 有更小的值则赋值给res
        if (tmpTotal <= tmpres) {
          res = total
        }
      }
    }
  }
  return res
};
```

### 解法2 双指针
首先要先将输入的数组进行排序，然后先循环第一个数。剩余两个数一个在当前索引的后面，一个在数组的最后。当取出计算的和为负数时，第二个指针向后 （+1）可以减小和，当和为正数时，第三个指向前（-1）可以减小和的大小。代码比解法1 多了一些，但是节省了许多没有必要的循环。
```javascript
var threeSumClosest = function (nums, target) {
  nums = nums.sort((a, b) => a - b);
  let res = nums[0] + nums[1] + nums[2];
  if (nums.length <= 3) {
    return res;
  }
  let j = 0; let k = 0;
  for (let i = 0; i < nums.length - 2; i++) {
    // 第二个指针
    j = i + 1;
    // 第三个指针
    k = nums.length - 1;
    while (j !== k) {
      // 本次循环的值
      const total = nums[i] + nums[j] + nums[k];
      // 本次循环与target的距离
      const tmpTotal = Math.abs(target - total);
      // 最小值与target的距离
      const tmpres = Math.abs(target - res);
      if (tmpTotal === 0) {
        return total
      }
      if (tmpTotal <= tmpres) {
        res = total;
      }
      total > target ? --k : ++j;
    }
  }
  return res;
};
```