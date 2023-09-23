---
title: 第十六章 模板与泛型编程
date: 2020-09-04 16:45:16
tags: C++ Primer
categories: C++ 
---

## 第十六章 模板与泛型编程

### 16.1 定义模板

#### 16.1.1 函数模板

- template

  ​	模板参数表示在类或函数定义中用到的类型或值

<!-- more -->

```c++
template <typename T> 
int compare(const T& v1, const T& v2)
{
    if(v1 < v2) return -1;
    if(v2 < v1) return 1;
    return 0;
}
```

- 实例化函数模板

  - 编译器用实参类型来确定模板参数类型

  - 编译器生成的版本被称为模板的实例

- 模板类型参数

  ​	**类型参数前必须使用关键字class或typename**

```c++
template <typename T,class U> calc(const T&, const U&);
```

- 非类型模板参数

   	非类型模板参数的模板实参必须是常量表达式

```c++
template <unsigned N, unsigned M>
int compare(const char(&p1)[N], const char(&p2)[M])
{
    return strcmp(p1, p2);
}
```

- inline和constexpr的函数模板

```c++
template <typename T> inline T min(const T&, constT&);
```

- 编写类型无关的代码

  ​		模板程序应该尽量减少对实参类型的要求

  - 函数参数是const的引用

  - 只用小于号（甚至是用less）

- 模板编译

  ​		为了生成一个实例化版本，编译器需要掌握函数模板或类模板成员函数的定义，**模板的头文件通常既包括声明也包括定义**

- 大多数编译错误在实例化期间报告

#### 16.1.2 类模板

- 定义类模板

```c++
template <typename T> class Blob {
public:
	typedef T value_type;
    typedef typename vector<T>::size_type size_type;
    Blob();
    Blob(initializer_list<T> il);
    size_type size() const { return data->size(); }
    bool empty() const { return data->empty(); }
    void push_back(T&& t) { data->push_back(move(t));}
    void pop_back;
    
    T& back();
    T& operator[](size_type i);
private:
    shread_ptr<vector<T>> data;
    void check(size_type i, const string& msg) const;
};
```

- 类模板的成员函数

  ​	**定义在类模板之外的成员函数必须以关键字template开始**

```c++
template <typename T> ret-type Blob<T>::member-name(parm-list)
```

- Blob

```c++
template <typename T>
void Blob<T>::check(size_type i, const string& msg) const
{
    if(i >= data->size())
        throw out_of_range(msg);
}
template <typename T>
T& Blob<T>::back()
{
	check(0, "back on empty Blob");
    return data->back();
}
template <typename T>
T& Blob<T>::operator[](size_type i)
{
    check(i, "subscript out of range");
    return (*data)[i];
}
template <typename T>
void Blob<T>::pop_back()
{
    check(0, "pop_back on empty Blob");
    data->pop_back();
}
```

- Blob构造函数

```c++
template <typename T>
Blob<T>::Blob() : data(make_shared<vector<T>>()) { }
template <typename T>
Blob<T>::Blob(initializer_list<T> il) : 
				data(make_shared<vector<T>>(il)){ }
```

- 类模板成员函数的实例化

  ​		默认情况下，一个类模板的成员函数只有当程序用到它时才进行实例化

- 在一个类模板的**作用域内**，我们可以直接使用模板名而不必指定模板实参

- 一对一友好关系

```c++
template <typename> class BlobPtr;
template <typename> class Blob;
template <typename T>
	bool perator==(const Blob<T>&, const Blob<T>&);

template <typename T> class Blob {
	friend class BlobPtr<T>;
    friend bool operator==<T>
        (const Blob<T>&, const Blob<T>&);
}
```

- 通用和特定的模板友好关系

```c++
// 前置声明
template <typename T> class Pal;
class C {	// C是普通的非模板类
    // Pal2的所有实例都是C的友元
    template <typename T> friend class Pal2;
};
template <typename T> class C2 {	// C2 本身是一个模板
    // C2的每个实例将相同实例化的Pal声明为友元
    friend class Pa1<T>;
    // Pal3是一个非模板类，它是C2所有实例的友元
    friend class Pal3;
};
```

- 令模板自己的类型参数成为友元

  ​	Sales_data是Bar<Sales_data>的友元

