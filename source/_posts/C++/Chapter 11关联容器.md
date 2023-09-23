---
title: 第十一章 关联容器
date: 2020-09-04 16:45:11
tags: C++ Primer
categories: C++ 
---

## 第十一章 关联容器

```c++
按关键字有序保存元素主线
map						// 键值对
set						// 关键字
multimap				// 关键字可重复出现
multiset				// 关键字可重复出现
无序集合
unordered_map			// 用哈希函数组织的map
unordered_set
unordered_multimap
unordered_multiset
```

<!-- more -->

### 11.1 使用关联容器

```c++
while(cin >> word)
    // 只统计不再exclude中的单词
    if(exclude.find(word) == exclude.end())
        ++word_count[word];
```

### 11.2 关联容器概述

- 关联容器的迭代器都是双向的

#### 11.2.1 定义关联容器

#### 11.2.2 关键字类型的要求

- 必须有<运算符才能使用关联容器
- 自定义<必须满足严格弱序

```c++
// 使用自定义操作
multiset<Sales_data, decltype(compareIsbn)*> bookstore(compareIsbn);
// 用compareIsbn来初始化bookstore对象，表示当我们向bookstore添加元素时，通过调用compareIsbn来为这些元素排序。
```

####  11.2.3 pair类型

```c++
pair<T1, T2> p;
pair<T1, T2> p(v1, v2);
make_pair(v1, v2);			// pair的类型从v1和v2类型推断出来
p1 relop p2					// 关系运算符
```

```c++
pair<string, int> process(vector<string>& v)
{
    return make_pair(v.back(), v.back().size());
}
```

### 11.3 关联容器操作

- 关联容器额外的类型别名

```c++
key_type			// 关键字类型
mapped_type			// 关键字关联的类型
value_type			// 对于set，与key_type相同，对于map，
    				// 为pair<const key_type,mapped_type>
```

#### 11.3.1 关联容器迭代器

```c++
// 获得指向word_count中一个元素的迭代器
auto map_it = word_count.begin();
cout << map_it->first;			// 关键字
cout << map_it->second;			// 值
map_it->first = "new key";		// 错误：关键字是const的
__map_it->second;				// 正确
```

#### 11.3.2 添加元素

- 插入一个已存在的元素对set、map没有影响

```c++
set.insert(ivec.begin(), ivec.end());
set.insert({1, 3, 5, 7});
c.emplace(args)
```

- **insert和emplace返回一个pair**，first成员是一个迭代器，指向具有给定关键字的元素；second成员是一个bool值，指出元素是否成功插入。

- 向multiset或multimap添加元素

  **insert返回一个指向新元素的迭代器**

#### 11.3.3 删除元素

- erase接受一个key_type参数，删除所有匹配给定关键字的元素，**返回实际删除的元素的数量**。

```c++
c.erase(k)
c.erase(p)				// 删除迭代器p指定的元素
c.erase(b, e)			// 删除范围中元素
```

#### 11.3.4 map的下标操作

- **如果关键字不在map中，会创建一个元素插入到map中**

```c++
c[k]					// k不在c中，添加k
c.at(k)					// k不在c中，抛出out_of_range异常
```

- 当对一个map进行下标操作时，会获得一个mapped_type对象

#### 11.3.5 访问元素

```c++
lower_bound和upper_bound不适用于无需容器
c.find(k)				
c.count(k)				// 返回值永远是0或1
c.lower_bound(k)		// 第一个关键字不小于k的元素
c.upper_bound(k)		// 第一个关键字大于k的元素
c.equal_range(k)		// 返回pair，表示关键字等于k的元素范围
```

- 如果一个multi中有多个元素具有给定关键字，这些元素在容器中会相邻存储

```c++
string search_item("A");
auto entries = authors.count(search_iterm);
auto iter = authors.find(search_item);
while(ehtries) {
    cout << iter->second << endl;
    ++iter;
    --entries;
}
```

- lower_bound和upper_bound会得到一个迭代器范围

```c++
for(auto beg = authors.lower_bound(search_item), 
   		  end = authors.upper_bound(search_item);
   	beg != end; ++beg)
    cout << beg->second << endl;
```

- equal_range返回一个迭代器pair，表示匹配元素范围

```c++
for(auto pos = authors.equal_range(search_item);
   	pos.first != pos.second; pos.first++)
    cout << pos.first->second << endl;
```

#### 11.3.6 一个单词转换的map

```c++
void word_transform(ifstream& map, ifstream& input);
map<string, string>buildMap(ifstream& map);
const string& transform(const string s, const map<string, string>& m);
```

### 11.4 无序容器

- 无序容器在存储在组织为一组桶，**使用一个哈希函数将元素映射到桶。**无序容器的性能依赖于哈希函数的质量和桶的数量和大小。

- 桶接口

```c++
c.bucket_count()			// 正在使用的桶的数目
c.max_bucket_count()		// 容器能容纳的最多的桶的数量
c.bucket_size(n)			// 第n个桶中有多少个元素
c.bucket(k)					// 关键字为k的元素在哪个桶中
```

- 桶迭代

```c++
local_iterator				// 可以用来访问桶中元素的迭代器类型
const_local_iterator
```

- 哈希策略

```c++
c.load_factor()				// 每个桶的平均元素数量，返回float
c.max_load_factor()			// 试图维护的平均桶大小，返回float
c.rehash(n)					// 重组存储，使得bucket_count>=n
    				// 且bucket_count>size/max_load_factor
c.reserve(n)				// 重组存储，使得c可以保存n个元素
```

- 无序容器对关键字类型的要求

  默认情况下， **无序容器使用关键字类型的==运算符来比较元素**，它们还使用一个hash<key_type>类型的对象来生成每个元素的哈希值。

```c++
size_t hasher(const Sales_data &sd)
{ return hash<string>()(sd.isbn());}
bool qaOp(const Sales_data& lhs, const Sales_data& rhs)
{ return lhs.isbn() == rhs.isbn(); }

using SD_multiset = unordered_multiset<Sales_data, decltype(hasher)*, decltype(eqOp)*>;
// 参数是桶大小，哈希函数指针和相等性判断运算符指针
SD_multiset bookstore(42, hasher, eqOp);
```

- 如果类定义了==运算符，则可以只重载哈希函数：

```c++
unordered_set<f, decltype(fHash)*> fSet(10, fHash);
```







