---
title: 第五章 语句
date: 2020-09-04 16:45:05
tags: C++ Primer
categories: C++ 
---

## 第五章 语句

### 5.1 简单语句

### 5.2 语句作用域

### 5.3 条件语句

<!-- more -->

- case标签必须是整型常量表达式

### 5.4 迭代语句

- do while的变量定义要放在外面

### 5.5 跳转语句

- 标签标识符可以和程序中的其他实体的标识符使用同一个名字

### 5.6 try语句块和异常处理

```c++
try {
    program-statements
} catch(exception-declaration) {
    handler-statements
}
```

- try块内声明的变量在catch子句内无法访问

#### 5.6.3 标准异常

- exception头文件只报告异常的发生，不提供额外信息。

- stdexcept头文件定义了几种常用的异常类

- new头文件定义了bad_alloc异常类型

- type_info头文件定义了bad_cast异常类型

- 只能以默认初始化的方式初始化exception、bad_alloc和bad_cast对象，不允许为这些对象提供初始值。

- what成员函数返回异常初始值



