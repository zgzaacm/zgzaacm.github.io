---
layout: post
title: 'C++:STL-algorithm总结'
date: 2021-01-03
author: zgzaacm
color: rgb(131,175,155)
tags: C++ STL
---
> 教程参考[105 STL Algorithms in Less Than an Hour](https://www.youtube.com/watch?v=bFSnXNIsK4A&t=2771s)

## 1.HEAPS

> In C++'s HEAPS, the parent always **bigger** than child.

#### a.make_heap

第一个参数是指向开始元素的迭代器，第二个参数是指向最末尾元素的迭代器，第三个参数是less<>()或是greater<>()，前者用于生成大顶堆，后者用于生成小顶堆，第三个参数默认情况下为less<>()，less<int>()用于生成大顶堆。
> 要使用less<int>()，以及greater<int>()，请添加头文件#include <functional>，且一定要加括号less<>()
> 会直接在迭代器身上进行改变 无需重新赋值

```cpp
int main(){
    vector<int> q;
    for (int i = 0; i < 10; i++) {
        q.push_back(i);
    }
    make_heap(q.begin(), q.end(),less<int>());
    for (int i = 0; i < q.size(); i++) {
        cout << q[i] << " ";
    }
    return 0;
}
```

#### b.push_heap

需要首先用`push_back`推入一个元素，在用`push_heap`将这个元素按照特定方式放到该放的位置。

```cpp
q.push_back(-1);
push_heap(q.begin(), q.end(), less<int>());
```

#### c.pop_heap

将堆顶元素和最后一个元素交换，然后将除最后一个元素外所有元素按堆重建，之后还需要`pop_back`将其顶出。
> 注意 `pop_back`不会返回值，只会弹出。想要最后值使用back().

```cpp
pop_heap(q.begin(), q.end(), greater<int>());
q.pop_back();
// 还可以进行堆排序
pop_heap(q.begin(), q.end(), greater<int>());
pop_heap(q.begin(), q.end()-1, greater<int>());
pop_heap(q.begin(), q.end()-2, greater<int>());
```

**tip**:我们也可以自定义比较方式，只需要重构operator <即可。

```cpp
struct test{
    int a;
    string c;
    bool operator < (const test b){
        if (this->c <= b.c)return true;
        else return this->a <= b.a;
    }
};

int main(){
    vector<test> q;
    for(int i=0;i<5;i++){
        test t;
        cin>>t.a>>t.c;
        q.push_back(t);
    }
    make_heap(q.begin(), q.end());
    return 0;
}
```

> *未完待续*
