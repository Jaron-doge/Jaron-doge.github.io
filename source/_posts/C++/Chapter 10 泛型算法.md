---
title: 第十章 泛型算法
date: 2020-09-04 16:45:10
tags: C++ Primer
categories: C++ 
---

## 第十章 泛型算法

### 10.1 概述

### 10.2 初始泛型算法

#### 10.2.1 只读算法

<!-- more -->

```c++
find()
count()
accumulate(begin, end, 0)	  // 序列中元素类型必须与第三个参数匹配
equal(begin1, end1, begin2)	  // 不能比较2个c-style strings
```

#### 10.2.2 写容器元素的算法

```c++
fill(b, e, t)
fill_n(b, n, t)
fill_n(back_inserter(vec), 10, 0)
```

- 拷贝算法

  保留原序列，增加第三个迭代器参数指出调整后序列的保存位置

```c++
replace_copy(ilist.cbegin(), ilist.cend(), back_inserter(ivec), 0, 42);
```

#### 10.2.3 重排容器元素的算法

```c++
sort(b, e)
unique(b, e)
```

### 10.3 定制操作

#### 10.3.1 向算法传递函数

- 谓词

  谓词是一个可调用的表达式，其返回结果是一个能用作条件的值。

```c++
bool isShorter(const string& s1, const string& s2)
{return s1.size() < s2.size();}
// 按长度排序
sort(words.begin(), words,end(), isShorter);
```

- 排序算法

```c++
stable_sort(b, e, isShorter)	// 保持等长元素间的字典序
```

#### 10.3.2 lambda表达式

```c++
[capture list](parameter list) -> return type{function body}
```

- capture list是一个lambda所在函数中定义的局部变量的列表（通常为空）
- lambda必须使用尾置返回

- 可以忽略参数列表和返回类型，但必须永远包含捕获列表和函数体
- **如果lambda的函数体包含任何单一return语句之外的内容，且未指定返回类型，则返回void**

```c++
auto f = [] {return 42;};
cout << f() << endl;		// 打印42
```

- 向lambda传递参数
- 只对lambda所在函数中定义的(非static)变量使用捕获列表

```c++
[sz](const string& a)
{return a.size() >= sz;};
```

#### 10.3.3 lambda捕获和返回

- 被捕获的变量的值是在lambda创建时拷贝

- 引用捕获

```c++
[&os](const string& s){os << s ;};
```

- 隐式捕获

  &: 引用捕获

  =: 值捕获

```c++
// os隐式捕获，引用捕获方式；c显示捕获，值捕获方式
[&, c](const string& s) {os << s << c;};
```

- 可变lambda

  在参数列表首加上关键字mutable可以改变被捕获变量的值

```c++
auto f = [v1]()mutable{return ++v1;};
auto f2 = [&v1](){return ++v1;};
v1 = 0;
f() = 43; f2() = 1;
```

- 返回类型

  当我们需要为一个lambda定义返回类型时，必须使用尾置返回类型

```c++
transform(vi.begin(), vi.end(), vi.begin(), 
          [](int i) -> int 
          {if(i < 0) return -i; else return i;});
```

#### 10.3.4 参数绑定

- bind

```c++
auto newCallable = bind(callable, arg_list);

bool check_size(const string& s, string::size_type sz)
{return s.size() >= sz;}
// 将第二个参数绑定到sz
auto check6 = bind(check_size, _1, sz);
```

- 绑定多个参数

```c++
auto g = bind(f, a, b, _2, c, _1);
g(X, Y) -> f(a, b, Y, c, X)
```

- 绑定引用参数

  不能直接用bind来代替对os的捕获，必须使用标准库**ref**函数

```c++
bind(print, ref(os), _1, ' ');
```

​		函数ref返回一个对象，包含给定的引用，此对象是可以拷贝的。

### 10.4 再探迭代器

- 插入迭代器、流迭代器、反向迭代器、移动迭代器

#### 10.4.1 插入迭代器

- back_inserter创建一个使用push_back的迭代器
- front_inserter创建一个使用push_front的迭代器
- inserter创建一个使用insert的迭代器

```c++
list<int> lst = {1, 2, 3, 4};
list<int> lst2, lst3;
copy(lst.b, lst.e, front_inserter(lst2));	// 4 3 2 1
copy(lst.b, lst.e, inserter(lst3, lst3.b));	// 1 2 3 4
```

#### 10.4.2 iostream迭代器

- istream_iterator操作

```c++
istream_iterator<int> in_iter(cin), eof;	// 从cin读取int
vector<int> vec(in_iter, eof);				
```

- 使用算法操作流迭代器

```c++
cout << accumulate(in, eof, 0) << endl;
```

- ostream_iterator操作

```c++
ostream_iterator<T> out(os, d);		// out将类型为T的值写到os中
for(auto e : vec)					// 打印
    *out++ = e;
copy(vec.b, vec.e, out);
```

#### 10.4.3 反向迭代器

```c++
r.base();							// 返回对应的普通迭代器
```

### 10.5 泛型算法结构

- 输入迭代器、输出迭代器、前向迭代器、双向迭代器、随机访问迭代器

#### 10.5.1 5类迭代器

- 输入迭代器

  必须支持==、!=、++、*、->运算符

- 输出迭代器

  必须支持++、*运算符

#### 10.5.2 算法形参模式

```c++
alg(beg, end, other args);
alg(beg, end, dest, other args);	
alg(beg, end, beg2, other args);
alg(beg, end, beg2, end2, other args);
```

- dest通常被绑定到一个插入迭代器或是一个ostream_iterator

#### 10.5.3 算法命名规范

- 一些算法使用重载形式传递一个谓词
- 接受一个元素值的算法通常有另一个不同名的（不是重载的）版本，**接收谓词参数的算法都有附加的_if前缀**

- **将元素写到额外目的空间的算法都在名字后面附加一个_copy**
- 一些算法同时提供_copy和 _if版本

```c++
// 将偶数元素从v1拷贝到v2；v1不变
remove_copy_if(v1.begin(), b1.end(), back_inserter(v2), [](int i){return i % 2;});
```

### 10.6 特定容器算法

- list和forward_list成员函数版本的算法

```c++
lst.merge(lst2)			// 将lst2元素合并入lst中，使用<运算符，
lst.merge(lst2, comp)	// lst2变为空，接受谓词
    
lst.remove()			// 接受谓词
lst.reverse()
lst.sort()				// 接受谓词
lst.unique()			// 接受谓词
```

```c++
lst.splce(args)
flst.splice_after(args)
(p, lst2)				// p指向lst，将lst2元素移到p前，清空lst2
(p, lst2, p2)			// lst2可以是与lst相同的链表
(p, lst2, b, e)
```

