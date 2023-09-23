---
title: 第二章 变量和基本类型
date: 2020-09-04 16:45:02
tags: C++ Primer
categories: C++ 
---
## 第二章 变量和基本类型

### 2.1 基本内置类型

#### 2.1.1 算术类型

- char在一些机器上是有符号的，而在另一些机器上无符号，应明确指定它的类型是signed char 或者 unsigned char。
- 浮点运算选double

<!-- more -->

#### 2.1.2 类型转换

- 含有无符号类型的表达式

```c++
1. 带符号数会转换为无符号数
    unsigned u = 10;
    int i = -42;
    std::cout << u + i << std::endl; // int32位，输出4294967264
2. 无符号减无符号确保结果不为负
    unsigned u1 = 42, u2 = 10;
std::cout << u2 - u1 << std::endl;	// 结果是取模后的值
```

#### 2.1.3 字面值常量

- 024	/* 八进制 */
- 0x14 /* 十六进制 */

- 八进制只有前3个数字与\构成转义序列
- \x要用到后面跟着的所有数字

### 2.2 变量

#### 2.2.1 变量定义

- 对象是指一块能存储数据并拥有某种类型的内存空间。

- **初始化不是赋值，**初始化的含义是创建变量时赋予其一个初始值，而赋值的含义是把对象的当前值擦除，而已一个新值来替代

- 列表初始化

```c++
int units_sold = 0;
int unit_sold = {0};
int units_sold{0};
int units_sold(0);
```

#### 2.2.2 变量生命与定义

- 变量只能被定义一次，但可以被多次声明

```c++
extern int i;	// 声明而非定义
int j;			// 声明 + 定义
extern int i=1	// 定义
```

#### 2.2.3 标识符

1. 命名规范

- 不能连续出现两个下划线
- 不能以下划线紧连大写字母
- 变量名一般用小写字母
- 用户自定义的类名一般以大写字母开头

#### 2.2.4 作用域

### 2.3 复合类型

#### 2.3.1 引用

- 引用即别名，必须被初始化
- 只能绑定在对象上
- 不能二次引用

#### 2.3.2 指针

- 指针是一个对象

- 赋值永远改变的是等号左侧的对象

#### 2.3.3 复合类型的声明

1. 对指针的引用

   从右向左阅读，离变量名最近的符号是&，所以是引用

```c++
int *&r = p;	// r 是一个对指针p的引用
```

### 2.4 const

1. 全局const

```c++
file.c
extern const int c = fun();
file.h
extern const int c;
```

#### 2.4.1 const 的引用

​	也称常量引用

```c++
const int ci = 1024;
const int &c = ci;
```

1. 初始化常量引用

   允许用任意表达式作为初始值

```c++
int i = 42;
const int &r1 = i;		// 正确
const int &r2 = 42;		// 正确
const int &r3 = r1 * 2;	// 正确
int &r4 = r1 * 2; 		// 错误, r4是非常量引用
```

#### 2.4.2 指针和const

1. 指向常量的指针

   不能通过指针改变对象的值

```c++
double dval = 3.14;
const double *cptr = &dval;	
```

2. 常量指针

```c++
int num = 0;
int *const curNum = &num;	// curNum将一直指向num
```

#### 2.4.3 顶层const

​		顶层表示指针本身是个常量，底层表示指针所指的对象是一个常量。

#### 2.4.4 constexpr

1. 常量表达式是指值不会改变并且在编译过程就能得到计算结果的表达式。

- 声明为constexpr的变量一定是一个常量

```c++
constexpr int mf = 20;			// 20是常量表达式
```

2. 指针和constexpr

   ​	constexpr仅对指针有效，与指针所指的对象无关

```c++
const int *p = nullptr;		// p是一个指向整型常量的指针
constexpr int *q = nullptr;	// q是一个指向整数的常量指针
```

### 2.5 处理类型

#### 2.5.1 类型别名

1. typedef

```c++
typedef double wages;		// wages是double的同义词
```

2. using

```c++
using wages = double;
```

​	误区

```c++
typedef char *pstring;
const char* cstr = 0 和 const pstring cstr = 0 不一样
```

​	前者是指向const char的指针，后者pstring是一个指向char的指针，const pstring是一个指向char的常量指针。

#### 2.5.2 auto类型说明符

- auto通过初始值来推算变量的类型。

- 一条声明语句只能有一个基本数据类型。

- auto会忽略掉顶层const, 保留底层const
- 保留顶层const需明确指出: const auto f = ci

#### 2.5.3 decltype类型说明符

- 选择并返回操作数的数据类型

```c++
decltype(f()) sum = x;		// sum的类型就是函数f的返回类型
```

- 如果decltype使用的表达式是一个变量，则返回该变量的类型
- 如果decltype使用的表达式不是一个变量，则返回表达式结果对应类型

```c++
int i = 42, *p = &i, &r = i;
decltype(r) a;				// a是int&
decltype(r + 0) b;			// b是int	
decltype(*p) c = i;			// c是int&
```

- decltype不加括号，结果是该变量的类型，**加上括号，得到引用类型**

### 2.6 自定义数据结构

#### 2.6.1 Struct

#### 2.6.3 编写自己的头文件

1. 头文件保护符

- #ifdef当且仅当变量已定义时为真
- #ifndef当且仅当变量未定义时为真
- #endif为结束

```c++
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include <string>
struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
#endif
```



