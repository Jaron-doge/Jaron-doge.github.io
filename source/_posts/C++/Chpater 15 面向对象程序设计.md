---
title: 第十五章 面向对象程序设计
date: 2020-09-04 16:45:15
tags: C++ Primer
categories: C++ 
---

## 第十五章 面向对象程序设计

### 15.1 OOP:概述

- 继承

  ​	通过继承联系在一起的类构成一种层次关系。通常在层次关机的根部有一个**基类**，其他类直接或间接地从基类继承而来，成为**派生类**

<!-- more -->

- 虚函数

  ​	对于某些函数，基类希望它的派生类各自定义适合自身的版本，此时基类就将这些函数声明称虚函数

```c++
class Quote {
public:
	string isbn() const;
    virtual double net_price(size_t n) const;
};
```

- 派生类通过使用**类派生列表**明确指出它是从哪些基类继承而来的

```c++
class Bulk_quote : public Quote {
public:	
    double net_price(size_t) const override;
}
```

​		因为Bulk_quote在派生列表中使用了public关键字，因此我们完全可以把Bulk_quote的对象当成Quote的对象来使用

- 动态绑定

  通过使用动态绑定，我们能用同一段代码分别处理Quote和Bulk_quote对象

```c++
double print_total(ostream& os, const Quote& item, size_t n)
{
    double ret = item.net_price(n);
    os << "ISBN: " << item.isbn()
        << " # sold: " << n << " total due: " << ret << endl;
    return ret;
}
```

- **在C++中，当我们使用基类的引用（或指针）调用一个虚函数时将发生动态绑定**

### 15.2 定义基类和派生类

#### 15.2.1 定义基类

```c++
class Quote {
public:
	Quote() = default;
    Quote(const string&book, double sales_price) :
    	bookNo(book), price(sales_price) { }
    string isbn() { return bookNo; }
    
    // 派生类负责改写并使用不同的折扣计算算法
    virtual double net_price(size_t n) const
    { return n * price; }
    virtual ~Quote() = default;		// 对析构函数进行动态绑定
private:
    string bookNo;
protected:
    double price = 0.0;
};
```

- 成员函数与继承
  - 当我们使用指针或引用调用虚函数时，该调用将被动态绑定
  - 关键字virtual只能出现在类内部的声明语句之前而不能用于类外部的函数定义

- protected
  - 基类有权访问该成员，同时禁止其他用户访问

#### 15.2.2 定义派生类

```c++
class Bulk_quote : public Quote {
public:
	Bulk_quote() = default;
    Bulk_quote(const string&, double, size_t, double);
    double net_price(size_t) const override;
private:
    size_t min_qty = 0;			// 适用折扣政策的最低购买量
    double discount = 0.0;		// 以小数表示的折扣额
};
```

- 派生类中的虚函数

  ​	派生类可以显式地著名它使用某个成员函数覆盖了它继承的虚函数，在const、引用限定符后添加一个关键字override

- 派生类到基类的类型转换

  ​	因为在派生类对象中含有与其基类对应的组成部分，所以我们能把派生类的对象当成基类对象来使用，**也能将基类的指针或引用绑定到派生类对象中的基类部分上**

```c++
Quote item;
Bulk_quote bulk;
Quote *p = &item;		
p = &bulk;				// p指向bulk的Quote部分
Quote &r = bulk;		// r绑定到bulk的Quote部分
```

- 派生类构造函数

  ​	**派生类不能直接初始化从基类继承而来的成员，派生类必须使用基类的构造函数来初始化它的基类部分**

  ​	首先初始化基类部分，然后按照声明的顺序依次初始化派生类的成员

```c++
Bulk_quote(const string& book, double p, size_t qty, double disc) :
	Quote(book, p), min_qty(qty), discount(disc) { }
```

- 派生类使用基类的成员

```c++
double Bulk_quote::newt_price(size_t cnt) const
{
    if(cnt >= min_qty)
        return cnt * ( 1 - discount ) * price;
	else
        return cnt * price;
}
```

- 继承与静态成员

  ​	如果基类定义了一个静态成员，则在整个继承体系中只存在该成员的唯一定义

- 被用作基类的类

  ​	如果我们想将某个类用作基类，则该类必须已经定义而非仅仅声明

```c++
class Quote;							// 声明但未定义
class Bulk_quote : public Quote{ ... };	// 错误：Quote未定义
```

