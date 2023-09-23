---
title: 第六章 函数
date: 2020-09-04 16:45:06
tags: C++ Primer
categories: C++ 
---

## 第六章 函数

### 6.1 函数基础

#### 6.1.1 局部对象

- 名字的作用域是程序文本的一部分，名字在其中可见。
- 对象的生命周期是程序执行过程中该对象存在的一段时间。

<!-- more -->

1. 自动对象

   变量定义本身含有初始值，就用初始值进行初始化

2. 局部静态对象

   局部作用域全局生命周期

### 6.2 参数传递

- 尽量声明常量引用

- 当使用argv中的实参时，可选的实参从argv[1]开始;argv[0]保存程序的名字

#### 6.2.6 含有可变形参的函数

1. initializer_list形参

```c++
error_msg(initializer_list<string> ls);
error_msg({"functionX", "okay"});
```

- initializer_list对象中的元素永远是常量值

- 序列放在花括号内

2. 省略符形参

### 6.3 返回类型和return语句

- 函数可以返回花括号包围的值的列表

```c++
vector<string> process()
{
    string s;
    return {"functionX", s};
}
```

#### 6.3.3 返回数组指针

1. 类型别名

```c++
typedef int arrT[10];
using arrT = int[10];
arrT* func(int i);		// func返回一个指向含有10个整数的数组的指针
```

2. 形参列表在数组维度之前

```c++
int (*func(int i))[10];
```

​	可以按照以下的顺序来逐层理解该声明的含义：

- func(int i)表示调用func函数时需要一个int类型的实参。
- (*func(int i))意味着对我们可以对函数调用的结果执行解引用操作。
- (*func(int i))[10]表示解引用func的调用将得到一个大小是10的数组。
- int (*func(int i))[10]表示数组中的元素是int类型。

3. 尾置返回类型

在本应该出现返回类型的地方放置一个auto

```c++
auto func(int i)->int(*)[10];
```

4. decltype

```c++
decltype(odd) *arrPtr(int i);
```

### 6.4 函数重载

- 不允许两个函数形参列表相同返回类型不同

- 顶层const不能重载，底层const可以

```c++
Record lookup(Account*);		// 作用于指向Account的指针
Record lookup(const Account*);	// 作用于指向常量的指针
```

### 6.5 特殊用途语言特性

#### 6.5.1 默认实参

- 一旦某个形参被赋予了默认值，它后面的所有形参都必须有默认值

#### 6.5.2 内联函数

- 调用时展开

#### 6.5.3 constexpr函数

- 函数的返回类型及所有形参的类型都得是字面值类型
- 必须有且只有一条return语句

**内联函数和constexpr函数定义在头文件中**

#### 6.5.4 调试帮助

1. assert

- 包含于cassert头文件中，用于检查“不能发生”的条件，为假终止程序的执行

2. NDEBUG

```c++
#define NDEBUG
#ifndef NEDBUG
cerr << _ _func_ _  << endl;
#endif
```

3. 内置变量

```c++
_ _func_ _	// 当前函数名字
_ _FILE_ _	// 文件名 
_ _LINE_ _	// 当前行号
_ _TIME_ _	// 编译时间
_ _DATE_ _	// 编译日期
```

### 6.6 函数匹配

- 调用重载函数应避免强制类型转换

### 6.7 函数指针

```c++
bool (*pf)(const string&, const string&);
```

- 函数名作为一个值时，该函数自动地转换成指针

```c++
pf = length;	// pf指向函数length
pf = &length;	// 等价: 取地址符是可选的
```

​		即使用时无须解引用

```c++
bool b1 = pf("hello", "goodbye");
bool b2 = (*pf)("hello", "goodbye");
bool b3 = length("hello", "goodbye");	// 全部等价
```

- 使用重载函数时

```c++
void (*pf)(int) = ff;
```

#### 6.7.1 函数指针形参

```c++
// Func 和 Func2是指向函数的指针
typedef bool (*FuncP)();
typedef decltype(length) *FuncP2;
```

#### 6.7.2 返回指向函数的指针

- 编译器不会将函数返回类型转化为指针类型

```c++
using PF = int(*)(int, int);	// PF是指针类型
PF f1(int);

int (*f1(int))(int, int);		// 等价类型
```

​		f1是个函数，返回一个指针；指针指向函数，返回类型是int。











