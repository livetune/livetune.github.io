---  
layout: post  
title: leetcode 串联所有单词的子串
categories: [algorithm] 
description:  串联所有子串
keywords: 算法  
---  

这一题一开始我是想直接将单词的所有排列组合列出来，再进行匹配，结果很明显就超时了。  
改成换成取字符串，因为单词的长度都是一样的，可以分割成和 words 里一样的数组，将words转成对象，value 是出现的次数，然后遍历 keys，减去字符串里出现的单词，如果最后所有的 key 都是 0，则将当前的索引加入结果。运行后的时间复杂度太大，虽然是 ac 了。
后来看题解好像可以使用滑动窗口的方式优化，运行速度马上就上去了
## 合并 串联所有单词的子串

### 描述

给定一个字符串 s 和一些长度相同的单词 words。找出 s 中恰好可以由 words 中所有单词串联形成的子串的起始位置。

注意子串要与 words 中的单词完全匹配，中间不能有其他字符，但不需要考虑 words 中单词串联的顺序。
```json
输入：
  s = "barfoothefoobarman",
  words = ["foo","bar"]
输出：[0,9]
解释：
从索引 0 和 9 开始的子串分别是 "barfoo" 和 "foobar" 。
输出的顺序不重要, [9,0] 也是有效答案。

```

### 解法 滑动窗口
![示例.001.jpeg](https://i.loli.net/2020/04/20/2OwiroAxpaYKRdF.jpg)
由于每个单词的长度是一样的，所以起点可以一个单词的每个字母的索引开始，遍历时每次递增一个单词的长度。  
需要有两个指针，左右各一个。每次右指针向后移动一位，并且记录下当前字符串里的单词出现次数。如果单词不再 words 里，或者超出了 words 里出现的次数，则左右指针都定位到右指针的下一位。  
使用 count 记录个数，当 count 和单词的长度相同时，记录索引，然后左指针向右移动一个单词的距离，同时减去在记录的 map 里数量。  
时间复杂度： 外层遍历一个单词 K，内层遍历 2 * N / K，左右指针移动的距离，每次递增一个单词的商都。O(K*2*N/K) => O(2*N) => O(N)
```javascript
/**
 * @param {string} s
 * @param {string[]} words
 * @return {number[]}
 */
var findSubstring = function (s, words) {
  if (!s || !words.length) {
    return []
  }
  const len = words[0].length
  const wordsLength = words.length * len
  const needLen = words.length
  const result = []
  words = words.reduce((r, v) => {
    r[v] ? r[v]++ : r[v] = 1
    return r
  }, {})
  for (let i = 0; i < len; i++) {
    let l = i, r = i, windows = {}, count = 0
    for (; r <= s.length - len;) {
      const key = s.slice(r, r + len)
      if (words[key] === undefined) {
        // 重新定位到右边的下一位
        r = l = r + len
        windows = {}
        count = 0
        continue
      } else if (words[key] === windows[key]) {
        // 左边向右移动一位
        windows[s.slice(l, l + len)]--
        l += len
        count--
        continue
      }

      windows[key] ? windows[key]++ : windows[key] = 1
      count++
      if (count === needLen) {
        result.push(l)
        // 左边向右移动一位
        windows[s.slice(l, l + len)]--
        l += len
        count--
      }
      r += len
    }
  }
  return result
};
```