- 防止继承的发生——final

```c++
class NoDerived final { /*  */ };	// NoDerived不能作为基类
class Base { };
class Last final : Base{ };			// Last不能作为基类
```

```c++
class Bulk_quote : public Quote {
public:
	Bulk_quote() = default;
    Bulk_quote(const string&, double, size_t, double);
    double net_price(size_t);
private:
	size_t min_qty = 0;
    double discount = 0.0;
}
```

#### 15.2.3 类型转换与继承

- 智能指针类也支持派生类向基类的类型转换

- 静态类型与动态类型

  ​	表达式的静态类型在编译时总是已知的，它是变量声明时的类型或表达式生成的类型；动态类型则是变量或表达式表示的内存中的对象的类型。动态类型直到运行时才可知

```c++
double ret = item.net_price(n);
```

​		item的静态类型是Quote&，它的动态类型则依赖于item绑定的实参

- 如果表达式既不是引用也不是指针，则它的动态类型永远与静态类型一致

- 不存在从基类向派生类的隐式类型转换

- 当用派生类对象为一个基类对象赋值时，只有该派生类对象中的基类部分会被拷贝、移动或赋值，它的派生类部分将被忽略掉

### 15.3 虚函数

- 对虚函数的调用可能在运行时才被解析

- 动态绑定只有当我们通过指针或引用调用虚函数时才会发生

- 一个派生类的函数如果覆盖了某个继承而来的虚函数，则它的形参类型必须与它覆盖的基类函数完全一致

- 派生类虚函数的返回类型也必须与基类一致，当返回类型是类本身的指针或引用时例外

- **final**和**override**说明符

  - **如果我们使用override标记了某个函数，但该函数并没有覆盖已存在的虚函数，此时编译器将报错**

  ```c++
  struct B {
      virtual void f2();
  };
  struct D : B {
      void f2(int) override; // 错误：B没有形如f2(int)的函数
  }
  ```

  - 我们能把某个函数指定为final，之后任何尝试覆盖该函数的操作都将引发错误
  
  ```c++
  struct D2 : B {
      virtual void f1(int) const final;
  }
  struct D3 : D2 {
      void f1(int) const;		// 错误：D2已经将f2声明称final
  }
  ```
  
  - final和override说明符出现在形参列表以及尾置返回类型之后

- 虚函数与默认实参

  ​		如果某次函数调用使用了默认实参，则该实参值由本次调用的静态类型决定

  - 如果虚函数使用默认实参，则基类和派生类中定义的默认实参最好一致

- 回避虚函数的机制

  ​	可以使用作用域运算符强迫其执行虚函数的某个特定版本

  - 如果一个派生类虚函数需要调用它的基类版本，但是没有使用作用域运算符，则在运行时该调用将被解析为对派生类版本自身的调用，从而导致无限递归

### 15.4 抽象基类

- 纯虚函数

  ​		我们通过在函数体的位置书学=0将一个虚函数说明为纯虚函数，=0只能出现在类内部的虚函数声明语句处

  - 不能在类的内部为一个=0的函数提供函数体

```c++
class Disc_quote : public Quote {
public:
	double net_price(size_t) const = 0;
pritected:
    size_t quantity = 0;
    double discount = 0.0;
};
```

- 含有纯虚函数的类是抽象基类，**我们不能（直接）创建一个抽象基类的对象**

- 派生类构造函数只初始化它的直接基类

```c++
class Bulk_quote : public : Disc_quote {
public:
	Bulk_quote() = default;
    Bulk_quote(const string& book, double price, size_ qty, double disc) :
    Disc_quote(book, price, qty, disc) { }
    
    double net_price(size_t) const override;
};
```

### 15.5 访问控制与继承

- protected

  派生类的成员和友元只能访问派生类对象中的基类部分的受保护成员，对于普通的基类对象中的成员不具有特殊的访问权限

