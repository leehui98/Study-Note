STL（标准模板库），泛型编程，templete

level0：使用C++标准库

level1：深入认识C++标准库

level2：良好使用C++标准库

level3：扩充C++标准库（可选）



C++标准库不需要带.h，如#include\<vecor>

新式C head file不带.h，如#include\<cstdio>



网站：cplusplus.com   cppreference.com   gcc.gnu.org



C++六大部件：容器、分配器、算法、迭代器、适配器、仿函数

分配器支持容器，迭代器（泛化的指针）拿出来容器的元素，算法操作元素，适配器转换，仿函数类似函数。



容器分类：序列式、关联式、无序容器

序列式：array（不能扩充）、vector（成倍扩充）、deqeue（双向扩充）、list（双向不连续）、forward-list（单向不连续）

关联式：set、multiset（key就是value）、map、multimap（key-value式）。底层式红黑树。

无序容器：unordered_set/multiset，unordered_map/multimap。底层式hashtable

容器种类：array、vector、deque、list、set、multiset、map、multimap、unordered_set、unordered_multiset、unordered_map、unordered_multimap



分配器：allocator。

种类：

<ext/array_allocator.h>

<ext/mt_allocator.h> 多线程

<ext/debug_allocator.h> 调试

<ext/pool_allocator.h> 内存池

<ext/bitmap_allocator.h>

<ext/malloc_allocator.h>

<ext/new_allocator.h>

在命名空间__gun_cxx里面，如list\<string,\_\_gun_cxx::malloc_allocator\<string>> lis;



sort的条件：需要传入随机指针，即可以加减

list不能使用全局的sort，所以他有自己的sort



泛化 

```cpp
templete <typename T>
class _type_traits{
    
};
```

特化

```cpp
templete<> class type_traits<int>{
    
};
```

前闭后开区间：begin指向第一个起点，end指向最后一个元素的下一个位置。容器不一定是连续区间。

```cpp
Container<T> c;
Container<T>::iterator ite=c.begin();
for(;ite!=c.end();++ite) ...
ite可以做* ->符号
```

```cpp
for(decl:coll){
    statement;
}
for(int i:{2,3,4,5,6})
    std::cout<<i<<endl;
for(auto elem:vec){}
for(auto& elem:vec){elem*=3;}
```

```cpp
auto ite=::find(c.begin(),c.end(),target);
```

# 1. 容器

分类：序列式、关联式（key-value）、unordered Containers（不定序容器，hash_table）

## 1.1 序列式，按放进去的次序排列

### 1.1.1 Array

连续的空间，没办法扩充

### 1.1.2 vector

连续空间，可以自动扩充

### 1.1.3 Deque

连续空间，双向队列，两端可进出

### 1.1.4 List

链表，双向链表，双向环装链表

### 1.1.5 Forward-List

单向链表

## 1.2 关联式

### 1.2.1 Set/Multiset

底部是红黑树，高度平衡红黑树，key就是value

set不能重复

Multiset元素可以重复

### 1.2.2 Map/Multimap

key-value，

Map不能重复

Multimap能重复

## 1.3 Unordered Containers，也相当于是关联式

分为：Unordered Set/Multiset、Unordered Map/Multimap

元素分散，hashtable实现，hashtable的Separate Chaining，即碰撞之后放一个篮子里面，篮子是一个链表。    

 clock()，程序运行开始到现在开始的ms数



vector的的空间不够是两倍增长  

deque两边扩充，每次扩充一个buffer 

deque支持queue、stack

# 2. 分配器

 operator new() 和 malloc()。new 调用 malloc

分配512ints

```cpp
int *p=allocator<int>().allocate(512,(int*)0);
allocator<int>().deallocate(p,512);
```