---
title: 2 js 语法
date: 2021-8-2 00:00:00
tags: js
categories: 前端
---

## js 语法

<!-- more -->

### 1. js 输入输出语句

| **方法**         | **说明**                       | **归属** |
| ---------------- | ------------------------------ | -------- |
| alert(msg)       | 浏览器弹出警示框               | 浏览器   |
| console.log(msg) | 浏览器控制台打印输出信息       | 浏览器   |
| prompt(info)     | 浏览器弹出输入框，用户可以输入 | 浏览器   |

```html
var name = prompt('输入姓名');
alert(name);
```

### 2. 变量

| **情况**                      | **说明**               | **结果**  |
| ----------------------------- | ---------------------- | --------- |
| var age ; console.log (age);  | 只声明 不赋值          | undefined |
| console.log(age)              | 不声明 不赋值 直接使用 | 报错      |
| age  = 10; console.log (age); | 不声明  只赋值         | 10        |

### 3. 数据类型

**JavaScript** **是一种弱类型或者说动态语言。**这意味着不用提前声明变量的类型，在程序运行过程中，类型会被自动确定。

```js
var age = 10;				// 数字型
var areYouOk = '是的';	   // 字符型
```

在代码运行时，变量的数据类型是由 JS引擎 根据 = 右边变量值的数据类型来判断 的，运行完毕之后， 变量就确定了数据类型。

JavaScript 拥有动态类型，同时也意味着相同的变量可用作不同的类型：

```js
var x = 6;			// x为数字
var x = 'Bill';		// x为字符串
```

#### 3.1 简单数据类型

