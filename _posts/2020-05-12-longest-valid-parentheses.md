---  
layout: post  
title: leetcode 最长有效括号
categories: [dynamic-programming, algorithm] 
description: 使用动态规划解决最长有效括号问题
keywords: 算法  
---  
需要在每一个右括号处判断是否有可以闭合的左括号

## 最长有效括号
### 描述
给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。


### 解法 动态规划
遍历字符串时只需要判断右括号的情况
1. 前一个字符就是左括号，无嵌套的括号  
   长度直接加2，如果左括号前面一个字符存在，则加上它存的有效长度
   ```js
   if(s[i-1] === '(') {
     dp[i] = 2 + (dp[i - 2] >= 0 ? dp[i - 2] : 0)
   }
   ```
2. 前一个字符不是左括号，是在嵌套的括号里  
   则通过前一个储存的长度得到它的左括号，再判断前一个字符是不是左括号。  
   `dp[i - 1]` 前一个存储的值  
   `i - dp[i - 1] -1 >= 0` 边界判断。这个用来判断是否还有字符串可以闭合当前的括号，小于等于 0 时，已经到字符串最左边了。  
   `s[i - dp[i - 1] - 1] === '('` 前一个合法字符的前一个括号是否是左括号，是的话就可以闭合，对应 `(())` 这种情况  
   由于前一个字符存储的长度是嵌套括号里的长度，算完之后还需要判断再加上括号外前一个字符的有效长度。对应 `()(())`
   ( | ) | ( | ( | ) | )
   -|-|-|-|- |- 
   0 | 2 | 0 | 0 | 2 | 4
   ```javascript
   if (i - dp[i - 1] -1 >= 0 && s[i - dp[i - 1] - 1] === '(') {
     dp[i] = dp[i - 1] + (i - dp[i - 1] >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2
   } 
   ```
### 完整代码
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var longestValidParentheses = function (s) {
  if (s.length < 2) {
    return 0
  }
  let dp = Array(s.length).fill(0), i = 0, j = 0, maxlength = 0
  for (i = 1; i < s.length; i++) {
    if (s[i] === ')') {
      if (s[i - 1] === '(') {
        dp[i] = 2 + (dp[i - 2] >= 0 ? dp[i - 2] : 0)
      } else if (i - dp[i - 1] > 0 && s[i - dp[i - 1] - 1] === '(') {
        dp[i] = dp[i - 1] + (i - dp[i - 1] >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2
      }
      maxlength = Math.max(dp[i], maxlength)
    }
  }
  return maxlength
};
```