---
title: 第十二章 动态内存
date: 2020-09-04 16:45:12
tags: C++ Primer
categories: C++ 
---

## 第十二章 动态内存

### 12.1 动态内存与智能指针

- 智能指针

  自动释放所指向的对象

<!-- more -->

```c++
#include <memory>
shared_ptr			// 多个指针指向同一个对象
unique_ptr			// “独占”所指向的对象
weak_ptr			// 弱引用，指向shared_ptr所管理的对象
```

#### 12.1.1 shared_ptr类

```c++
shared_ptr<string> p1;
```

- 如果在一个条件判断中使用智能指针，效果是检测其是否为空

```c++
p.get()				// 返回p中保存的指针
p.swap(q)			// 交换指针
make_share<T>(args)	// 返回一个shared_ptr
p.unique()			// 若p.use_count()为1返回true
p.use_count()		// 返回与p共享对象的智能指针数量
```

- 当进行拷贝或赋值操作时，每个shared_ptr都会记录有多少个其他shared_ptr指向相同的对象，**当计数器变为0**，它就会自动释放自己所管理的对象

- 程序使用动态内存的三个原因
  1. 程序不知道自己需要使用多少对象
  2. 程序不知道所需对象的准确类型
  3. 程序需要在多个对象间共享数据

#### 12.1.2 直接管理内存

- 使用new动态分配和初始化对象

  只有当括号中仅有一个单一初始化器时才可以使用auto

```c++
auto p1 = new auto(obj);			// 该对象用obj进行初始化
auto p2 = new auto{a, b, c};		// 错误
```

- 内存耗尽

  默认情况下，new分配失败会抛出bad_alloc异常

```c++
int *p1 = new int;
int *p2 = new (nothrow) int;	// 阻止异常抛出，返回空指针
```

- 释放动态内存

  delete表达式执行两个动作：销毁给定的指针指向的对象；释放对应的内存。

#### 12.1.3 shared_ptr和new结合使用

- 接受指针参数的智能指针构造函数是explicit的，不能将一个内置指针隐式转换为一个智能指针

```c++
shared_ptr<int> p1 = new int(1024);		// 错误
shared_ptr<int> p2(new int(1024));		// 正确
```

- **用来初始化智能指针的普通指针必须指向动态内存**

```c++
shared_ptr<T> p(p2, d)
p.reset()				// p是唯一指向其对象的s_p，释放此对象
p.reset(q)				// 传递了可选的参数内置指针q，p指向q
p.reset(q, d)			// 调用d而不是delete来释放q
```

- 当将一个shared_ptr绑定到一个普通指针时，我们就将内存的管理责任交给了这个shared_ptr。一旦这样做了，我们就不应该再使用内置指针来访问shared_ptr所指向的内存了。

- get返回一个内置指针，永远不要用get初始化另一个智能指针或者为另一个智能指针赋值。

- 在改变底层对象之前，检查自己是否是当前对象仅有的用户。

```c++
if(!p.unique())
    p.reset(new string(*p));	// 不是唯一用户，分配新的拷贝
*p += newVal;					// 唯一用户，可以改变对象的值
```

#### 12.1.4 智能指针和异常

- 若异常发生在new之后delete之前，则内存不能正常释放

- 使用我们自己的释放操作

```c++
struct destination;					// 表示我们正在连接什么
struct connection;					// 使用连接所需的信息
connection connect(destination*);	// 打开连接
void disconnect(connection);		// 关闭给定的连接
```

```c++
void end_connection(connection *p){ disconnect(*p); }
```

```c++
void f(destination &d /* 其他参数 */)
{
    connection c = connect(&d);
    shared_ptr<connection> p(&c, end_connection);
    // 当f退出时，connection会被正确关闭
}
```

- 智能指针陷阱
  - 不使用相同的内置指针值初始化(或reset)多个智能指针
  - 不delete get()返回的指针
  - 如果使用get()返回的指针，当最后一个对应的智能指针销毁后，指针就变为无效了
  - 如果使用智能指针管理的资源不是new分配的内存，记住传递给它一个删除器

#### 12.1.5 unique_ptr

- 没有类似make_shared的标准库函数，因此定义unique_ptr时需要将其绑定到一个new返回的指针上。

