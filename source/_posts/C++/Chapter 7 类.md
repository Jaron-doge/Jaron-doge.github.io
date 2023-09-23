---
title: 第七章 类
date: 2020-09-04 16:45:07
tags: C++ Primer
categories: C++ 
---

## 第七章 类

### 7.1 定义抽象数据类型

1. this

   this是一个指向非常量的常量指针，不能被常量对象调用

2. const成员函数

   const this是一个指向常量的常量指针

<!-- more -->

#### 7.1.4 构造函数

1. 默认构造函数

```c++
Sales_data() = default;
```

2. 构造函数初始值列表

- 当某个数据成员被构造函数初始值列表忽略时，它将以与合成默认构造函数相同的方式隐式初始化。

### 7.2 访问控制与封装

- struct的默认访问权限是public、class的默认访问权限是private

1. 封装

- 确保用户代码不会无意间破坏封装对象的状态
- 被封装的类的具体实现细节可以随时改变而无须调整用户级别的代码

#### 7.2.1 友元

- 友元的声明之外还需要独立的函数声明

### 7.3 类的其他特性

- 一个const成员函数如果以引用的形式返回*this，那么它的返回类型将是常量引用

```c++
class X;
class Y;
class X{
    Y *y = nullptr;
};
class Y{
    X x;
};
```

### 7.4 类的作用域

#### 7.4.1类的作用域

1. 在成员函数内查找该名字的声明。
2. 在类内查找。
3. 在成员函数定义之前的作用域内查找。

### 7.5 构造函数

#### 7.5.1 构造函数初始化列表

- 当成员是const或者是引用时，必须将其初始化

#### 7.5.2 委托构造函数

```c++
class Sales_data{
public:
	// 非委托构造函数
	Sales_data(std::string s, unsigned cnt, double price)
        :bookNo(s), units_sold(cnt), revenue(cnt * price){}
    // 委托构造函数
	Sales_data(): Sales_data("", 0, 0) {}
    Sales_data(std::string s): Sales_data(s, 0, 0) {}
    Sales_data(std:;istream &is): Slaes_data()
    {read(is, *this);}
};
```

#### 7.5.4 隐式的类类型转换

- explicit抑制构造函数的隐式转换
- 只能在类内声明构造函数时使用explicit
- explicit构造函数只能用于直接初始化不能用于赋值

```c++
explicit Sales_data(const std::string &s): bookNo(s) {}
item.combine(null_book);  //错误，因为需要从string转换为Sales_data
```

#### 7.5.5 聚合类

- 所有成员都是public的
- 没有定义任何构造函数
- 没有类内初始值
- 没有基类，也没有virtual函数

### 7.6 类的静态成员

- static静态成员被所有类对象共享
- 静态成员函数不包含this指针，不能声明成const

- 在类外定义时不能重复static关键字

- 静态成员可以作为默认实参







