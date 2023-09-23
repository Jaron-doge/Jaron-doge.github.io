---
title: 第十三章 拷贝控制
date: 2020-09-04 16:45:13
tags: C++ Primer
categories: C++ 
---

## 第十三章 拷贝控制

- 拷贝构造函数
- 拷贝赋值运算符
- 移动构造函数
- 移动构造运算符
- 析构函数

<!-- more -->

### 13.1 拷贝、赋值与销毁

#### 13.1.1 拷贝构造函数

​		如果一个构造函数的第一个参数是自身类类型的引用，且任何额外参数都有默认值，则此构造函数是拷贝构造函数。

- 拷贝初始化

  ​	当使用直接初始化时，我们实际上是要求编译器使用普通的函数匹配来选择与我们提供的参数最匹配的构造函数。当使用拷贝初始化时，我们要求编译器将右侧运算对象拷贝到正在创建的对象中，如果需要的话进行类型转换。

  - 当我们调用insert或push成员时，容器会对其元素进行拷贝初始化。用emplace成员创建的元素都进行直接初始化。

- **拷贝构造函数自己的参数必须是引用类型**，不然形参需要拷贝实参，实参又要调用拷贝构造函数

#### 13.1.2 拷贝赋值运算符

- 重载赋值运算符

  ​	赋值运算符通常应该返回一个指向其左侧运算对象的引用

```c++
Sales_data& Sales_data::operator=(const Sales_data& rhs)
{
    bookNo = rhs.bookNo;
    units_sold = rhs.units_sold;
    revenue = rhs.revenue;
    return *this;
}
```

#### 13.1.3 析构函数

​		在一个析构函数中，首先执行函数体，然后销毁成员。成员按初始化顺序的逆序销毁。

```c++
~Sales_data();
```

- 变量在离开其作用域时被销毁
- 当一个对象被销毁时，其成员被销毁
- 容器被销毁时，其元素被销毁
- 对于动态分配的对象，delete时被销毁
- 对于临时对象，当创建它的完整表达式结束时被销毁

- 成员是在析构函数体之后隐含的析构阶段中被销毁的

#### 13.1.4 三/五法则

- 如果一个类需要一个析构函数，它也需要一个拷贝构造函数和一个拷贝赋值运算符

#### 13.1.5 使用=default

​		我们可以通过将拷贝控制成员定义为=default来显示地要求编译器生成合成的版本

```c++
class Sales_data {
public:
	Sales_data() = default;
 	Sales_data(const Sales_data&) = default;
    Sales_data& operator=(const Sales_data&);
    ~Sales_data() = default;
}
```

#### 13.1.6 阻止拷贝

- 定义删除的函数

  ​	在函数的参数列表后面加上=delete来指除我们虽然声明了它们，但不能以任何方式使用它们。

```c++
struct NoCopy {
public:
	NoCopy() = default;				// 使用合成的默认构造函数
    NoCopy(const NoCopy&) = delete;				// 阻止拷贝
    NoCopy &operator=(const NoCopy) = delete;	// 阻止赋值
    ~NoCopy() = default;			// 使用合成的析构函数
}
```

- 如果一个类有数据成员不能默认构造、拷贝、复制或销毁，则对应的成员函数将被定义为删除的。

- 在新标准发布之前，类是通过将其拷贝构造函数和拷贝赋值运算符声明为private来阻止拷贝的

```c++
class Employee {
public:
    Employee(){};
    Employee(const string& name) : _name(name)
    {
        static int i = 0;
        _ID = i++;
    };
    Employee(const Employee&) = delete;
    Employee& operator=(const Employee&) = delete;
private:
    string _name;
    int _ID;
}
```

### 13.2 拷贝控制和资源管理

- 首先确定类型对象的拷贝语义：定义拷贝操作，使类的行为看起来像一个值或者像一个指针

#### 13.2.1 行为像值的类

- 每个对象都应该拥有一份自己的拷贝。

```c++
// 通过先拷贝右侧运行对象，可以处理自赋值情况
HasPtr& HasPtr::operator(const HasPtr& rhs)
{
    auto newp = new string(*rhs.ps);	
    delete ps;
    ps = newp;
    i = rhs.i;
    return *this;
}
```

