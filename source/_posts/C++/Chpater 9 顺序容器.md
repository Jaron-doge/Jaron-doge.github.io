---
title: 第九章 顺序容器
date: 2020-09-04 16:45:09
tags: C++ Primer
categories: C++ 
---

## 第九章 顺序容器

### 9.1 顺序容器概述

```c++
vector			// 可变大小数组，在尾部之外的位置修改元素可能很慢
deque			// 双端队列，在头尾修改速度很快
list			// 双向链表，只支持双向顺序访问，在任何位置修改都很快
forward_list	// 单向链表，支持单项顺序访问，在任何位置修改都很快
array			// 固定大小数组
string			// 与vector相似的容器，专门用于保存字符
```

<!-- more -->

### 9.2 容器库概览

#### 9.2.1 迭代器

- 迭代器范围为左闭合区间

#### 9.2.2 容器定义与初始化

```c++
C c;
C c1(c2);
C c{a, b, c};
C c(begin, end);
C seq(n);		// n个元素
C seq(n, t);	// n个初始值为t的元素
```

- 当传递迭代器参数来拷贝一个范围时，不要求容器类型是相同的，元素类型需可互相转化。

- 标准库array的大小是类型的一部分

```c++
array<int, 10>;
```

#### 9.2.5 赋值和swap

1. assign操作

```c++
seq.assign(begin, end);
```

2. swap操作

   swap交换两个相同类型容器的内容。

#### 9.2.6 容器大小操作

- size、empty、max_size

#### 9.2.7 关系运算符

- 只有当元素类型定义了相应的比较运算符时，才能使用关系运算符来比较两个容

  器

### 9.3 顺序容器操作

#### 9.3.1 向顺序容器添加元素

1. push_back、push_front、insert

2. emplace操作

   在调用emplace时，会在容器管理的内存空间中直接创建对象。而调用push则会创建一个局部临时对象，并将其压入容器中。

```c++
c.emplace_back("978-0590353403", 25, 15.99);
c.push_back(Sales_data("978-0590353403", 25, 15.99));
```

#### 9.3.2 访问元素

- front和back返回首尾元素的引用

- at返回下标为n的元素引用

#### 9.3.3 删除元素

- pop_back、pop_front、erase

- erase返回指向删除的元素之后位置的迭代器

#### 9.3.4 特殊的forward_list操作

```c++
before_begin()		// 返回首元素之前不存在的元素的迭代器
insert_after(p)		// p后插入
emplace_after(p,a)	// p后插入
lst.erase_after(p)	// p后删除
```

#### 9.3.5 改变容器大小

- resize

```c++
c.resize(n)			// 调整c的大小为n个元素，少增多删
c.resize(n, t)		// 带初始值	
```

#### 9.3.6 容器操作可能使迭代器失效

- 循环中更新迭代器

### 9.4 vector对象是如何增长的

- 适用于vector和string

```c++
c.shrink_to_fit()		// 请将capacity()减少为与size()相同大小
c.capacity()			// 不重新分配内存空间，c可保存的元素
c.reserve(n)			// 分配至少能容纳n个元素的内存空间
c.resize(n)				// resize既分配空间又分配对象
```

![image-20200814135745978.png](https://i.loli.net/2020/09/04/ye2oiVtGglhkb8B.png)

### 9.5 额外的string操作

#### 9.5.1 构造string的其他方法

```c++
string s(cp, n)		// s是cp指向的数组中前n个字符的拷贝
s.substr(pos, n)	// 返回一个从pos开始n个字符的拷贝
```

#### 9.5.2 改变string的其他方法

```c++
// 在s[0]之前插入s2中s2[0]开始的s2.size()个字符
s.insert(0, s2, 0, s2,size());	
s.append("4th");			// 末尾追加
s.replace(11, 3, "5th");	// 先删除3个字符再插入
```

#### 9.5.3 string搜索操作

- 均返回size_type

```c++
s.find()				// 第一次出现位置
s.find_first_of()		// 任何一个args中字符第一次出现位置
s.find_first_not_of()	// 第一个不在args中的字符
```

#### 9.5.4 compare函数

```c++
s.compare(pos1, n1, s2, pos2, n2)
s.compare(pos1, n1, cp, n2)
```

#### 9.5.5 数值转换

- 要转换为数值的string中第一个非空白符必须是数值中可能出现的字符

```c++
d = stod(s2.substr(s2.find_first_of("+-.0123456789")));
to_string(val)		// val可以是任何算术类型
stoi(s, p, b)		// b为转换基数，默认10，p默认0
```

### 9.6 容器适配器

- stack默认基于deque实现

```c++
s.pop()				// 删除栈顶元素，不返回值
s.push()			// 压入栈顶
s.top()				// 返回栈顶元素
```

- queue默认基于deque实现，priority_queue默认基于vector实现

```c++
q.pop()				// 不删除
q.back()			// 不删除
q.top()				// 只适用于priority_queue
q.push()			// 末尾
```

