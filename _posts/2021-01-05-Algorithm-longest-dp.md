---
layout: post
title: '算法：回文串之中心扩散和manacher'
date: 2021-01-05
author: zgzaacm
color: rgb(154,133,255)
tags: algorithm manacher
---

## 问题概述

给定一个字符串S, 找到最长回文子串并返回。

## 中心扩散法

之前只知道写$O(N^2)$空间复杂度的dp，没想到还有复杂度$O(1)$的方法。具体思想就是任意回文串中心只有$O(N)$种可能，分奇偶遍历中心依次展开即可。

代码：

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        start, end = 0, 1
        for i in range(len(s)-1):
            l1, r1 = self.center(i, i, s) # 奇数串
            l2, r2 = self.center(i, i+1, s) # 偶数串
            if r1 - l1 > end - start:
                start, end = l1, r1
            if r2 - l2 > end - start:
                start, end = l2, r2
        return s[start:end]
    
    def center(self, l, r, s: str) -> Tuple[int, int]:
        if s[l] != s[r]:
            return l, r
        while l > 0 and r < len(s) - 1 and s[l-1] == s[r+1]:
            l-=1
            r+=1
        return l, r + 1
```

## Manacher算法

很久之前专心搞算法时就想学习马拉车这个算法，这次就完成之前的遗憾。

> 参考[Longest Palindromic Substring Manacher's Algorithm](https://www.youtube.com/watch?v=nbTSfrEfo6M)

本质上也是用中心扩散法，但如同KMP，利用了之前串的信息。假设之前的回文串最远右边界为R，如果当前判断位置在边界内部的话，可以找到镜像位置对应的长度。如果长度超过了边界，则直接取当前位置离右边界的距离。可以从当前长度继续扩张边界。因为右边界线性增长，所以时间复杂度是$O(N)$。

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # 预处理 这样不用考虑偶串中心问题
        snew = '#'
        for ch in s:
            snew = snew + ch + '#'
        # 加上开头结尾 不用考虑边界问题
        snew = '[' + snew + ']'
        
        maxlen, maxdex = 0, 0
        p = [0 for i in range(len(snew))] #记录以i为中心 左右臂长长度
        c, r = 1, 1
        for i in range(1, len(snew) - 1):
            mirr = 2 * c - i #找到镜像位置
            # 如果在边界内
            if mirr < r:
                p[i] = min(p[mirr], r - i)
            # 扩张
            while snew[i+1+p[i]] == snew[i-1-p[i]]:
                p[i] += 1
            # 更新边界
            if i + p[i] > r:
                c, r = i, i + p[i]
            # 更新最大值
            if p[i] > maxlen:
                maxlen, maxdex = p[i], i
    
        # 变回原来的串
        return ''.join(filter(lambda x:x!='#', snew[maxdex-maxlen:maxdex+maxlen+1]))
    
```
