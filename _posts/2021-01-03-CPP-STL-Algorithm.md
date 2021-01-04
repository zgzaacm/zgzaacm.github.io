---
layout: post
title: 'C++:STL-algorithm总结'
date: 2021-01-03
author: zgzaacm
color: rgb(131,175,155)
tags: C++ STL
---
> 教程参考[105 STL Algorithms in Less Than an Hour](https://www.youtube.com/watch?v=bFSnXNIsK4A&t=2771s)

**目录**

- [Permutations(元素排列)](#permutations元素排列)
  - [Heap](#heap)
    - [make_heap](#make_heap)
    - [push_heap](#push_heap)
    - [pop_heap](#pop_heap)
  - [Sort](#sort)
    - [sort](#sort-1)
    - [partial_sort](#partial_sort)
    - [nth_element](#nth_element)
    - [inplace_merge](#inplace_merge)
    - [merge](#merge)
  - [Partition](#partition)
    - [partition](#partition-1)
    - [partition_point](#partition_point)
  - [Other](#other)
    - [rotate](#rotate)
    - [shuffle](#shuffle)
    - [next_permutation](#next_permutation)
    - [prev_permutation](#prev_permutation)
    - [reverse](#reverse)

## Permutations(元素排列)

### Heap

> In C++'s HEAPS, the parent always **bigger** than child.

#### make_heap

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

#### push_heap

需要首先用`push_back`推入一个元素，在用`push_heap`将这个元素按照特定方式放到该放的位置。

```cpp
q.push_back(-1);
push_heap(q.begin(), q.end(), less<int>());
```

#### pop_heap

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

### Sort

#### sort

排序，默认从小到大（与堆相反），第三个参数可以自定义cmp函数，可以使用functional库的less或greater（降序），也可以在重载类的<运算符。

```cpp
sort(q.begin(), q.end(), cmp);
```

#### partial_sort

部分排序。四个参数，第二个参数是排序到哪里，其余跟sort一样。

```cpp
partial_sort(std::begin(numbers), std::begin(numbers) + count, std:: end (numbers) , std::greater<>());
```

#### nth_element

四个参数，第二个参数是nth，指定特定的位置。该位置之前的元素将不大于nth，之后的元素将不小于他。前后顺序并不一定。

```cpp
nth_element(iarray,iarray+6,iarray+len);
```

#### inplace_merge

可以合并同一个序列中两个连续有序的元素序列。它有三个参数： first、second、last 和 last 是一个双向迭代器。这个序列中的第一个输入序列是 [first，second)， 第二个输入序列是 [second,last)，因而 second 指向的元素在第二个输入序列中。

```cpp
std:inplace_merge(first,second,last,op);
```

#### merge

合并两个序列到一个序列，六个参数。分别是第一个序列begin，end，第二个的begin，end，存储结果的序列的begin，以及比较函数。返回指向存储结果序列尾部的迭代器。

```cpp
std::merge(std::begin(these), std::end(these),std::begin(those), std::end(those),std::begin (result), std::greater<>());
```

### Partition

#### partition

std::partition会将区间[first,last)中的元素重新排列，满足判断条件pred的元素会被放在区间的前段，不满足pred的元素会被放在区间的后段。该算法不能保证元素的初始相对位置，如果需要保证初始相对位置，应该使用stable_partition.

```cpp
bool IsOdd (int i) { //用语判断奇数
    return (i%2)==1; //奇数返回true，偶数返回0
}

int main(void)
{
    std::vector<int> ivec;  
    for (int i=1; i<10; ++i) 
        ivec.push_back(i); // 1 2 3 4 5 6 7 8 9 
    auto bound = partition (ivec.begin(), ivec.end(), IsOdd);   
    std::cout << "odd elements:";
    for (auto it=ivec.begin(); it!=bound; ++it)
        std::cout << ' ' << *it;       //输出1,9,3,7,5    
    std::cout << "even elements:";
    for (auto it=bound; it!=ivec.end(); ++it)
        std::cout << ' ' << *it;     //输出6,4,8,2    
    return 0;
}
```

#### partition_point

在已分好组的数据中找到分界位置，返回第一个不负责规则的元素的迭代器。

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::partition_point
#include <vector>       // std::vector
using namespace std;
//以普通函数的方式定义筛选规则
bool mycomp(int i) { return (i % 2) == 0; }
//以函数对象的形式定义筛选规则
class mycomp2 {
public:
    bool operator()(const int& i) {
        return (i % 2 == 0);
    }
};
int main() {
    vector<int> myvector{ 2,4,6,8,1,3,5,7,9 };
    //根据 mycomp 规则，为 myvector 容器中的数据找出分界
    vector<int>::iterator iter = partition_point(myvector.begin(), myvector.end(),mycomp);
    //输出第一组的数据
    for (auto it = myvector.begin(); it != iter; ++it) {
        cout << *it << " ";
    }
    cout << "\n";
    //输出第二组的数据
    for (auto it = iter; it != myvector.end(); ++it) {
        cout << *it << " ";
    }
    cout << "\n*iter = " << *iter;
    return 0;
}
```

### Other

#### rotate

rotate() 的第一个参数是这个序列的开始迭代器；第二个参数是指向新的第一个元素的迭代器，它必定在序列之内。第三个参数是这个序列的结束迭代器。图 1 中的示例说明在容器 ns 上的旋转操作使值为 4 的元素成为新的第一个元素，最后一个元素的值为 3。元素的圆形序列会被维持，因此可以有效地旋转元素环，直到新的第一个元素成为序列的开始。这个算法会返回一个迭代器，它指向原始的第一个元素所在的新位置。

```cpp
std::vector<string> words { "one", "two", "three", "four", "five","six", "seven", "eight"};
auto iter = std::rotate(std::begin(words), std::begin(words)+3, std::end(words));
```

#### shuffle

使用随机生成器g对元素[first, last)可行交换的容器内部元素进行随机排列。第三个参数为随机数生成器实例，在<random>中定义，

```cpp

#include <iostream>
#include <vector>
#include <algorithm> // std::move_backward
#include <random> // std::default_random_engine
#include <chrono> // std::chrono::system_clock
 
int main (int argc, char* argv[])
{
    std::vector<int> v;
 
    for (int i = 0; i < 10; ++i) {
        v.push_back (i);
    }
 
    // obtain a time-based seed:
    unsigned seed = std::chrono::system_clock::now ().time_since_epoch ().count ();
    std::shuffle (v.begin (), v.end (), std::default_random_engine (seed));
 
    for (auto& it : v) {
        std::cout << it << " ";
    }
 
    std::cout << "\n";
 
    return 0;
}
```

#### next_permutation

对于next_permutation函数，其函数原型为：

```cpp
#include <algorithm>
bool next_permutation(iterator start,iterator end)
```

当前序列不存在下一个排列时，函数返回false，否则返回true

#### prev_permutation

同理。

#### reverse

反转序列。

> *未完待续*
