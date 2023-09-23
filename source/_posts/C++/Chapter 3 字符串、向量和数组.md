---
title: 第三章 字符串、向量和数组
date: 2020-09-04 16:45:03
tags: C++ Primer
categories: C++
---

## 第三章 字符串、向量和数组

### 3.1 命名空间的using声明

### 3.2 string

#### 3.2.1 四种初始化方式

<!-- more -->

```c++
string s1;				// s1是空串
string s2(s1);			// s2是s1的副本
string s2 = s1;			// 等价于s2(s1)
string s3(n, 'c');		// 把s3初始化为由连续n个字符c组成的串	
```

#### 3.2.2 string对象上的操作

```c++
os<<s;				// 将s写到os中，返回os
is>>s;				// 从is中读取字符串给s，返回is
getline(is, s);		// 从is中读取一行给s，返回is
s.empty();			// s为空返回true
s.size();			// 返回s中字符的个数
s[n];				// 返回s中第n个字符的引用
s1 + s2;			// 返回s1和s2连接后的结果
s1 = s2;			// 用s2的副本代替s1中原来的字符
s1 == s2;			// 如果s1和s2中所含的字符完全一样，则相等
s1 != s2;			
<. <=, >, >=;		
```

#### 3.2.3 getline

```c++
getline(cin, line);
cout << line << endl;
```

#### 3.2.4 字面值和string对象相加

- string与字符串字面值相加必须保证至少有一个运算对象是string

- 字符串字面值与string是不同类型

#### 3.2.5 string对象中的字符

```c++
isalnum(c);		// 字母和数字
isalpha(c);		// 字母
iscntrl(c);		// 控制字符
isdigit(c);		// 数字
isgraph(c);		// 不是空格但可打印
islower(c);		// 小写字母
isprint(c);		// 可打印字符
ispunct(c);		// 标调符号
isspace(c);		// 空白(空格、横向制表符、回车、换行等)
isupper(c);		// 大写字母
isxdigit(c);	// 十六进制数字
tolower(c);		// 输出小写字母
toupper(c);		// 输出大写字母
```

#### 3.2.6 范围for语句

```c++
for(declaration: expression)
    statement

1.遍历
    string str("some string");
    for(auto c: str)
        cout << c <<endl;
2.赋值
    // 必须把循环变量定义成引用类型
    for(auto &c: str)
        c = toupper(c);
```

### 3.3 vector

#### 3.3.1 vector初始化

```c++
vector<int> v1;						// 空vector				
vector<int> v2(v1);	
vector<int> v2 = v1;
vector<int> v3(n, val);				// n个val
vector<int> v4(n);					// n个初始化元素
vector<int> v4{n};					// 1个n
```

- ()用来描述容量,{}用来赋值

#### 3.3.2 添加元素

- push_back

### 3.4 迭代器

#### 3.4.1 begin和end

- cbegin和cend返回const_iterator

```c++
auto it = v.cbegin();		// it的类型是vector<int>::const_iterator
```

#### 3.4.2 迭代器运算

- the **+** operator between two iterators is illegal.

### 3.5 数组

#### 3.5.1 数组初始化

- 维度必须是一个常量表达式

- 不能用auto

```c++
int *ptrs[10];				// ptrs是含有10个整形指针的数组
int (*Parray)[10] = &arr;	// Parray指向一个含有10个整数的数组
int (&arrRef)[10] = arr;	// arrRef引用一个含有10个整数的数组
```

#### 3.5.2 访问数组元素

#### 3.5.3 指针和数组

1. begin和end

```c++
int ia[] = {1,2,3,4,5};
int *beg = begin(ia);
int *end = end(ia);
```

#### 3.5.4 C风格字符串

- 头文件cstring

#### 3.5.5 与旧代码的接口

1. c_str

```c++
string s;
const char *str = s.c_str();		// 返回一个C风格的字符串
```

2. 使用数组初始化vector对象

```c++
int arr[] = {0, 1, 2, 3, 4, 5};
vector<int> ivec(begin(arr), end(arr));
```

### 3.6 多维数组

 ```c++
constexpr row = 3, col = 3;
int ia[row][col] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
for(const auto &row: ia)
    for(auto col: row)
        cout << col << endl;
 ```

```c++
int ia[3][4];
int (*p)[4] = ia;	// *p是一个含有4个整数的数组
for(auto p = begin(ia); p != end(ia); ++p) {
    // q指向内层数组的首元素
    for(auto q = begin(*p); q != end(*p); ++q)
        cout << *q << ' ';
    cout << endl;
}
```

