---
layout: post
title: leetcode 编辑距离
categories: [dynamic-programming, algorithm]
description: 使用动态规划解决编辑距离问题
keywords: 算法
---

暴力的方法是使用递归一层一层计算所有的路径，这个一定会超时。使用动态规划将其转化为多个子问题，可以完美解决

## 编辑距离

### 描述

```
给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

插入一个字符
删除一个字符
替换一个字符
```
### 解法 动态规划

首先路径会有四种情况。

1. 两个单字母相同，下一个字母的距离取 i ， j 的距离，不用操作
    ```javascript
    map[i+1][j+1] = map[i][j]
    ```
2. 要进行插入操作，下一个单词的距离取 i+1, j 的距离，相当于继续计算第一个单词的距离，忽略第二个单词和插进来的单词
    ```javascript
    map[i+1][j+1] = map[i+1][j] + 1
    ```
3. 要进行删除操作，下一个单词的距离取 i, j+1 的距离，相当于继续计算第二个单词的距离，忽略那个被删掉的单词。
    ```javascript
    map[i+1][j+1] = map[i][j+1] + 1
4. 要进行替换操作，下一个单词的距离取 i, j 的距离，和第一种情况相同
    ```javascript
    map[i+1][j+1] = map[i][j+1] + 1
    ```
可以列一张表，当进行插入操作的时候取右边的值，当进行删除操作时取下面的值，当值相同或者直接替换，直接使用当前值。
```
    #	H	O	R	S	E
#	0	1	2	3	4	5
R	1	1	2	2	3	4
O	2	2	2	3	3	4
S	3	3	3	3	4	4
```
### 代码
```javascript
/**
 * @param {string} word1
 * @param {string} word2
 * @return {number}
 */
const minDistance = function (word1, word2) {
    let map = []
    // 构建初始化表格（空单词变为word1、word2的路径）
    for (let i = 0; i <= word1.length; i++) {
        for (let j = 0; j <= word2.length; j++) {
            if (!map[i]) {
                map[i] = []
            }
            if (i === 0) {
                map[i][j] = j
            } else if (j === 0) {
                map[i][j] = i
            } else {
                map[i][j] = 0
            }
        }
    }
    for (let i = 0; i < word1.length; i++) {
        for (let j = 0; j < word2.length; j++) {
            if (word1[i] === word2[j]) {
                map[i + 1][j + 1] = map[i][j]
            } else {
                map[i + 1][j + 1] = 1 + Math.min(map[i][j], map[i + 1][j], map[i][j + 1])
            }
        }
    }
    return map[word1.length][word2.length]
};
```