- unique_ptr不支持普通的拷贝或赋值操作

```c++
unique_ptr<T> u1
unique_ptr<T, D> u2			// u2会使用一个可调用对象D释放指针
u.release()					// 放弃对指针的控制权，返回指针
u.reset()					// 释放u指向的对象
u.reset(q)					// 如果提供内置指针q，令u指向q对象
```

- 可以通过调用release或reset将指针的所有权从一个unique转移给另一个:

```c++
unique_ptr<string> p2(p1,release());	// p1转移给p2
p2.reset(p3.release());		// reset释放了p2原来指向的内存
```

- 我们可以拷贝或赋值一个将要被销毁的unique_ptr

```c++
unique_ptr<int> clone(int p) 
{
    return unique_ptr<int>(new int(p));
}
```

- 向unique_ptr传递删除器

```c++
unique_ptr<objT, decT> p(new objT, func);
```

```c++
void f(destination &d /* 其他需要的参数 */)
{
    connection c = connect(&d);
    unique_ptr<connection, decltype(end_connection)*>
        p(&d, end_connection);
}
```

#### 12.1.6 weak_ptr

- weak_ptr指向一个由shared_ptr管理的对象，并且不改变shared_ptr的引用计数。

```c++
w.reset()
w.use_count()
w.expired()					// 若w.use_count()为0返回true
w.lock()					// 若expired为true，返回一个空								 shared_ptr；否则返回一个指向								w对象的shared_ptr
```

### 12.2 动态数组

#### 12.2.1 new和数组

- 分配一个数组会得到一个元素类型的指针

```c++
int *pia = new int[get_size()];
```

```c++
int *pia2 = new int[10]();			// 值初始化
int *pia3 = new int[10]{1, 2, 3};	
```

- 释放动态数组

  释放一个指向数组首元素的指针，元素被逆序销毁，空方括号对是必需的。

```c++
delete [] pa;
```

- **unique_ptr**

```c++
unique_ptr<int[]> up(new int[10]);
up.release();					// 自动用delete[]销毁其指针
```

- unique_ptr指向的时一个数组而不是单个对象，**不能使用点和箭头成员运算符，可以使用下标运算符**

```c++
unique_ptr<T[]> u(p)		// u指向内置指针p所指向的动态分配								  的数组
u[i]
```

- share_ptr不支持管理动态数组，如果希望使用，需要自己提供删除器

```c++
shared_ptr<int> sp(new int[10], [](int *p) {delete[]p;})
```

- shared_ptr未定义下标运算符，智能指针类型不支持指针算术运算

```c++
for(size_t i = 0; i != 10; ++i)
	*(sp.get() + i) = i;		// 使用get获取内置指针
```

#### 12.2.2 allocator类

- 将内存分配和对象构造分离开来

```c++
allocator<T> a			// 定义了一个名为a的allocator对象
a.allocate(n)			// 分配一段未构造的内存保存n个对象
a.deallocate(p, n)		// 释放从T*指针p地址开始的内存，p必须							 是一个先前由allocate返回的指针，且							 n必须是p创建时所要求的大小。
a.construct(p, agrs)	// p必须指向一块原始内存；用来在p指向							的内存中构造对象
a.destroy(p)			// 对p指向的对象执行析构函数
```

- 分配未构造的内存

```c++
auto q = p;
alloc.construct(q++, 10, 'c');	// *q为cccccccccc
```

- 拷贝和填充未初始化内存的算法

```c++
uninitialized_copy(b, e, b2)	// 从b和e范围拷贝元素到b2指									定的原始内存中。 
uninitialized_copy_n(b, n, b2)	// 从b开始拷贝n个元素到b2中
uninitialized_fill(b, e, t)		// 在b和e范围中创建值为t对象
uninitialized_fill_n(b, n, t)	// 从b开始创建n个值为t的对象
```

### 12.3 文本查询程序

#### 12.3.1 文本查询程序设计

- 用vector\<string>保存整个输入文件的一份拷贝，每行作为vector中的一个元素
- 使用istringstream将每行分解为单词
- 使用set保存每个单词在输入文本中出现的行号
- 使用一个map将每个单词与它出现的行号set关联起来

#### 12.3.2 文本查询程序类的定义



















