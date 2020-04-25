---  
layout: post  
title: leetcode 下一个排列
categories: [algorithm] 
description:  使用字典序法找到的下一个排列
keywords: 算法  
---  
需要找出当前数组的排排列，使用了字典序法。  
1. 假设数组有n个元素，找出从右往左满足第 arr[i] > arr[i+1] 的元素
2. 从 i+1 往后找到比 i+1 大的最小值 j。
3. 交换 i, j
4. 把 i 右边的元素做一个倒序。

## 下一个排列

### 描述

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须原地修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

### 解法 字典序法
需要遍历最坏的情况下需要遍历整个数组 O(N)，寻找最小的值O(N - 1)，倒序整个数组 - 1，O(N - 1)，总共三个循环。  
时间复杂度为 O(3N-2) => O(N) 
```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var nextPermutation = function (nums) {
  let length = nums.length, i = length - 2
  var swap = function (i, j) {
    [nums[i], nums[j]] = [nums[j], nums[i]]
  }
  // 寻找一个升序序列
  for (; i >= 0; i--) {
    if (nums[i] < nums[i + 1]) {
      break
    }
  }
  // 没有就倒序数组
  if (i === -1) {
    return nums.reverse()
  }
  // 寻找 i+1 向右最小的一个值
  let min = Infinity, tmp, j = i + 1, k = j
  for (; j < length; j++) {
    tmp = nums[j] - nums[i]
    if (tmp <= 0) {
      continue
    } else if (min >= tmp) {
      min = tmp
      k = j
    }
  }
  // 交换这两个值
  swap(i, k)
  ++i
  // 最后两个元素交换的话，不需要倒序
  if (i === length - 1) {
    return nums
  }
  // 倒序
  let orderLength = (length - i) / 2 >> 0
  for (k = 0; k < orderLength; k++) {
    swap(i + k, length - 1 - k)
  }
  return nums
};
```