---
layout: post
title: 'Python:基础文件操作'
date: 2021-01-06
author: zgzaacm
color: rgb(131,175,155)
tags: python
---

> 教程参考：[Python Tutorial: File Objects - Reading and Writing to Files](https://www.youtube.com/watch?v=Uh2ebFW8OYM&t=1181s)

## 前言

因为平时编程很少涉及文件操作，一直都是随用随查，这次想好好整理一下以便记忆和以后的使用。

## 文件操作

* 使用`open(path)`打开文件，默认可读。
* `open(path, 'r')`指明方式
* r可读 r+可读可写

```python
f = open('test.txt', r)

print(f.name, f.mode, f.closed)

f.close()
```

* 可以通过name属性查看文件名字
* mode:文件当前被打开的模式(r,w,...)
* closed:文件是否关闭
* 必须要close关闭

```python
with open('test.txt', 'r') as f:
    print(f.name, f.mode)
    f_contents = f.readlines()
```

* 使用Context manager，会自动关闭文件，就算出错
* f.read()默认一次性读入所有内容 适合小文件
* f.read(n)读入n 个字符，读过内容不会重复读入
* 当没有可以读的时候，read会返回空
* f.readlines()每一行为一个元素的列表
* f.readline()每调用一次读入一行
* f.tell()告诉你当前读了文件的几个字符
* f.seek(n)跳转到n字符的位置

```python
with open('test.txt', 'r') as f:
    for line in f:
        print(line)
```

* 更好的方式：直接遍历f，得到每一行

```python
with open('test2.txt', 'w') as f:
    f.write('test')
```

* 默认w会overwirte源文件。不想的话使用a-append
* write写入文件
* 也可以在写的过程使用seek进行移动位置
* with可以嵌套 边读边写

* 读入写入图片时需要使用二进制形式，默认采用utf-8
* 使用`rb`和`wb`
