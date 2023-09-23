---
title: 第六章 以template进行编程
date: 2020-01-04 21:16:25
tags: Essential C++
categories: C++
---
## 第六章 以template进行编程

### 6.0 Binary tree

<!-- more -->

- tree由结点(node 或 vertice) 以及连接不同结点的链接(link)组成。

- 左子节点(left child)、右子节点(right child)，根节点(root)、叶结点(leaf)。

- insert、remove、find、clear、print

- 包含两个class: BinaryTree储存一个指向根节点的指针，BTnode存储节点实值以及连接至左、右两个子节点的链接。

- 遍历

  ![image-20200104231238849.png](https://i.loli.net/2020/01/04/k5HXq9MRjQbGNc6.png)

  - 前序(preorder): 节点—>左子树—>右子树

    Piglet, Eeyore, Chris, Kanga, Roo, Pooh, Tigger

  - 中序(inorder): 左子树—>节点—>右子树

    Chris, Eeyore, Kanga, Piglet, Pooh, Roo, Tigger

  - 后序(postorder): 左子树—>右子树—>节点

    Chris, Kanga, Eeyore,  Pooh, Tigger, Roo, Piglet



### 6.1 被参数化的类型

#### 6.1.1 template

```c++
template <typename valType>
class string_BTnode{
public:
    // ...
private:
    valType _val;
    int _cnt;
    string_BTnode *_lchild;
    string_BTnode *_rchild;
}
```



1. friend

```c++
template <typename Type>
class BinaryTree;	// 前置声明

template <typename valType>
class BTnode {
    // 以下操作使得BinaryTree的全部member function都成为
    // BTnode的friend
    friend class BinaryTree<valType>;
    // ...
};
```

2. 实例化

   将实际类型绑定至class template。

```c++
BTnode<int> bti;
BTnode<string> bts;
// “代入string而产生出来的BinaryTree”是“代入string所产生出来的BTnode”的friend，不是“代入int所产生出来的BTnode”的friend。
```



```c++
template <typename elemType>
class BinaryTree {
public:
    // ...
private:
    // BTnode必须以template parameter list加以限定
    BTnode<elemType> *_root;
};
```



### 6.2 Class Template 的定义

1. 

```c++
template <typename elemType>
class BinaryTree {
public:
    BinaryTree();
    BinaryTree(const BinaryTree&);
    ~BinaryTree();
    BinaryTree& operator=(const BinaryTree&);
    
    bool empty() {return _root == 0;}
    void clear();
private:
    BTnode<elemType> *_root;
    
    // 将src所指子树(subtree)复制到tar所指子树。
    void copy(BTnode<elemType>*tar, 		  			BTnode<elemType>*src);
};
```

2. 为class template定义一个inline函数，正常定义，如empty()。

   但是在类体外，定义语法则不同。

```c++
template <typename elemType>
inline BinaryTree<elemType>::
BinaryTree() : _root(0) 
{}
// 在class scope运算符出现后，其后所有的东西
// 都被视为位于class定义范围内。

// copy constructor
template<typename elemType>
inline BinaryTree<elemType>::
BinaryTree(const BinaryTree& rhs)
	{copy(_root, rhs._root);}

// destructor
template<typename elemType>
inline BinaryTree<elemType>::
~BinaryTree()
	{clear();}

// copy assignment operator
template<typename elemType>
inline BinaryTree<elemType>&
BinaryTree<elemType>::
operator=(const BinaryTree &rhs)
{
    if(this != &rhs) {
        clear();
        copy(_root, rhs._root);
    }
    return *this;
}
```



### 6.3 Template 类型的参数

1. constructor初始化

```c++
template<typename valType>
inline BTnode<valType>::
BTnode(const valType &val)
    // 将valType视为某种class类型
    : _val(val)	// 不建议在函数体内赋值，因为可能是class类型
{
	_cnt = 1;
        _lchild  = _rchild = 0;
}

```



### 6.4 实现Class Template

#### 6.4.1 insert

1. insert

```c++
template <typename elemType>
inline void
BinaryTree<elemType>::
insert(const elemType &elem)
{
    if(!_root)
        _root = new BTnode<elemType>(elem);
    else
        _root->insert_value(elem);
}
```

2. insert_value

```c++
template <typename valType>
void BTnode<valType>::
insert_value(const valType &val)
{
    if(val == _val) {
        _cnt++;
        return;
    }
    
    if(val < _val) {
        if(!_lchild)
            _lchild = new BTnode(val);
        else
            _lchild->insert_value(val);
    }
    
    else {
        if(!_rchild)
            _rchild = new BTnode(val);
        else
            _rchild->insert_value(val);
    }
}
```

#### 6.4.2 remove

以节点的右子节点取代节点本身，然后搬移左子节点，使它成为右子节点的左子树的叶节点。如果此刻并无右子节点，那么就以左子节点取代节点本身。

```c++
template <typename elemType>
inline void
BinaryTree<elemType>::
remove(const elemType &elem)
{
    if(_root) {
        if(_root->val == elem)
            remove_root();	// 根节点的移除操作以特例处理
        else
            _root->remove_value(elem, _root);
    }
}
```

​	无论remove_root()或remove_value()，都会搬移左子节点，使它成为右子节点的左子树的叶结点。将操作剥离至lchild_leaf()

```c++
template <typename valType>
void BTnode<valType>::
lchild_leaf(BTnode *leaf, BTnode *subtree)
{
    while(subtree->_lchild)
        subtree = subtree->_lchild;
    subtree->_lchild = leaf;
} 
```

1.  remove_root

   如果根节点拥有任何子节点，remove_root()就会重设根节点。如果右子节点存在，就以右子节点取而代之；如果左子节点存在，就直接搬移，或通过lchild_leaf()完成。如果右子节点为null，_root便以左子节点取代。

```c++
template <typename elemType>
void BinaryTree<elemType>::
remove_root()
{
    if(!_root) 
        return;
    
    BTnode<elemType> *tmp = _root;
    if(_root->_rchild) {
        _root = _root->_rchild;
        
        // 好啦，现在我们必须将左子节点搬移到
        // 右子节点的左子树的底部
        if(tmp->_lchild) {
            // 为了可读性，分解如下。
            BTnode<elemType> *lc = tmp->_lchild;
            BTnode<elemType> *newlc = _root->_lchild;
            if(!newlc)
                // 没有任何子树，那么就直接接上
                _root->_lchild= lc;
            // lchild_leaf()会遍历整个左子树
            // 寻找某个可接上去的null左子节点。
            // lchild_leaf是个static member function。
            else BTnode<elemType>::lchild_leaf(lc,newlc);
        }
    }
    else _root = _root->_lchild;
    
    delete tmp;	// ok：现在我们已移去先前的那个根节点了
}
```

2. remove_value

   remove_value()拥有两个参数，将被删除的值(如果存在的话)以及一个指针，指向目前关注的节点的父节点。

```c++
template <typename valType>
void BTnode<valType>::
remove_value(const valType &val, BTnode *& prev);
```

​	两个参数皆以传址(by reference)方式传递，这是为了避免“当valType被指定为class类型时，因传值(by value)而产生的昂贵的复制开销”。

​	第二个参数用reference to pointer。如果以pointer传递，只能改变pointer所指之物，而不是pointer本身。

```c++
template <typename valType>
void BTnode<valType>::
remove_value(const valType &val, BTnode *& prev)
{
    if(val < _val) {
        if(!_lchild)
            return;	// 不在此二叉树中
        else 
            _lchild->remove_value(val, _lchild);
        // 指向下一个
    }
    else if(val > _val) {
        if(!_rchild)
            return;	// 不在此二叉树中
        else 
            _rchild->romove_value(val, _rchild);
    }
    else {
        // ok: 找到了
        // 重置此树，然后删除这一节点
        
        if(_rchild) {
            prev = _rchild;
            // 如果根节点的左子节点存在
            if(_lchild)
                // 如果右子节点的左子树点不存在
                if(! prev->_lchild)
                    // 根节点的左子节点给右子节点
                    prev->_lchild = _lchild;
            	// 如果右子节点的左子树存在
            	else
                    // 转移到最左边的那一个
                    BTnode<valType>::
            		lchild_leaf(_lchild, prev->_lchild);
        }
        else prev = _lchild;
        delete this;
    }
}
```

### 6.4.3 clear

​	删除整棵二叉树。

```c++
template <typename elemType>
class BinaryTree {
public:
    void clear()
    {
        if(_root)
            clear(_root);
        _root = 0;
    }
    // ...
private:
    // 重载版本
    void clear(BTnode<elemType>*);
    // ...
};
```



定义

```c++
template <typename elemType>
void BinaryTree<elemType>::
clear(BTnode<elemType> *pt)
{
    if(pt) {
        clear(pt->_lchild);
        clear(pt->_rchild);
        delete pt;
    }
}
```



### 6.4.4 order

​	三个算法的差别仅在于三个操作的执行顺序

1. preorder

```c++
template<typename valType>
void BTnode<valType>::
preorder(BTnode *pt, ostream &os) const
{
    if(pt) {
        display_val(pt, os);
        if(pt->_lchild)
            preorder(pt->_lchild, os);
        if(pt->_rchild)
            preorder(pt->_rchild, os);
    }
}
```

2. inorder

```c++
template<typename valType>
void BTnode<valType>::
inorder(BTnode *pt, ostream &os) const
{
    if(pt) {
        if(pt->_lchild)
            preorder(pt->_lchild, os);
        display_val(pt, os);
        if(pt->_rchild)
            preorder(pt->_rchild, os);
    }
}
```

3. postorder

```c++
template<typename valType>
void BTnode<valType>::
postorder(BTnode *pt, ostream &os) const
{
    if(pt) {
        if(pt->_lchild)
            preorder(pt->_lchild, os);
        if(pt->_rchild)
            preorder(pt->_rchild, os);
         display_val(pt, os);
    }
}
```



### 6.5 output运算符

```c++
// function template版本
template <typename elemType>
inline ostream&
operator<<(ostream& os, const BinaryTree<elemType> &bt)
{
    os << "Tree: " << endl;
    bt.print(os);
    return os;
}
```



### 6.6 常量表达式与默认参数值

1. Template参数并不是非得已某种类型不可，也可以用常量表达式作为template参数。

```c++
template <int len>
class num_sequence {
public:
num_sequence(int beg_pos = 1);
    // ...
};

template <int len>
class Fibonacci:public num_sequence<len> {
public:
Fibonacci(int beg_pos = 1)
    : num_sequence<len>(beg_pos){}
    // ...
};
```

2. 默认值

```c++
template<int len, int beg_pos>
class num_sequence{ ... };

template<int len, int beg_pos = 1>
class Fibonacci:public num_sequence<len,beg_pos>
{ ... };


例：
    // 下面这行会被展开为
    // num_sequence<32,1> *pnslto32 = new Fibonacci<32,1>;
    num_seuquence<32> *pnslto32 = new Fibonacci<32>;
```

3. 地址也是一种常量表达式

```c++
template <void (*pf)(int pos, vector<int> &seq)>	// 函数指针
class numeric_sequence {
public:
	numeric_sequence(int len, int beg_pos = 1)
    {
        if(!pf)
            return ;
        
        _len = len > 0 ? len : 1;
        _beg_pos = beg_pos > 0 ? beg_pos : 1;
        pf(beg_pos + len - 1, _elems);
    }
	
private:
	int 		_len;
    int 		_beg_pos;
    vector<int> _elems;
}

// pf是一个指向“依据特定数列类型，产生pos个元素，放到vector内”的函数
void fibonacci(int pos, vector<int> &seq);
numeric_sequence<fibonacci> ns_fib(12);
```



### 6.7 以Template参数作为设计策略

#### 6.7.1 LessThan function object

```c++
template <typename elemType>
class LessThan {
public:
	LessThan(const elemType &val) : _val(val) {}
    bool operator()(const elemType &val) const 
    			   { return val < _val;}
    
    void val(const elemType &newval) {_val = newval;}
    elemType val() const {return _val;}
private:
    elemType _val;
};

```

```c++
// less<>(a, b) {return a < b;}
template <typename elemType, typename Comp = less<elemType> >
class LessThanPred {
public:
	LessThanPred(const elemType &val) : _val(val) {}
    bool operator() (const elemType &val) const
    {return Comp(val, _val);}
    
    void val(const emeTYple &newval) {_val = newval;}
    elemType val() const {return _val;}
private:
	elemType _val;
}

// 字符串比较版本
class StringLen {
public:
	bool operator()(const string &s1, const string &s2)
    {return s1.size() < s2.size();}
}

// 调用
LessThanPred<int> ltpi(1024);
LessThanPred<string, StringLen> ltps("Pooh");
```

### 6.8 Member Template Function

1. member template function

```c++
class PrintIt {
public:
	PrintIt(ostream &os)
        : _os(os) {}
    // 下面是一个member template function
    template <typename elemType>
    void print(const elemTYpe &elem, char delimiter = '\n')
    { _os << elem << delimiter;}
private:
	ostream& _os;
};

// 调用
PrintIt to_standard_out(cout);
to_standard_out.print("hello");
```

2. Class template内也可以定义member template function.

```c++
template <typename OutStream>
class PrintIt {
public:
	PrintIt(OutStream &os)
        : _os(os) {}
    // ...
};
```



