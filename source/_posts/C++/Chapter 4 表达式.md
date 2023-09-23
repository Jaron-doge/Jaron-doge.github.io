---
title: 第四章 表达式
date: 2020-09-04 16:45:04
tags: C++ Primer
categories: C++ 
---

## 第四章 表达式

### 4.1 基础

#### 4.1.1 左值和右值

- 当一个对象被用作右值的时候，用的是对象的值；当对象被用作左值的时候，用的是对象的身份。

<!-- more -->

- decltype

  ​	如果表达式的求值结果是左值，得到一个引用类型

#### 4.1.2 优先级与结合律

#### 4.1.3 求值顺序

### 4.2 算术运算符

### 4.3 逻辑和关系运算符

### 4.4 赋值运算符

### 4.5 递增和递减运算符

### 4.6 成员访问运算符

- —>作用于一个指针类型的运算对象，结果是左值
- 点运算符作用于左值结果是左值，作用于右值结果是右值

### 4.7 条件运算符

- 右结合律

### 4.8 位结合律

### 4.9 sizeof运算符

- 右结合律

- 返回size_t类型的常量表达式

### 4.10 逗号运算符

### 4.11 类型转换

#### 4.11.3 显示转换

```c++
cast-name<type>(expression);
```

- cast-name是static_cast、dynamic_cast、const_cast和reinterpret_cast中的一种

1. static_cast

```c++
void *p = &d;
double *dp = static_cast<double*>(p);
```

2. const_cast

   const_cast只改变运算对象的底层const

```c++
const int constant = 21;
const int* const_p = &constant;
int* modifier = const_cast<int*>(const_p);
*modifier = 7;
```

3. reinterpret_cast

### 4.12 运算符优先级表

![image-20200120213610912.png](https://i.loli.net/2020/09/04/64jmMvIzWZpxSne.png)



![image-20200120213630510.png](https://i.loli.net/2020/09/04/G2tYarPp4sJ7nM3.png)

![image-20200120213642127.png](https://i.loli.net/2020/09/04/6Gsgm9riHkDlIzB.png)