```c++
template <typename type> class Bar {
	friend Type;	// 将访问权限授予用来实例化Bar的类型
};
```

- 模板类型别名

```c++
template<typename T> using twin = pair<T, T>;
twin<string> authors; // authors是一个pair<T, T>;
```

```c++
template <typename T> using partNo = pair<T, unsigned>;
```

- 类模板的static成员

  ​		对任意给定类型X，都有一个Foo\<X>::ctr和一个Foo\<X>::count成员，所有Foo\<X>类型的对象共享相同的ctr和count

```c++
template <typename T> class Foo {
public:
    static size_t count() { return ctr; }
private:
    staitc size_t ctr = 0;
};
```

#### 16.1.3 模板参数

- 模板声明必须包含模板参数

- 使用类的类型成员

  ​		编译器需要知道我们正在定义一个名为p的变量还是将一个名为size_type的static数据成员与名为p的变量相乘

  ```c++
  T::size_type * p;
  ```

  - **当我们希望通知编译器一个名字表示类型时，必须使用关键字typename**而非class

  ```c++
  template <typename T> 
  typename T::value_type top(const T& c);
  ```

- 默认模板实参
  
  - 对于一个模板参数，只有当它右侧的所有参数都有默认实参时，它才可以有默认实参

```c++
template <typename T, typename F = less<T>>
int compare(const T& v1, const T& v2, F f = F()) {
    if(f(v1, v2)) return -1;
    if(f(v2, v1)) return 1;
    return 0;
}
```

- 模板默认实参与类模板
  - 无论何时使用一个类模板，都必须在模板名后接上尖括号

```c++
template<class T = int> class Numbers {
public:
    Numbers(T v= 0) : val(v) { }
private:
    T val;
};
Numbers<> average_precision;
```

#### 16.1.4 成员模板

- 成员模板不能是虚函数
- 普通（非模板）类的成员模板

```c++
// 对给定指针执行delete
class DebugDelete {
public:
    DebugDelete(ostream& s = cerr) : os(s) { }
    template <typename T> void operaotr()(T *p) const
    { os << "deleting unique_ptr" << endl; delete p; }
private:
    ostream& os;
};
```

```c++
double *p = new double;
DebugDelete d;
d(p);
int * ip = new int;
DeBugDelete()(ip);
```

​			由于调用一个DebugDelete对象会delete其给定的指针，我们也可以将DebugDelete用作删除器

```c++
// 实例化DebugDelete::operator()<int>(int*)
unique_ptr<int, DebugDelete> p(new int, DebugDelete());
```

- 类模板的成员模板

  ​		当我们在类模板外定义一个成员模板时，必须同时为类模板和成员模板提供模板参数列表

```c++
template <typename T>
template <typename It>
Blob<T>::Blob(It b, It e) :
	data(make_shared<vector<T>>(b, e)) { }
```

#### 16.1.5 控制实例化

- 显式实例化

```c++
extern template declaration;			// 实例化声明
template declaration;					// 实例化定义
```

​			由于编译器在使用一个模板时自动对其实例化，因此extern声明必须出现在任何使用此实例化版本的代码之前

- 实例化定义会实例化所有成员

#### 16.1.6 效率与灵活性

​			通过在编译时绑定删除器，unique_ptr避免了间接调用删除器的运行时开销。通过在运行时绑定删除器，shared_ptr使用户重载删除器更为方便

### 16.2 模板实参推断

#### 16.2.1 类型转换与模板类型参数

- 能在调用中应用于函数模板的两项：
  - const转换：将一个非const对象的引用传递给一个const引用形参
  - 数组或函数指针转换：如果函数形参不是引用类型，则可以对实参应用正常的指针转换

#### 16.2.2 函数模板显式实参

- 指定显式模板实参

```c++
template <typename T1, typename T2, typename T3>
T1 sum(T2, T3);
```

```c++
auto val = sum<long long>(int i,long lng);
```

- 正常类型转换应用于显式指定的实参

```c++
long lng;
compare<long>(lng, 1024);
```

#### 16.2.3 尾置返回类型与类型转换

​			由于尾置返回出现在参数列表之后，它可以使用函数的参数

```c++
template <typename It>
auto fcn(It beg, It end) -> decltype(*beg)
{
    // 处理序列
    return *beg;
}
```