- 公有、私有和受保护继承

  - 派生访问说明符的目的是控制派生类用户对于基类成员的访问权限

  ```c++
  class Base {
  public:
      void pub_mem();
  protected:
      int prot_mem;
  private:
      char priv_mem;
  };
  
  struct Pub_Derv : public Base { };
  struct Priv_Derv : private Base { };
  ```

  ```c++
  Pub_Derv d1;
  Priv_Derv d2;
  d1.pub_mem();
  d2.pub_mem();		// 错误：pub_mem在派生类中是private的
  ```

  - **派生访问说明符还可以控制继承自派生类的新类的访问权限**

  ```c++
  struct Derived_from_Public : public Pub_Derv {
      int use_base() { return prot_mem; }
  };					// 正确:Base::prot_mem仍是protected的
  struct Derived_from_Private : public Priv_Derv {
      int use_base() { return prot_mem; }
  };					// 错误：Base::prot_mem是private的
  ```
  - **基类应该将其接口成员声明为共有的，同时将属于其实现的部分分成两组：一组可供派生类访问，声明为受保护的；另一组只能由基类及基类的友元访问，声明为私有的**

- 派生类向基类转换的可访问性

  - 只有当D公有地继承B时，**用户代码**才能使用派生类向基类的转换
  - 不论D以什么方式继承B，D的成员函数和友元都能转换
  - 如果D继承B的方式是公有的或者受保护的，则D的派生类的成员和友元可以使用D向B的类型转换

- 友元与继承

  - **友元关系不能继承**，基类的友元在访问派生类成员时不具有特殊性

  - 基类的访问权限由基类本身控制，即使对于派生类的基类部分也是如此

- 改变个别成员的可访问性

  - using

    ​	派生类只能为它能访问的名字提供using声明

  ```c++
  class Base {
  public:
  	size_t size() const { return n; }
  protected:
      size_t n;
  };
  
  class Derived : private Base {		// 注意：private继承
  public:
      using Base::size;
  protected:
      using Base::n;
  }；
  ```

- 默认的继承保护级别

  ​	默认情况下，使用class关键字定义的派生类是私有继承的；使用struct关键字定义的派生类是公有继承的

### 15.6 继承中的类作用域

- 一个对象、引用或指针的**静态类型**决定了该对象哪些成员是可见的

```c++
clalss Disc_quote : public Quote {
public: pair<size_t, double> discount_policy() const{ }
};
```

```c++
Bulk_quote bulk;
Bulk_quote *bulkP = &bulk;
Quote *itemP = &bulk;
bulkP->discount_policy();	// 正确：bulkP的类型是Bulk_quote*
itemP->discount_policy();	// 错误：itemP的类型是Quote*
```

- 名字冲突与继承

  ​	派生类能重用定义在其直接基类或间接基类中的名字

  - 除了覆盖继承而来的虚函数之外，派生类最好不要重用定义在基类中的名字

- 覆盖重载的函数

  ​	为重载的成员提供一条using声明语句，可以把该函数的所有重载实例添加到派生类作用域中，对派生类没有重新定义的重载版本的访问实际上是对using声明点的访问

### 15.7 构造函数与拷贝控制

#### 15.7.1 虚析构函数

- 我们通过在基类中将析构函数定义成虚函数确保执行正确的析构函数版本

```c++
class Quote {
public:
    // 如果我们删除的是一个指向派生类对象的基类指针，则需要虚析构函数
    virtual ~Quote() = default;
};
```

```c++
Quote *itempP = new Quote;		// 静态类型与动态类型一致
delete itemP;					// 调用Quote的析构函数
itemP = new Bulk_quote;			// 静态类型与动态类型不一致
delete itemP;					// 调用Bulk_quote的析构函数
```

- 虚析构函数将阻止合成移动操作
  - 基类没有移动操作意味着它的派生类也没有

#### 15.7.2 合成拷贝控制与继承

- 派生类中删除的拷贝控制与基类的关系
  
- 如果基类中的默认构造函数、拷贝构造函数、拷贝赋值运算符或析构函数是被删除的函数或者不可访问，则派生类中对应的成员将是被删除的
  
- 移动操作与继承

  ​	当派生类需要执行移动操作，可以在基类中定义合成的版本

  - 一旦基类定义了移动操作，那么必须同时显式地定义拷贝操作

```c++
class Quote {
public:
    Quote() = default;						// 默认初始化
    Quote(const Quote&) = default;			// 拷贝构造函数
    Quote(Qutoe&&) = default;				// 移动构造
    Quote& oeprator(const Quote&) = default;// 拷贝赋值
    Quote& operator=(Qutoe&&) = default;	// 移动赋值
    virtual ~Quote() = default;
}；
```

#### 15.7.3 派生类的拷贝控制成员

- 当派生类定义了拷贝或移动操作时，该操作负责拷贝或移动包括基类部分成员在内的整个对象

