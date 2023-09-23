---
title: 第十四章 重载运算与类型转换
date: 2020-09-04 16:45:14
tags: C++ Primer
categories: C++ 
---

## 第十四章 重载运算与类型转换

### 14.1 基本概念

- 如果一个运算符函数是成员函数，则它的第一个运算对象绑定到隐式的this指针上

<!-- more -->

- 对于一个运算符函数，它或者是类的成员，或者至少含有一个类类型的参数，这一约定意味着当运算符作用域内置类型的运算对象，我们无法改变该运算符的含义
- 不能被重载的运算符

```c++
::	.*	.	? :
```

- 直接调用一个重载的运算符函数

```c++
// 一个非成员运算符函数的等价调用
data1 + data2;					// 普通的表达式
operator+(data1, data2);		// 等价的函数调用
// 成员运算符函数的等价调用
data1 += data2;
data1.operator+=(data2);
```

- 通常情况下，不应该重载逗号、取地址、逻辑与和逻辑或运算符
- **成员函数/非成员函数准则**
  - 赋值（=）、下标（[]）、调用（()）、和成员访问箭头（->）必须是成员
  - 复合赋值运算符一般来说应该是成员
  - 改变对象状态的运算符或者与给定类型密切相关的运算符，如递增、递减和解引用运算符，通常应该是成员
  - 具有对称性的运算符可能转换任意一端的运算对象，例如算数、相等性、关系和位运算符等，通常应该是普通的非成员函数

### 14.2 输入和输出运算符

#### 14.2.1 重载输出运算符<<

- 输出运算符不应该打印换行符

- 输入输出运算符必须是非成员函数

#### 14.2.2 重载输入运算符>>

- 输入运算符必须处理输入可能失败的情况，而输出运算符不需要

- 当流含有错误类型的数据时读取操作可能失败
- 当读取操作到达文件末尾或遇到输入流的其他错误时也会失败

### 14.3 算术和关系运算符

- 最有效的方式是使用复合赋值来定义算术运算符

#### 14.3.1 相等运算符

- 相等运算符和不相等运算符中的一个应该把工作委托给另外一个

#### 14.3.2 关系运算符

- 如果存在唯一一种逻辑可靠的<定义，则应该考虑为这个类定义<运算符。如果类同时还包含\==，则当且仅当<的定义和==产生的结果一致时才定义<运算符。

### 14.4 赋值运算符

```c++
StrVec& StrVec::operator=(initializer_list<string> il)
{
    auto data = alloc_n_copy(il.begin(), il.end());
    free();
    elements = data.first;
    first_free = cap = data.second;
    return *this;
}
```

### 14.5 下标运算符

- 如果一个类包含下标运算符，则它通常会定义两个版本：一个返回普通引用，另一个是类的常量成员并且返回常量引用

```c++
class StrVec {
public:
	string& operator[](std::size_t n)
    { return elements[n]; }
    const string& operator[](size_t n) const 
    { return elements[n]; }
private:
    string *elements;
};
```

### 14.6 递增和递减运算符

- 定义递增和递减运算符的类应该同时定义前置版本和后置版本

```c++
StrBlobPtr& StrBlobPtr::operator++()
{
    // 如果curr已经指向了容器的尾后位置，则无法递增它
    check(curr, "increment past end of StrBlobPtr");
    ++curr;
    return *this;
}

StrBlobPtr& StrBlobPtr::operator--()
{
    // 如果curr是0，则继续递减它将产生一个无效下标
    --curr;
    check(curr, "decrement past begin of StrBlobPtr");
    return *this;
}
```

- 后置版本接受一个额外的（不被使用）int类型的形参
- 后置运算符返回对象的原值而非引用

```c++
StrBlobPtr StrBlobPtr::operator++(int)
{
    // 此处无需检查有效性，调用前置递增运算时才需要检查
    StrBlobPtr ret = *this;
    ++*this;				// 前置++需要检查递增的有效性
    return ret;
}

StrBlobPtr StrBlobPtr::operator--(int)
{
    // 此处无需检查有效性，调用前置递减运算时才需要检查
    StrBlobPtr ret = *this;
    --*this;				// 前置--需要检查递增的有效性
    return ret;
}
```

- 显式地调用后置运算符

```c++
StrBlobPtr p(a1);
p.operator++(0);			// 调用后置版本的operator++
p.operator++();				// 调用前置版本的operator++
```

### 14.7 成员访问运算符

