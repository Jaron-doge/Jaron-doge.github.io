---
title: 第八章 IO库
date: 2020-09-04 16:45:08
tags: C++ Primer
categories: C++ 
---

### 第八章 IO库

### 8.1 IO类

- iostream定义了用于读写流的基本类型
- fstream定义了读写命名文件的类型
- sstream定义了读写内存string对象的类型

<!-- more -->

### 8.1.1 IO对象无拷贝或赋值

- 由于不能拷贝IO对象，进行IO操作的函数通常以引用方式传递和返回流，读写一个IO对象会改变其状态，因此传递和返回的引用不能是const的

#### 8.1.3 管理输出缓冲

1. 刷新输出缓冲区

```c++
cout << "hi!" << endl;	// 换行并刷新
cout << "hi!" << flush;	// 刷新
cout << "hi!" << ends;	// 空字符+刷新
```

2. unitbuf操纵符

   在接下来的每次写操作之后都进行一次flush操作。

```c++
cout << unitbuf;	
// 任何输出都立即刷新，无缓冲
cout << nounitbuf;	// 回到正常的缓冲方式
```

3. 关联输入和输出流

```c++
1. cin.tie();		// 返回指向输出流的指针
2. cin.tie(&cout);	// 将cin和cout关联在一起
```

- 每个流同时最多关联到一个流，但多个流可以同时关联到同一个ostream

### 8.2 文件输入输出

```c++
fstream fstrm;			// 创建一个未绑定的文件流
fstream fstrm(s);		// 创建一个fstream，并打开s文件
fstream fstrm(s, mode);	// 以mode模式打开s
fstream.open(s);		// 打开s，并于fstrm绑定
fstrm.close();			// 关闭文件
fstrm.is_open();		// 是否成功打开
```

#### 8.2.1 使用文件流对象

- 可以用fstream代替iostream&

#### 8.2.2 文件模式

```c++
in		// 以读方式打开
out 	// 以写方式打开
app		// 每次写操作前均定位到文件末尾
ate		// 打开文件后立即定位到文件末尾
trunc	// 截断文件
binary	// 以二进制方式进行IO
```

- 保留被ofstream打开的文件中已有数据的唯一方法是显式指定app或in模式

```c++
ofstream app2("file2", ofstream::out | ofstream::app);
```

### 8.3 string流

```c++
sstream strm;
strm.str();		// 返回strm所保存的string的拷贝
strm.str(s);	// 将string s拷贝到strm中
```