- 定义派生类的拷贝或移动构造函数

```c++
class Base;
class D : public Base {
public:
    D(const D& d) : Base(d) ... { }
    D(D&& d) : Base(std::move(d)) ... { }
};
```

- 派生类赋值运算符

  ​		显式调用基类赋值运算符

```c++
Base::operator=(const Base&);
D& D::operator=(const D& rhs)
{
    Base::operator=(rhs);
    ...
    return *this;
}
```

- 在构造函数和析构函数中调用虚函数

  ​		如果构造函数或析构函数调用了某个虚函数，则我们应该执行与构造函数或析构函数所属类型相对应的虚函数版本

#### 15.7.4 继承的构造函数

- 通过using继承构造函数
  - 派生类自己的数据成员将被默认初始化

```c++
class Bulk_quote : public Disc_quote {
public:
    using Disc_quote::Disc_quote;
    double net_price(size_t) const;
};
```

- using的构造函数不会改变访问级别

### 15.8 容器与继承

- 当派生类对象被赋值给基类对象时，其派生类部分将被“切掉”

- 在容器中放置（智能）指针而非对象

```c++
vector<shared_ptr<Quote>> basket;
basket.push_back(make_shared<Bulk_quote>("0-201-54848-8", 50, 10, 0.25));
cout << basket.back()->net_price(15) << endl;
```

#### 15.8.1 编写Basket类

- 我们无法直接使用对象进行编程，必须使用指针和引用。为了减少复杂性，我们经常定义一些辅助类来帮助处理

```c++
class Basket {
public:
    // Basket使用合成的默认构造函数和拷贝控制成员
	void add_item(const shared_ptr<Quote>& sale)
    { item.insert(sale); }
    double total_receipt(ostream&) const;
private:
    static bool compare(const shared_ptr<Quote>& lhs, 
                       shared_ptr<Quote>& rhs)
    { return lhs->isbn() < rhs->isbn(); }
    // multiset保存多个报价，按照compare成员排序
    std::multiset<shared_ptr<Quote>, decltype(compare)*>
        items{compare};
};

double Basket::total_receipt(ostream& os) const 
{
    double sum = 0.0;
    // upper_bound令我们跳过与当前关键字相同的所有元素
    for(auto iter = items.cbegin(); iter != item.cend();
       		iter = items.upper_bound(*iter)) {
       sum += print_total(os, **iter, items.count(*iter));
    }
    os << "Total Sale: " << sum << endl;
    return sum;
}
```

- 模拟虚拷贝

  ​		new Quote(sale)不确定分配的类型，因此给Quote类添加一个虚函数，该函数将申请一份当前对象的拷贝

```c++
class Quote {
public:
	// 该虚函数返回当前对象的一份动态分配的拷贝
    virtual Quote* clone() const & 		
    { return new Quote(*this); }
    virtual Quote* clone() &&
    { return new Quote(std::move(*this)); }
};

class Bulk_quote : public Quote {
    Bulk_quote* clone() const & 		
    { return new Bulk_quote(*this); }
    Bulk_quote* clone() && 		
    { return new Bulk_quote(std::move(*this)); }
};
```

```c++
class Basket {
public:
    void add_item(const Quote& sale)
    {items.insert(std::shared_ptr<Quote>(sale.clone()));}
    void add_item(Quote&& sale)
    { items.insert(
    	std::shared_ptr<Quote>(std::move(sale).clone()));}
};
```

- sale的动态类型决定了到底运行Quote的函数还是Bulk_quote的函数

### 15.9 文本查询程序再探

- 单词查询

  ​		得到匹配的行

- 逻辑非查询

  ​		得到不匹配的行

- 逻辑或查询

  ​		返回匹配两个条件中的一个的行

- 逻辑与查询

  ​		返回匹配全部两个条件的行

#### 15.9.1 面向对象的解决方案

- 我们应该将几种不同的查询建模成互相独立的类，这些类共享一个公共基类