```c++
class StrBlobPtr {
public:
	string& operator*() const 
    { auto p = check(curr, "dereference past end");
    return (*p)[curr];			// (*p)是对象所指的vector
   	}
    string* operator->() const
    { return& this->operator*(); }
}
```

- 对于point->member
  1. 如果point是指针，则我们应用内置的箭头运算符，表达式等价于(*point).member
  2. 如果point是定义了operator->的类的一个对象，则我们使用point.operator->()的结果来获取member。如果该结果是一个指针，则执行第一步，如果该结果本身含有重载的operator->()，则重复调用当前步骤

### 14.8 函数调用运算符

- 函数调用运算符必须是成员函数

```c++
struct absInt {
    int operator() (int val) const {
        return val < 0 ? -val : val;
    }
};
absInt absObj;				// 含有函数调用运算符的对象
int ui = absObj(42);		// 将i传递给absObj.operator()
```

- 含有状态的函数对象类

```c++
class PrintString {
public:
	PrintString(ostream& o = cout, char c = ' ') :
        os(o), sep(c) { }
    void operator()(const string& s) const 
    { os << s << sep; }
private:
    ostream& os;
    char sep;
}
```

- 函数对象常常作为泛型算法的实参

```c++
for_each(vs,begin(), vs.end(), PrintString(cerr, '\n'));
```

```c++
bool operator()(const int elem) { return value == elem; }
replace_if(vec.begin(), vec.end(), Compare(3), 5);
```

#### 14.8.1 lambda是函数对象

- **当一个lambda表达式通过引用捕获变量时**，将由程序负责确保lambda执行时引用所引的对象确实存在，**编译器可以直接使用该引用而无须在lambda产生的类中将其存储为数据成员**

- **通过值捕获的变量被拷贝到lambda中，产生的类必须为每个值捕获的变量建立对应的数据成员，同时创建构造函数，使用捕获的变量的值来初始化数据成员**

```c++
auto wc = find_if(words.begin(), words.end(), 
                 [sz](const string& a)
                  { return a.size() >= sz; });
```

- 该lambda产生的类

```c++
class SizeComp {
public:
    SizeComp(size_t n) : sz(n) { }	// 该形参对应捕获的变量
    bool operator()(const string& s) const
    { return s.size() >= sz; }
private:
    size_t sz;				// 该数据成员对应通过值捕获的变量
};
```

```c++
auto wc = find_if(words.begin(), words.end(), SizeComp(sz));
```

- **lambda表达式产生的类不含默认构造函数、赋值运算符及析构函数；**它是否含有默认的拷贝/移动构造函数则通常要视捕获的数据成员类型而定

```c++
class SizeComp {
public:
    SizeComp(const size_t n) : sz(n) { }
    bool operator()(const string& s) 
    { return s.size() == sz; }
private:
    size_t sz;
}
```

#### 14.8.2 标准库定义的函数对象

