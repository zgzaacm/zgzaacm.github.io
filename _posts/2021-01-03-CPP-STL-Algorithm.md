---
layout: post
title: 'C++:STL-algorithm总结'
date: 2021-01-03
author: zgzaacm
color: rgb(100,210,32)
tags: C++ STL
---
> 教程参考[105 STL Algorithms in Less Than an Hour](https://www.youtube.com/watch?v=bFSnXNIsK4A&t=2771s)

## 1.HEAPS
> In C++'s HEAPS, the parent always **bigger** than child.

### a.make_heap
第一个参数是指向开始元素的迭代器，第二个参数是指向最末尾元素的迭代器，第三个参数是less<>()或是greater<>()，前者用于生成大顶堆，后者用于生成小顶堆，第三个参数默认情况下为less<>()，less<int>()用于生成大顶堆。
> 要使用less<int>()，以及greater<int>()，请添加头文件#include <functional>，且一定要加括号less<>()
> 会直接在迭代器身上进行改变 无需重新赋值
```cpp
make_heap(q.begin(), q.end(),less<int>());
```