- 当编写赋值运算符时，需要记住两点:
  - **如果将一个对象赋予它自身，赋值运算符必须能正常工作**
  - 大多数赋值运算符组合了析构函数和拷贝构造函数的工作

#### 13.2.2 定义行为像指针的类

- 引用计数
  - 每个构造函数要创建一个引用计数，将计数器初始化为1
  - 拷贝构造函数递增共享的计数器
  - 析构函数递减计数器
  - 拷贝赋值运算符递增右侧对象的计数器，递减左侧运算对象的计数器

- 定义一个使用引用计数的类

```c++
class HasPtr {
public:
    HsaPtr(const string&s = string()): ps(new string(s)), i(0), use(new size_t(1)){}
    HasPtr(const HasPtr &p):
    	ps(p.ps), i(p.i), use(p.use){++*use;}
    HasPtr& operator = (const HasPtr&);
    ~HasPtr();
private:
    string *ps;
    int i;
    size_t *use;
}
```

### 13.3 交换操作

- 每个swap调用应该都是未加限定的，即每个调用都应该是swap。如果存在类型特定的swap版本，其匹配程度会优于std中定义的版本。

```c++
class HasPtr {
 	friend void swap(HasPtr&, HasPtr&);
}
inline void swap(HasPtr& lhs, HasPtr& rhs)
{
    using std::swap;
    swap(lhs.ps, rhs.ps);		// 交换指针，而不是string数据
    swap(lhs.i, rhs.i);	
}
```

### 13.4 拷贝控制示例