![image-20200831143609829.png](https://i.loli.net/2020/09/04/woXf46DAp1BRmk3.png)

- 在算法中使用标准库函数对象

```c++
sort(svec.begin(), svec.end(), greater<string>());
count_if(vec.begin(), vec.end(), bind(greater<int>(), _1, 1024));
```

- **我们不能直接比较两个无关指针，但是可以使用标准库函数对象来实现该目的**

```c++
vector<string*> nameTable;
// 错误：nameTble中的指针彼此之间没有关系，<将产生未定义的行为
sort(nameTable.begin(), nameTable.end(), []	
     (string* a, string* b) { return a < b; });
//正确：标准库规定指针的less是定义良好的
sort(nameTable.begin(), nameTable.end(), less<string*>());
```

#### 14.8.3 可调用对象与function

- 不同类型可能具有相同的调用形式

```c++
int add(int i, int j) { return i + j; }
auto mod = [](int i, int j) { return i % j; };
struct divide {
    int operator()(int denominator, int divisor) {
        return divisor / denominator;
    }
};
```

- 我们可以构建从运算符到函数指针的映射关系

```c++
map<string, int(*)(int, int)> binops;
binops.insert({"+", add});	 	// 正确：add是一个函数指针
binops.insert({"%", mod});		// 错误：mod不是一个函数指针
```

- 标准库function类型

```c++
function<T> f;		// f是一个用来存储可调用对象的空function
function<T> f(obj);	// 在f中存储可调用对象obj的副本
f					// f作为条件，当f含有可调用对象时为真
    
定义为function<T>的成员的类型
result_type			// 该funtion类型的可调用对象返回的类型
argument_type		// 如果T只有一个实参，则argument_type是该类
first_argument_type // 型的同义词；如果T有两个实参，则first、
second_argument_type// second分别代表两个实参的类型
```

- 用function构建函数

```c++
function<int(int, int)> f1 = add;		// 函数指针
function<int(int, int)> f2 = divide();	// 函数对象类的对象
function<int(int, int)> f3 = [](int i, int j)	// lambda
							{ return i * j; };
```

- 用function重新构建map

```c++
map<string, function<int(int, int)>> binops;
binops = {
    {"+", add},							// 函数指针
    {"-", std::munus<int>()},			// 标准库函数对象 
    {"/", divide()},					// 用户定义的函数对象
    {"*", [](int i, int j) { return i * j; }},	
    {"%", mod}
};
```

- 索引map

```c++
binops["+"](10, 5);		// 调用add(10, 5)
binops["-"](10, 5);		// 使用minuns<int>对象的调用运算符
```

- 我们不能（直接）将重载函数的名字存入function类型的对象中，可以通过存储函数指针解决

```c++
int add(int i, int j) { return i + j; }
Sales_data add(const Sales_data&, const Sales_data&);
binops.insert({"+", add});			// 错误
```

```c++
int (*fp)(int, int) = add;
binops.insert({"+", fp});			// 正确
```

### 14.9 重载、类型转换与运算符

#### 14.9.1 类型转换运算符

- operator *type*() const;

  一个类型转换函数必须是类的成员函数；它不能声明返回类型，形参列表页必须为空。类型转换函数通常应该是const

- 定义函数类型转换运算符的类

```c++
class SmallInt {
public:
	SmallInt(int i = 0) : val(i)
    {
        if(i < 0 || i > 255)
            throw std::out_of_range("bad SmallInt value");
    }
    operator int() const { return val; }
private:
    size_t val;
};
```

​		构造函数将算术类型的值转换成SmallInt对象，而类型转换运算符将SmallInt对象转换成int

```c++
SmallInt si;
si = 4;			// 首先将4隐式地转换成SmallInt,在调用operator=
si + 3;			// 首先将si隐式地转换成int，然后执行整数的加法
```

- 显式的类型转换运算符

```c++
class SmallInt {
public:
    explicit operator int() const { return val; }
};
```

​		必须通过显式的强制类型转换才能执行

```c++
SmallInt si = 3;
static_cast<int>(si) + 3;
```

- 当表达式出现在下列位置时，显式的类型转换将被隐式地执行：
  - if、while及do语句的条件部分
  - for语句头的条件表达式
  - ！、||、&&的运算对象
  - 条件运算符（? :）的条件表达式

- 转换为bool
  - 无论我们什么时候在条件中使用流对象，都会使用为IO类型定义的operator bool
  - 向bool的类型转换通常用在条件部分，因此operator bool一般定义成explicit的

```c++
while(cin >> value)
```

```c++
operator string() const { return bookNo; }
operator double() const { return revenue; }
```

#### 14.9.2 避免有二义性的类型转换

- 第一种情况：A定义了接受B类对象的转换构造函数，B定义了转换目标是A的类型转换运算符

```c++
struct B;
struct A {
   	A() = default;
    A(const B&);
};
struct B {
    operator A() const;
};
```

- 需要显式调用

```c++
A f(const A&);
B b;
A a1 = f(b.operator A());
A a2 = f(A(b));
```

- 第二种情况：转换源类型本身可以通过其他类型转换联系在一起

  ​	**最好不要创建两个转换源都是算术类型的类型转换**

```c++
struct A {
    A(int = 0);
    A(double);
    operator int() const;
    operator double() const;
};
```

- 除了显式地向bool类型的转换之外，我们应该尽量避免定义类型转换函数

- 在调用重载函数时，如果需要额外的标准类型转换，则该转换的级别只有当所有可行函数都请求同一个用户定义的类型转换时才有用。如果所需的用户定义的类型转换不止一个，则该调用具有二义性

- **order:**
  1. exact match
  2. const conversion
  3. promotion
  4. arithmetic or pointer conversion
  5. class-type conversion

#### 14.9.3 函数匹配与重载运算符

- 如果我们对同一个类既提供了转换目标是算术类型的类型转换，也提供了重载的运算符，则将会遇到重载运算符与内置运算符的二义性问题