---
layout: post
title: 'C++:函数对象'
date: 2021-01-04
author: zgzaacm
color: rgb(154,133,255)
tags: C++
---
> 转载自[blog](https://blog.csdn.net/u014709760/article/details/89323988)

- [什么是函数对象](#什么是函数对象)
- [重载了()运算符的类对象例子](#重载了运算符的类对象例子)
- [重温的原因](#重温的原因)

## 什么是函数对象

- 函数名
- 指向函数的指针
- 重载了()运算符的类对象

## 重载了()运算符的类对象例子

```cpp
#include <iostream>
using namespace std;

class Linear{

private:
	double slope;
	double y0;
public:
	//构造函数
	Linear(double sl_ = 1,double y_ = 0):slope(sl_),y0(y_){}
	//重载()运算符
	double operator()(double x)
	{
		return y0 +slope*x;
	}
};

int main()
{
	Linear f1;
	Linear f2(2.5,10.0);
	
	//在此处Linear类的对象 f1和f2利用重载的（）运算符以函数的方式实现了 y0 +slope*x 功能
	//因此 f1和f2 可以成为函数对象（或函数符）
	double y1 = f1(12.5);
	double y2 = f2(0.4);

	cout<<"y1: "<< y1 <<endl;
	cout<<"y2: "<< y2 <<endl;

	return 0;
}
```

## 重温的原因

学习STL-algorithm时，很多函数的参数经常可以由一个类的实例来表示，之前只是一知半解，搞懂后来这里记录一下。