![1.png](https://i.loli.net/2021/08/02/TLO1IPRhJjqaHVG.png)

##### 3.1.1 数字型三个特殊值

- Infinity ，代表无穷大，大于任何数值

- -Infinity ，代表无穷小，小于任何数值

- NaN ，Not a number，代表一个非数值

##### 3.1.4 Undefined 和 Null

一个声明后没有被赋值的变量会有一个默认值 undefined ( 如果进行相连或者相加时，注意结果）

```js
var variable;
console.log(variable);				// undefined
console.log('你好' + variable);	   // 你好undefined
console.log(11 + variable);			// NaN
console.log(true + variable);		//  NaN
```

一个声明变量给 null 值，里面存的值为空

```js
var vari = null;
console.log('你好' + vari);  		// 你好null
console.log(11 + vari);     	 // 11
console.log(true + vari);   	 //  1
```

#### 3.2 typeof

```js
var num = 18;
console.log(typeof num) // 结果 number      
```

#### 3.3 数据类型转换

![2.png](https://i.loli.net/2021/08/02/hpMuOKcFIlHgiP5.png)

![3.png](https://i.loli.net/2021/08/02/oxbztX2KLRn3VJ8.png)

### 4. 运算符

#### 4.1 浮点数的精度问题

浮点数值的最高精度是 17 位小数，但在进行算术计算时其精确度远远不如整数。

```js
var result = 0.1 + 0.2;    // 结果不是 0.3，而是：0.30000000000000004
console.log(0.07 * 100);   // 结果不是 7，  而是：7.000000000000001
```

不能直接判断两个浮点数是否相等 ! 

#### 4.2 比较运算符

![4.png](https://i.loli.net/2021/08/02/BRNKEaCLf1QqXGr.png)

#### 4.3 逻辑运算符（短路运算）

**短路运算的原理：**当有多个表达式（值）时,左边的表达式值可以确定结果时,就不再继续运算右边的表达式的值。

##### 1. 逻辑与

- 语法： <font color="red">表达式1 && 表达式2</font>

- 如果第一个表达式的值为真，则返回表达式2

- 如果第一个表达式的值为假，则返回表达式1

##### 2. 逻辑或

- 语法： <font color="red">表达式1 || 表达式2</font>

- 如果第一个表达式的值为真，则返回表达式1

- 如果第一个表达式的值为假，则返回表达式2

#### 4.4 运算符优先级

![5.png](https://i.loli.net/2021/08/02/C4nlscPOirNQXJZ.png)

### 5. 数组

#### 5.1 创建数组

- 利用 new 创建数组

  ```js
  var arr = new Array();   
  ```

- 利用数组字面量创建数组

  ```js
  var  arr = ['小白','小黑','大黄','瑞奇'];

#### 5.2 数组新增元素

- 通过修改 length 长度新增数组元素，length 是可读写的

  ```js
  var arr = ['red', 'green', 'blue', 'pink'];
  arr.length = 7;
  ```

- 通过修改数组索引新增数组元素

  ```js
  var arr = ['red', 'green', 'blue', 'pink'];
  arr[4] = 'hotpink';
  ```

### 6. 函数

#### 6.1 函数参数

![6.png](https://i.loli.net/2021/08/02/rhd9Uqo72QSvbkG.png)

#### 6.2 函数返回值

- 如果函数没有 return ，返回的值是 undefined

#### 6.3 arguments

当我们不确定有多少个参数传递的时候，可以用 **arguments** 来获取。在 JavaScript 中，arguments 实际上它是当前函数的一个**内置对象**。所有函数都内置了一个 arguments 对象，**arguments 对象中存储了传递的所有实参**。

**arguments展示形式是一个伪数组**，因此可以进行遍历。伪数组具有以下特点：

- 具有 length 属性
- 按索引方式储存数据
- 不具有数组的 push , pop 等方法

```js
function maxValue() {
      var max = arguments[0];
      for (var i = 0; i < arguments.length; i++) {
         if (max < arguments[i]) {
                    max = arguments[i];
         }
      }
      return max;
}
 console.log(maxValue(2, 4, 5, 9));
 console.log(maxValue(12, 4, 9));
```

#### 6.4 函数声明方式

- 命名函数

  ```js
  // 声明定义方式
  function fn() {...}
  // 调用  
  fn();  
  ```

- 匿名函数

  ```js
  // 这是函数表达式写法，匿名函数后面跟分号结束
  var fn = function(){...}；
  // 调用的方式，函数调用必须写到函数体下面
  fn();
  ```

#### 6.5 立即执行函数

不需要调用，立马能够自己执行的函数

```js
1. (function(){})()
2. (function(){}());
```

主要作用：创建一个独立的作用域，避免了命名冲突问题

### 7. 作用域

- js 没有块作用域
- 用 var 在函数内创建局部变量

```js
fn() {
    a = b = c = 10;		// a, b, c为全局变量
}
```

### 8. 预解析

**|预解析|**：在当前作用域下, JS 代码执行之前，浏览器会默认把带有 var 和 function 声明的变量在内存中进行提前声明或者定义。

```js
console.log(num);  		// undifined
var num = 10;   
```

```js
fn();					// 10
function fn() {
    console.log(10);
}
```

```js
fn();					// undifined
var  fn = function() {
    console.log(10);
}
```

等价于

```js
var fn;
fn();
fn = function() {
	console.log(10);
}
```

### 9. 对象

#### 9.1 利用字面量创建对象

```js
var star = {
    name : 'Jaron',
    age : 18,
    sex : '男',
    sayHi : function(){
        alert('嗷呜~');
    }
};
```

#### 9.2 利用new Object创建对象

```js
var andy = new Obect();
andy.name = 'Jaron';
andy.age = 18;
andy.sex = '男';
andy.sayHi = function(){
    alert('嗷呜~');
}
```

#### 9.3 利用构造函数创建对象

```js
function Person(name, age, sex) {
     this.name = name;
     this.age = age;
     this.sex = sex;
     this.sayHi = function() {
      alert('我的名字叫：' + this.name + '，年龄：' + this.age + '，性别：' + this.sex);
    }
}
var bigbai = new Person('大白', 100, '男');
var smallbai = new Person('小白', 21, '男');
console.log(bigbai.name);
console.log(smallbai.name);
```

1.   构造函数约定首字母大写。
2.   函数内的属性和方法前面需要添加 this ，表示当前对象的属性和方法。
3.   构造函数中不需要 return 返回结果。
4.   当我们创建对象的时候，必须用 new 来调用构造函数。

#### 9.4 遍历对象属性

```js
for (var k in obj) {
    console.log(k);      // 这里的 k 是属性名
    console.log(obj[k]); // 这里的 obj[k] 是属性值
}
```

### 10. 内置对象

#### 10.1 Math 对象

```js
Math.PI		 		   // 圆周率
Math.floor() 	 	   // 向下取整
Math.ceil()            // 向上取整
Math.round()           // 四舍五入版 就近取整   注意 -3.5   结果是  -3 
Math.abs()			   // 绝对值
Math.max()/Math.min()  // 求最大和最小值 
```

#### 10.2 Date 对象

```js
var now = new Date();
console.log(now);
```

- 如果Date()不写参数，就返回当前时间
- 如果Date()里面写参数，就返回括号里面输入的时间 

![7.png](https://i.loli.net/2021/08/02/JryvZ4H63QsOu7b.png)

#### 10.3 数组对象

```js
var arr = new Array();
```

##### 10.3.1 数组检测

```js
console.log(arr instanceof Array); // true
console.log(Array.isArray(arr));   // true
```

##### 10.3.2 数组方法

![8.png](https://i.loli.net/2021/08/02/oYngFlBaxfe6D9O.png)

**sort**

```js
var arr = [1, 64, 9, 6];
arr.sort(function(a, b) {
    return b - a;      // 降序
    // return a - b;   // 升序
});
console.log(arr);
```

![9.png](https://i.loli.net/2021/08/02/J3hjPanmOCplWVo.png)
![10.png](https://i.loli.net/2021/08/02/iapmJ4zGtb7NKBg.png)

#### 10.4 字符串对象

##### 10.4.1 基本包装类型

**|基本包装类型|**：就是把简单数据类型包装成为复杂数据类型，这样基本数据类型就有了属性和方法。

```js
var str = 'andy';
console.log(str.length);

// 1. 生成临时变量，把简单类型包装为复杂数据类型
var temp = new String('andy');
// 2. 赋值给我们声明的字符变量
str = temp;
// 3. 销毁临时变量
temp = null;
```

##### 10.4.2 字符串的不可变

```js
var str = 'abc';
str = 'hello';
// 当重新给 str 赋值的时候，常量'abc'不会被修改，依然在内存中
// 重新给字符串赋值，会重新在内存中开辟空间，这个特点就是字符串的不可变
// 由于字符串的不可变，在大量拼接字符串的时候会有效率问题
var str = '';
for (var i = 0; i < 100000; i++) {
    str += i;
}
console.log(str); // 这个结果需要花费大量时间来显示，因为需要不断的开辟新的空间
```

##### 10.4.3 字符串方法

![11.png](https://i.loli.net/2021/08/02/uietF4qzPwZ7jO5.png)

replace() 方法用于在字符串中用一些字符替换另一些字符。

```js
replace(被替换的字符串， 要替换为的字符串)；		// 只替换第一个字符
```

split()方法用于切分字符串，它可以将字符串切分为数组。在切分完毕之后，返回的是一个新数组。

```js
var str = 'a,b,c,d';
console.log(str.split(','));   // 返回的是一个数组 [a, b, c, d]
```

