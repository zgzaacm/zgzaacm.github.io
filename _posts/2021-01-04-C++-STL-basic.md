---
layout: post
title: 'C++:STL基本入门'
date: 2021-01-04
author: zgzaacm
color: rgb(131,175,155)
tags: C++ STL
cover: '../assets/c++.jfif'
---

> 笔记总结自[C++ Standard Library](https://www.youtube.com/watch?v=Vc1RyqWFbiA&list=PL5jc9xFGsL8G3y3ywuFSvOuNm3GjBwdkb&index=1&t=11s)。本教程言简意赅，适用于有良好C++基础想快速入门STL的人。

- [Template](#template)
  - [function template](#function-template)
  - [class template](#class-template)
- [概括](#概括)

## Template

能够熟练使用STL的前提，一定是要对template的基本使用熟练掌握。

template：一份代码，多种用途。

### function template

```cpp
template <typename T>
T square(T x) {
    return x * x;
}
```

如上，是函数模板的定义方式，我们可以显示告诉编译器调用的类型，也可以让编译器自己推测。

```cpp
square<int>(5)
square(5.5)
```

template的副作用`code bloat`是指每个数据类型对应的模板编译后会各有自己的代码，当时用数据类型很多时会造成代码膨胀。

### class template

```cpp
template <typename T>
class BoVector {
    T arr[100];
    int size;
public:
    BoVector():size(0) {}
    void push(T x) { arr[size] = x; size++; }
    void print() const { for(int i=0; i<size ; i++) {cout << arr[i] << endl;}}
}
```

> 关于const:非静态成员函数后面加const（加到非成员函数或静态成员后面会产生编译错误），表示成员函数隐含传入的this指针为const指针，决定了在该成员函数中，任意修改它所在的类的成员的操作都是不允许的（因为隐含了对this指针的const引用）；唯一的例外是对于mutable修饰的成员。加了const的成员函数可以被非const对象和const对象调用，但不加const的成员函数只能被非const对象调用。

类模板并不能自动推断类型。

## 概括

STL:Standard Template Library。是C++标准库的核心。内部有Algorithm和Containers。有趣的是，STL与传统OOP背道而驰，将算法和数据结构分离的很好，目的是为了用户的自定义以及两者可以相互复用。

Algorithm和Containers相互沟通的桥梁就是Iterators，Iterator可以遍历Containers的每一个item，提供了高级抽象的接口。

```cpp
using namespace std;
```

STL中一切对象都在std这个namespace中定义。所以使用std前要引用这句话。之后的代码会默认这句话的存在。

一个基本使用的例子：

```cpp
#include<vector>
vector<int> vec;
vec.push_back(3);
vec.push_back(4);
vector<int>::iterator itr1 = vec.begin();
vector<int>::iterator itr2 = vec.end();
for (auto itr = itr1; itr!=itr2; ++itr) cout<< *itr << endl;
```

`vector<int>`是一种容器，`vector<int>::iterator`可以定义该容器的`iterator`类型。可以看出迭代器的使用方式和指针类似。