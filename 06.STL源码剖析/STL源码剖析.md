C++标准库不需要带.h，如#include\<vecor>

新式C head file不带.h，如#include\<cstdio>



网站：cplusplus.com   cppreference.com   gcc.gnu.org



C++六大部件：**容器、分配器、算法、迭代器、适配器、仿函数**

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

```cpp
array<tyep,size> a;
array<int,asize> c;
c.front();//第一个元素
c.back();//最后一个元素
```



### 1.1.2 vector

连续空间，可以自动扩充

```cpp
vector<type> v;

vector<int> v;
cout<<v.capcity();//输出容量，扩容的时候两倍增长。寻找两倍的空间，然后把元素搬过去。
v.data();//vector的起始点
```



### 1.1.3 Deque

连续空间，双向队列，两端可进出，双向扩充，每次扩充一个buffer

有多个buffer，在push_front或者push_back的时候，如果buffer用完了，就在申请一个buffer放在前面或者后面。

### 1.1.4 List

链表，双向链表，双向环状链表。每次扩充一个结点。

list有一个sort函数

``` cpp
list<type> l;
```



### 1.1.5 Forward-List

每次扩充一个结点，单向链表

```cpp
forward_list<type> fl;
fl.push_front();//只有push_front，没有push_back 	
```

### 1.1.6 queue、stack

技术上叫做容器适配器

底层都是deque实现的



## 1.2 关联式

不能用下标操作符[]取元素

### 1.2.1 set/multiset

底部是红黑树，高度平衡红黑树，key就是value

set不能重复

Multiset元素可以重复

```cpp
set<int> s;
s.insert(1);
s.size();
if(s.find(1)!=s.end()) cout<<"found"<<endl;
```



### 1.2.2 map/multimap

key-value，底层是红黑树

Map不能重复

Multimap能重复

```cpp
map<long,string> memo;
memo.insert(pair<long,string>(1,"2"));
auto item=memo.find(1);
if(item!=memo.end())
{
    cout<<item->first<<" "<<item->second<<endl;
}

//map可以用[]赋值
//multimap不可以用[]赋值
```



## 1.3 Unordered Containers，也相当于是关联式

分为：Unordered Set/Multiset、Unordered Map/Multimap

元素分散，hashtable实现，hashtable的Separate Chaining，即碰撞之后放一个篮子里面，篮子是一个链表。    



 clock()，程序运行开始到现在开始的ms数

vector的的空间不够是两倍增长  

deque两边扩充，每次扩充一个buffer 

deque支持queue、stack

### 1.3.1 unordered_set/unordered_multiset

```cpp
unordered_multiset<string> c;
c.insert("hello");
c.bucker.size(1);//第1个篮子有多少个元素
//如果元素的个数大于等于篮子的个数，篮子就要重新扩充，大约两倍
auto item=c.find("hello");
if(item!=c.end()){
    cout<<*item<<endl;
}
```



### 1.3.2 unordered_map/unordered_multimap

```cpp
可以用[]赋值，也可以insert插入
```

## 其他

```cpp
priority_queue<int,vector<int>,less<int>> q;//优先队列，大根堆
```

**分配器**的类型有哪些：

```cpp
#include<ext\array_allocator.h>
array_allocator;
mt_allocator;//多线程
debug_alloctor;
pool_allocator;//内存池
bitmap_allocator;
malloc_allocator;
new_allocator;
list<string,allocator<string>> c1;
list<string,__gun_cxx::malloc_allocator<string>> c2;
```

# 2.STL

源代码：gun c++ 2.9

#include<bits/stdc++.h>

随机访问迭代器

泛化、特化。

![](./image/特化.png)

![](./image/特化1.png)

![](./image/特化2.png)

![](./image/特化3.png)