- 抽象基类

 ![image-20200902165615876.png](https://i.loli.net/2020/09/04/kliUGRSczaFJXVC.png)	

![image-20200902170106157.png](https://i.loli.net/2020/09/04/ND2T4RpcMykmJbr.png)

#### 15.9.2 Query_base类和Query类

```c++
class Query_base {
	friend class Query;
protected:
    using line_no = TextQuery::line_no;
    virtual ~Query_base() = default;
private:
    // eval返回与当前Query匹配的QueryResult
    virtual QueryResult eval(const TextQuery&) const = 0;
    // rep是表示查询的一个string
    virtual string rep() const = 0;
};
```

```c++
class Query {
	friend Query operator~(const Query&);
    friend Query operator|(const Query&, const Query&);
    friend Query operator&(const Query&, const Query&);
public:
    Query(const string&);
    QueryResult eval(const TextQuery& t) const 
    { return q->eval(t); }
    string rep() const
    { return q->rep(); }
private:
    Query(shared_ptr<Query_base> query) : q(query) { }
    shared_ptr<Query_base> q;
};

ostream& operator<<(ostream& os, const Query& query)
{
    return os << query.rep();
}

inline Query::Query(const string& s) : q(new WordQuery(s)){ }
```

#### 15.9.3 派生类

- WordQuery类

```c++
class WordQuery : public Query_base {
    friend class Query;			// Query使用WordQuery构造函数
    WordQuery(const string& s) : query_word(s) { }
	QueryResult eval(const TextQuery& t) const 
    { return t.query(query_word); }
    string rep() const { return query_word; }
    string query_word;
};
```

- NotQuery类及~运算符

```c++
class NotQuery : public Query_base {
    friend Query operator~(const Query &);
    NotQuery(const Query& q) : query(q) { }
    string rep() const { return "~(" + query.rep() + ")"; }
    QueryResult eval(const TextQuery&) const;
    Query query;
};
inline Query operator~(const Query& operand)
{ return shared_ptr<Query_base>(new NotQuery(operand)); }
```

- BinaryQuery类

```c++
class BinaryQuery : public Query_base {
protected:
    BinaryQuery(const Query& l, const Query& r, string s) :
    	lhs(l), rhs(r), opSym(s) { }
    string rep() const { return "(" + lhs.rep() + " " 
        				+ opSym + " " + rhs.rep() + ")"; }
    Query lhs, rhs;
    string opSym;
}
```

- AndQuery类

```c++
class AndQuery : public BinaryQuery {
    friend Query operator&(const Query&, const Query&);
        AndQuery(const Query& left, const Query& right) :
    		BinaryQuery(left, right, "&") { }
    // 具体的类：AndQuery继承了rep并且定义了其他纯虚函数
    QueryResult eval(const TextQuery&) const;
};
inline Query operator&(const Query& lhs, const Query& rhs)
{
    return shared_ptr<Query_base>(new AndQuery(lhs, rhs));
}
```

- OrQuery类

```c++
 class OrQuery : public BinaryQuery {
     friend Query operator|(const Query&, const Query&);
     OrQuery(const Query& left, const Query& right) :
     		BinaryQuery(left, right, "|") { }
     QueryResult eval(const TextQuery&) const;
 };
inline Query oeprator|(const Query& lhs, const Query& rhs)
{
     return shared_ptr<Query_base>(new OrQuery(lhs, rhs));
}
```

#### 15.9.4 eval函数

- OrQuery::eval

```c++
QueryResult OrQuery::eval(const TextQuery& text) const 
{
    auto right = rhs.eval(text), left = lhs.eval(text);
    auto ret_lines = make_shared<set<line_no>>(left.begin(), 												left.end());
    ret_lines->insert(right.begin(), right.end());
    return QueryResult(rep(), ret_lines, left.get_file());
}
```

- AndQuery::eval

```c++
QueryResult AndQuery::eval(const TextQuery& text) const 
{
    auto right = rhs.eval(text), left = lhs.eval(text);    
	auto ret_lines = make_shared<set<line_no>>();
    set_intersection(left.begin(), left.end(), 
            right.begin(), right.end(), 
            inserter(*ret_lines, ret_lines->begin()));
    return QueryResult(rep(), ret_lines, left.get_file());
}
```

- NotQuery::eval

```c++
QueryResult NotQuery::eval(const TextQuery& text) const
{
    auto result = query.eval(text);
	auto ret_lines = make_shared<set<line_no>>();
    auto beg = result.begin(), end = result.end();
    auto sz = result.get_file()->size();
    for(size_t n = 0; n != sz; ++n) {
        if(beg == end || *beg != n)
            ret_lines->insert(n);
        else if(beg != end)
            ++beg;
    }
    return QueryResult(rep(), ret_lines, result.get_file());
}
```