![image-20200829214205850.png](https://i.loli.net/2020/09/04/n7aKDQuiALtvlHj.png)



- Message类

```c++
class Message {
    friend void swap(Message&, Message&);
    friend void swap(Floder&, Folder&);
	friend class Floder;
public:
	// floders被隐式初始化为空集合
    explicit Message(const string& str = " ")
        contents(str) {}
    // 拷贝控制成员，用来管理指向本Message的指针
    Message(const Message&);			// 拷贝构造函数
	Message operator=(const Message&);	// 拷贝赋值运算符
    ~Message();
    // 从给定Folder集合中添加/删除本Message
    void save(&Folder);
    void remove(&Folder);
private:
    string contents;				// 实际消息文本
    set<Folder*> folders;			// 包含本Message的Folder
    // 拷贝构造函数、拷贝赋值运算符和析构函数所使用的工具函数
    // 将本Message添加到指向参数的Folder中
    void add_to_Folders(const Message&);
    // 从folders中的每个Folder中删除本Message
    void remove_from_Folders();
};
```

```c++
void Message::save(Folder& f)
{
    folders.insert(&f);	// 将给定Folder添加到我们的Folder列表中
    f.addMsg(this);		// 将本Message添加到f的Message集合中
}
void Message::remove(Folder& f)
{
    folders.erase(&f);	// 将给定Folder从我们的Floder列表中删除
    f.remMsg(this);		// 将本Message从f的Message集合中删除
}
void Message::add_to_Folders(const Message& m)
{
    for(auto f : m.floders)		// 对每个包含m的Folder
        f->addMsg(this);		// 向该Folder添加一个指向本									   Message的指针
}
Message::Message(const Message& m) :
	contents(m.contents), folders(m.folders)
{
	add_to_Folders(m);		// 将本消息添加到指向m的Folder中   
}
void Message::remove_from_Folders()
{
	for(auto f : folders)		// 对folders中每个指针
        f->remMsg(this);		// 从该Folder中删除本Message
}
Message::~Message()
{
    remove_from_Folders();
}
Message& Message::(const Message& rhs)
{
    remove_from_Folders();		// 更新已有Folder
    contents = rhs.contents;	// 从rhs拷贝消息内容
    folders = rhs.folders;		// 从rhs拷贝Folder指针
    add_to_Folders(rhs);		// 将本Message添加到Folder中
    return *this;
}
```

- Message的swap函数

  我们通过两边扫描folders中每个成员来正确处理Folder指针。全部删除——交换——重新添加

```c++
void swap(Message& lhs, Message& rhs)
{
    using std::swap;
    for(auto f : lhs.folders)
        f->remMsg(&lhs);
    for(auto f : rhs.folders)
        f->remMsg(&rhs);
    swap(lhs.contents, rhs.contents);
    swap(lhs.folders, rhs.folders);
    for(auto f : lhs.folders)
        f->addMsg(&lhs);
    for(auto f : rhs.folders)
        f->addMsg(&rhs);
}
```

### 13.5 动态内管理类

- 类vector类内存分配策略的简化实现

```c++
class StrVec {
public:
    StrVec() : 
    	elements(nullptr), first_free(nullptr), cap(nullptr){ }
    StrVec(const StrVec&);
    StrVec& operator=(const StrVec&);
    ~StrVec();
    void push_back(const std::string&);
    size_t size() const { return first_free - elements; }
    size_t capacity() const { return cap - elements; }
    std::string *begin() const { return elements; }
    std::string *end() const { return first_free; }
private:
    std::allocator<std::string>> alloc;	// 分配元素
    void chk_n_alloc()
    { if(size() == capacity()) reallocate(); }
    // 工具函数，被拷贝构造函数、赋值运算符和析构函数所使用
    std::pair<std::string*, std::string*> alloc_n_copy
        (const std::string*, const std:: string*);
    void free();				// 销毁元素并释放内存
    void reallocate();			// 获得更多内存并拷贝已有元素
	std::string* elements;		// 指向数组首元素的指针
    std::string* first_free;	// 指向数组第一个空闲元素的指针
    std::string* cap;			// 指向数组尾后位置的指针
};
```

- 使用construct

``` c++
void StrVec::push_back(const string& s)
{
    chk_n_alloc();
    alloc.construct(first_free++, s);
}

pair<string*, string*> StrVec::alloc_n_copy(const string* b, const string* e)
{
    auto data = alloc.allocate(e - b);
    return {data, uninitialized_copy(b, e, data) };
}

void StrVec::free()
{
    if(elements) {
        for_each(elements, first_free,[](string& rhs)
                 { alloc.destroy(&rhs); } );
        alloc.deallocate(elements, cap-elements);
    }
}

StrVec::StrVec(const StrVec& s)
{
    // 调用alloc_n_copy分配空间以容纳与s中一样多的元素
	auto newdata = alloc_n_copy(s.begin(), s.end());
    elements = newdata.first;
    first_free = cap = newdata.second;
}

StrVec::~StrVec()
{
    free();
}

StrVec& operator=(const StrVec& rhs)
{
    auto data = alloc_n_copy(rhs.begin(), rhs.end());
    free();
    elements = data.first;
    first_free = cap = data.second;
    return *this;
}
```

- reallocate
  - 为一个新的、更大的string数组分配内存
  
  - 在内存空间的前一部分构造对象，保存现有元素

  - 销毁原内存空间的元素，并释放这块内存
  
    在reallocate中，我们需要拷贝再销毁string数据，如果我们能
  
  避免分配和释放string的额外开销，会提升StrVec的性能。
  
- 移动构造函数和std::move

  ​	当我们使用move时，直接调用std::move而不是move

```c++
void StrVec::reallocate()
{
    // 分配两倍新内存，StrVec为空则分配容纳一个元素的空间
    auto newcapacity = size() ? 2 * size() : 1;
    // 分配新内存
	auto newdata = alloc.allocate(newcapacity);
    // 将数据从旧内存移到新内存
    auto dest = newdata;		// 指向新数组中下一空闲位置
    auto elem = elements;		// 指向旧数组中下一个元素
    for(size_t i = 0; i != size(); ++i)
        alloc.construct(dest++, std::move(*elem++));
    free();						// 移动完释放旧内存空间
    // 更新我们的数据结构，执行新元素
	elements = newdata;
    first_free = dest;
    cap = elements + newcapacity;
}
```

### 13.6 对象移动

- IO类或unique_ptr这样的类不能被拷贝但可以移动

#### 13.6.1 右值引用

- 右值引用只能绑定到临时对象

- 不能将一个右值引用直接绑定到一个左值上

```c++
int& r = i;						// 左值引用
const int& r2 = i * 42;			
int&& rr2 = i * 42;				// 将rr2绑定到乘法结果上
```

- 不能将一个右值引用绑定到一个右值引用类型的变量上

- 标准库move函数

  我们可以显示地将一个左值转换为对应的右值的引用类型

```c++
int&& rr3 = std::move(rr1);
```

- 我们可以销毁一个移后源对象，也可以赋予它新值，但不能使用一个移后源对象的值

#### 13.6.2 移动构造函数和移动赋值运算符

- 任何额外的参数都必须由默认实参
- noexcept承诺函数不抛出异常
- 必须在类头文件的声明和定义中都指定noexcept
- 移动构造函数

```c++
StrVec::StrVec(StrVec&& s) noexcept	// 移动操作不应抛出任何异常
	//成员初始化器接管s中的资源
    : elements(s.elements), first_free(s.first_free), cap(s.cap)
{
    // 令s进入这样的状态——对其运行析构函数是安全的
	s.elements = s.first_free = cap = nullptr;        
}
```

- 移动赋值运算符

```c++
StrVec& StrVec::operator=(StrVec&& rhs) noexcept
{
    // 直接检测自赋值
    if(this != &rhs) {
        free();
        elements = rhs.elements;
        first_free = rhs.first_free;
        cap = rhs.cap;
        rhs.elements = rhs.first_free = rhs.cap = nullptr;
    }
    return *this;
}
```

- 合成的移动操作

  - 只有当一个类没有任何自己版本的拷贝控制成员，且类的每个非static数据成员都可以移动时，编译器才会为它合成移动构造函数或移动赋值运算符
  - 有类成员定义了自己的拷贝构造函数且未定义移动构造函数，则被定义为删除的

  - 定义了一个移动构造函数或移动赋值运算符的类必须也定义自己的拷贝操作。否则，这些成员默认地被定义为删除的

- 移动右值，拷贝左值

```c++
StrVec v1, v2;
v1 = v2;					// v2是左值，使用拷贝赋值
StrVec getVec(istream&);	// getVec返回一个右值
v2 = getVec(cin);			// getVec(cin)是一个右值，移动赋值
```

- 如果没有移动构造函数，右值也被拷贝

- 移动迭代器
  - 移动迭代器的解引用运算符生成一个右值引用
  - **可以将移动迭代器传递给uninitialized_copy**
  - make_move_iterator接受一个迭代器参数，返回一个移动迭代器

```c++
void StrVec::reallocate()
{
    auto newcapacity = size() ? 2 * size() : 1;
    auto first = alloc.allocate(newcapacity);
    // 移动元素
    auto last = uninitialized_copy(make_move_iterator(begin()), 		make_move_iterator(end()), first);
    free();
    elements = first;
    first_free = last;
    cap = elements + newcapacity;
}
```

#### 13.6.3 右值引用和成员函数

- 允许移动的成员函数通常由两个版本，一个版本接收一个指向const的左值引用，第二个版本接受一个指向非const得右值引用

```c++
void push_back(const X&);		// 拷贝：绑定到任意类型得X
void push_back(X&&);			// 移动：只能绑定到X可修改的右值
```

- 当我们调用函数时，实参类型决定了新元素是拷贝还是移动到容器中

```c++
string s = " ";
vec.push_back(s);			// 调用push_back(const string&)
vec.push_back("done");		// 调用push_back(string&&)
```

- 引用限定符

  ​		为了防止向右值赋值(s1 + s2 = "wow"是可以发生的)，我们指出this的左值/右值属性的方式与定义const成员函数相同，即在参数列表后放置一个引用限定符

  - **引用限定符可以是&或&&，分别指出this可以指向一个左值或右值**
  - 引用限定符只能用于非static成员函数,**且必须同时出现在函数的声明和定义中**

```c++
class Foo {
public:
	Foo& operator=(const Foo&) &;	// 只能向可修改的左值赋值
};
```

- 一个函数可以同时用const和引用限定，引用限定符必须跟随在const后

```c++
class Foo {
public:
	Foo someMem() const &;
};
```

- 当我们定义两个或两个以上具有相同名字和相同参数列表的成员函数，就必须对所有函数都加上引用限定符，或者所有都不加