- 进行类型转换的标准库模板类

  ​		有时我们无法直接获得所需要的类型，可以使用type_traits中的类型转换模板

```c++
// 返回一个元素值的版本
template <typename It>
auto fcn2(It beg, It end) -> 
typename remove_reference<decltype(*beg)>::type
{
    // 处理序列
    return *beg;
}
```

#### 16.2.4 函数指针和实参推断

​			当我们用一个函数模板初始化一个函数指针或为一个函数指针赋值时，编译器使用指针的类型来推断模板实参

```c++
template <typename T> int compare(const T&, const T&);
int (*pf1)(const int&, const int&) = compare;
```

- 重载时需要使用显式模板实参消除歧义

```c++
void func((int*)(const string&, const string&));
void func((int*)(const int&, const int&));
func(compare<int>);
```

#### 16.2.5 模板实参推断和引用

- 从左值引用函数参数推断类型

```c++
template<typename T> void f2(const T&);
// 在每个调用中，f2的函数参数都被推断为const int&
f2(int i);			// T都是int
f2(const int ci);
f2(5);
```

- 从右值引用函数参数推断类型

```c++
template <typename T> void f3(T&&)
f3(42);				// T是int
```

- **引用折叠和右值引用参数**

  - 对于一个给定类型X:

  ```C++
  X& &、X& &&和X&& &都折叠成类型X&
  类型X&& &&折叠成X&&
  ```

  - 当我们将一个左值传递给f3(右值引用)函数参数时，编译器推断T为一个左值引用类型

  ```c++
  f3(int i);			// T是int& 
  f3(const int ci);	// T是const int&
  ```

  - **结论：对于T&&类型的函数参数，我们可以传递给它右值，也可以传递给它左值**

- 编写接受右值引用参数的模板函数

  ​	我们可以对使用右值引用的函数模板进行重载

```c++
template <typename T> void f(T&&);	// 绑定到非const右值
template <typename T> void f(const T&);	// 左值和const右
```

#### 16.2.6 理解std::move

- std::move的定义

  ```c++
  template <typename T>
  typename remove_reference<T>::type&& move(T&& t)
  {
      return static_cast
          <typename remove_reference<T>::type&&>(t);
  }
  ```

  - 传递右值"hello"：
    - 推断出T的类型为string
    - remove_reference用string进行实例化
    - type是string
    - move的返回类型是string&&
    - move的函数参数t的类型是string&&
    - 函数：string&& move(string&& t){ return t; }
  - 传递左值
    - T的类型为string&
    - remove_reference用string&进行实例化
    - type是string
    - move的返回类型是string
    - move的函数参数t实例化为string& &&，折叠为string&
    - static_cast<string&&>(t)将t转换为string&&

	#### 16.2.7 转发

- 定义能保持类型信息的函数参数

```c++
voif f(int v1, int& v2) { cout << v1 << ++v2 << endl;}
// 可以接受一个左值引用和函数，不能接受右值引用参数的函数
template <typename F, typename T1, typename T2>
void flip2(F f, T1&& t1, T2&& t2)
{	f(t2, t1;)	}
```

- 在调用中使用std::forward保持类型信息

  - forward必须通过显式模板实参来调用
  - forward返回该显式实参类型的右值引用



- **当用于一个指向模板参数类型的右值引用函数参数(T&&)时，forward会保持实参类型的所有细节**

  ```c++
  template <typename Type> intermediary(Type&& arg)
  {
      finalFcn(std::forward<Type>(arg));
      // ...
  }
  ```

  - 实参是右值：
    - Type是普通类型
    - forward\<Type>返回Type&&
  - 实参是左值:
    - Type本身是一个左值引用类型
    - 对forward\<type>返回类型进行引用折叠，返回一个左值引用类型

### 16.3 重载与模板

- 编写重载模板

```c++
template <typename T> string debug_rep(const T& t)
{
    ostringstream ret;
    ret << t;
    return ret.str();
}
```

```c++
template <typename T> string debug_rep(T* p)
{
    ostringstream ret;
    ret << "pointer: " << p;
    if(p)
        ret << " " << debug_rep(*p);
    else 
        ret << " null pointer";
    return ret.str();
}
```

- 当有多个重载模板对一个调用提供同样好的匹配时，应选择最特例化的版本

