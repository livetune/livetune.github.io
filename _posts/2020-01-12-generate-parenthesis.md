---  
layout: post  
title: leetcode 括号生成
categories: [backtracking, algorithm] 
description:  使用回溯生成括号
keywords: 算法  
---  

生成括号可以有许多种排列组合，我们需要从所有的组合里找出适合的几组数据。这类问题可以使用回溯法来解，如果使用嵌套多层遍历暴力去解的话会超时。

## 最接近三数之和

### 描述
给出 n 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且有效的括号组合。

例如，给出 n = 3，生成结果为：
```json
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```
### 解法 回溯（递归）
回溯的写法一般是用 dfs 来实现，每次生成的结果判断是否符合规则，符合的话就继续往下走，直到所有情况的递归完。

```js
var generateParenthesis = function (n) {
  const results = []
  // 第一次传空字符串
  dfs(results, '', '(', 0, n)
  return results
};
/**
 * @param {*} results  果集
 * @param {*} res 当前值
 * @param {*} currentVal 当前需要加哪一种括号
 * @param {*} quotaStack 当前的括号是否符合规则
 * @param {*} target 当前需要多少组括号
 */
var dfs = function (results, res, currentVal, quotaStack, target) {
  res += currentVal
  currentVal === '(' ? ++quotaStack : --quotaStack
  // 小于 0，括号提前闭合，也不能大于所需要的括号数
  if (quotaStack < 0 || quotaStack > target) {
    return
  }
  // 括号数满足，且括号完成
  if (res.length / 2 === target) {
    quotaStack === 0 && (results.push(res))
    return
  }
  dfs(results, res, '(', quotaStack, target)
  dfs(results, res, ')', quotaStack, target)
}
```