### 16.4 可变参数模板

  	  可变参数模板就是一个接收可变数目参数的模板函数或模板类,可变数目的参数被称为参数包，存在两种参数包：模板参数包、函数参数包

```c++
// Args是一个模板参数包；reset是一个函数参数包
// Args表示零个或多个模板类型参数
// rest表示零个或多个函数参数
template <typename T, typename... Args>
void foo(const T& t, const Args& ... rest);
```

- 编译器会推断包中参数的数目

```c++
int i = 0; double d = 3.14; string s= "hello world!";
foo(i, s, 42, d);		// 包中有3个参数
foo(d, s);				// 包中有1个参数
foo("hi");				// 空包
```

- sizeof...运算符（注意有...）

```c++
template <typename ... Args> void g(Args ... args) {
    cout << sizeof...(Args) << endl;	// 类型参数的数目
    cout << sizeof...(args) << endl;	// 函数参数的数目
}
```

#### 16.4.1 编写可变参数函数模板

​	    **可变参数函数通常是递归的。第一步调用处理包中的第一个实参，然后用剩余实参调用自身**

  ```c++
// 用来终止递归并打印最后一个元素的函数
template <typename T>	
ostream& print(ostream& os, const T& t)
{ return os << t; }
// 包中除了最后一个元素之外的其他元素都会调用这个版本print
tempalte <typename T, typename ... Args>
ostream& print(ostream& os, const T& t,const Args& ... rest)
{
 	os << t << ", ";
    return print(os, rest...);
}
  ```

#### 16.4.2 包扩展

​		扩展一个包就是将它分解为构成的元素，我们通过在模式右边放一个省略号来触发扩展操作

```c++
template <typename T, typename... Args>
ostream& print
(ostream& os,const T& t,const Args&... rest)// 扩展Args
{
    os << t << ", ";
    return print(os, rest...);				// 扩展rest
}
```

- 理解包扩展

  ​	扩展中的模式会独立地应用于包中的每个元素

```c++
template <typename... Args>
ostream& errorMsg(ostream& os, const Args&... rest)
{
    return print(os, debug_rep(rest)...);
}
```

#### 16.4.3 转发参数包

```c++
class StrVec {
public:
    template <class... Args> 
        void emplace_back(Args&&...);
};

template <class... Args> 
inline void StrVec::emplace_back(Args&&... args)
{
    chk_n_alloc();
    alloc.construct(first_free++, std::forward<Args>(args)...);
}
```

​		既扩展了模板参数包Args，也扩展了函数参数包args

```c++
// 生成元素：std::forward<Ti><ti>
svec.emplace_back(10, 'c');
// 扩展:
std::forward<int>(10), std::forward<char>(c)
```

### 16.5 模板特例化

​		一个特例化版本就是模板的一个独立的定义，在其中一个或多个模板参数被指定为特定的类型

- 定义函数模板特例化

  ​	空尖括号对指出我们将为原模版的所有模板参数提供实参

```c++
template <>
int compare(const char* const& p1,const char*const& p2)
{
    return strcmp(p1, p2);
}
```

- 函数重载与模板特例化

  ​		模板及其特例化版本应该声明在同一头文件中。所有同名模板的声明应该放在前面，然后是这些模板的特例化版本

- 类模板特例化

```c++
// 打开std命名空间，以便特例化std::hash
namespace std{
template <>
struct hash<Sales_data>
{
	typedef size_t result_type;
    typedef Sales_data argument_type;
    size_t operator()(const Sales_data&) const;
};
size_t
hash<Sales_data>::operator()(const Sales_data& s) const
{
    return hash<string>()(s.bookNo) ^ 
        	hash<unsigned>()(s.units_sold) ^
        hash<double>()(s.revenue);
}
}	// 关闭空间，注意：没有分号
```

- 类模板部分特例化

```c++
template <class T> struct remove_reference 
{ typedef T type; };
template <class T> struct remove_reference<T&> 
{ typedef T type; };
template <class > struct remove_reference<T&&>
{ typedef T type; };
```

```c++
int i;			// a, b, c均为int
remove_reference<decltype(42)>::type a;
remove_reference<decltype(i)>::type b;
remove_reference<decltype(std::move(i))>::type c;
```

- 特例化成员而不是类

```c++
template<>
void Foo<int>::Bar() { }
```





