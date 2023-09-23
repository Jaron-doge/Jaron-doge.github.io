---
title: 第四章 关系代数
date: 2021-4-15 24:00:00
tags: 
categories: 数据库
---
## 第四章 关系代数

### 4.1 一元运算

#### 4.1.1 选择

<!-- more -->

**|σ<sub>predicate</sub>(R)|**：选择运算作用于单个关系R，得到一个新关系，它由R中满足特定条件的元组组成。

例：σ<sub>salary > 10000 </sub>(Staff)

#### 4.1.2 投影运算

**|∏col<sub>1</sub>, ... , col<sub>n</sub>(R)|**：投影运算作用于单个关系R，得到由R的一个垂直子集构成的新关系，该垂直子集抽取R中指定属性上的值并**去掉了重复元组**。

### 4.2 集合运算

#### 4.2.1 并

**|R∪S|**：两个关系R和S的并，定义了一个包含R、S中所有不同元组的新关系。R和S必须具有**并相容性**。

例：∏<sub>city</sub>(Branch)∪Π<sub>city</sub>(PropertyForRent)

#### 4.2.2 差

**|R - S |**：集合差运算定义了一个新的关系，它由所有属于R但不属于S的元组构成。R和S必须具有并相容性。

列出有分公司但待租房产的城市清单：∏<sub>city</sub>(Branch) - Π<sub>city</sub>(PropertyForRent)

#### 4.2.3 交

**|R∩S|**：交运算定义了一个关系，它由既属于R又属于S的元组构成。R和S必须具备**并相容性**。

#### 4.2.4 笛卡尔乘积

**|R × S|**：笛卡尔乘积运算定义了一个关系，它是关系R中每个元组与关系S中每个元组并联的结果。

### 4.3 连接运算

将两个关系结合起来组成一个新的关系。相当于对两个参与运算关系的笛卡尔乘积执行一次选择运算。

#### 4.3.1 θ连接

**|R ⋈<sub>F</sub> S|**：θ连接运算定义一个关系，它包含R和S的笛卡尔乘积中所有满足谓词F的元组。谓词F的格式为R.a<sub>i</sub> θ S.b<sub>i</sub>, 其中θ为6个比较运算符之一。

​	R⋈<sub>F</sub>S = σ<sub>F</sub>(R × S)

#### 4.3.2 等接运算

在谓词F仅包含等号(=)的情况下，θ连接就变成了等接。

#### 4.3.3 自然连接

**|R ⋈ S|**：自然连接是关系R和S在所有公共属性x上的等接。但在得到的结果中每个公共属性只保留一次，其余删除。

<img src="https://i.loli.net/2021/05/05/li2zGxt69XHsk4N.png" alt="1.png" style="zoom:80%;" />

#### 4.3.4 外连接

**|R ⋊ S|**：(左)外连接是这样一种连接，它将R中的所有元组都保留在结果关系中，包括那些公共属性与S不匹配的，不过，结果关系中来自S的所有非公共属性均取空。

#### 4.3.5 半连接

**|R![2.png](https://i.loli.net/2021/05/05/MUHVFwZqfDliu26.png)S|**：半连接运算定义的关系包含**R中的这样一些元组**，它们参与了R与S满足谓词F的连接。

​		半连接运算执行了两个关系的连接后，再将结果投影到第一个参与运算的关系的所有属性上。

R![2.png](https://i.loli.net/2021/05/05/MUHVFwZqfDliu26.png)S = Π<sub>A</sub>(R ⋈<sub>F</sub> S)	A是R中所有属性的集合

### 4.4 除法运算

假设关系R定义在属性集合A上，关系S定义在属性集合B上，并且B ⊆ A。C = A - B，即C是属于R但不属于S的属性集合。

**|R ÷ S|**：除法运算定义了属性集合C上的一个关系，该关系的元组与S中每个元组的组合都能在R中找到匹配元组。

除法运算的步骤：

- T<sub>1</sub> ⬅ Π<sub>c</sub>(R)
- T<sub>2</sub> ⬅ Π<sub>c</sub>( (T<sub>1</sub> × S) - R )
- T ⬅ T<sub>1</sub> - T<sub>2</sub>

<img src="https://i.loli.net/2021/05/05/pzS2DgOT1vXARek.png" alt="3.png" style="zoom:80%;" />

### 作业

1. Define the five basic relational algebra operations. Define the Join, Intersection, and Division operations in terms of these five basic operations.

   **|σ<sub>predicate</sub>(R)|**：选择运算作用于单个关系R，得到一个新关系，它由R中满足特定条件的元组组成。

   **|∏col<sub>1</sub>, ... , col<sub>n</sub>(R)|**：投影运算作用于单个关系R，得到由R的一个垂直子集构成的新关系，该垂直子集抽取R中指定属性上的值并**去掉了重复元组**。

   **|R∪S|**：两个关系R和S的并，定义了一个包含R、S中所有不同元组的新关系。R和S必须具有**并相容性**。

   **|R × S|**：笛卡尔乘积运算定义了一个关系，它是关系R中每个元组与关系S中每个元组并联的结果。

   **|R - S |**：集合差运算定义了一个新的关系，它由所有属于R但不属于S的元组构成。R和S必须具有并相容性。

   **|R∩S|**：交运算定义了一个关系，它由既属于R又属于S的元组构成。R和S必须具备**并相容性**。

   连接关系：将两个关系结合起来组成一个新的关系。相当于对两个参与运算关系的笛卡尔乘积执行一次选择运算。

   假设关系R定义在属性集合A上，关系S定义在属性集合B上，并且B ⊆ A。C = A - B，即C是属于R但不属于S的属性集合。

   **|R ÷ S|**：除法运算定义了属性集合C上的一个关系，该关系的元组与S中每个元组的组合都能在R中找到匹配元组。

   除法运算的步骤：

   - T<sub>1</sub> ⬅ Π<sub>c</sub>(R)
   - T<sub>2</sub> ⬅ Π<sub>c</sub>( (T<sub>1</sub> × S) - R )
   - T ⬅ T<sub>1</sub> - T<sub>2</sub>

2. Discuss the difference between the five Join operations: Theta join, Equijoin, Natural join, Outer join, and Semijoin. Give examples to illustrate your answer.
    Use the Hotel schema defined at the Exercises at the end of Chapter 3, finish exercises 4 and 5.

  **|R ⋈<sub>F</sub> S|**：θ连接运算定义一个关系，它包含R和S的笛卡尔乘积中所有满足谓词F的元组。谓词F的格式为R.a<sub>i</sub> θ S.b<sub>i</sub>, 其中θ为6个比较运算符之一。

  等接运算是特殊的θ运算。在谓词F仅包含等号(=)的情况下，θ连接就变成了等接。

​	自然连接是特殊的等接运算。自然连接是关系R和S在所有公共属性x上的等接，得到结果后要去重。

  外连接将R中的所有元组都保留在结果关系中，包括那些公共属性与S不匹配的，不过，结果关系中来自S的所有非公共属性均取空。

  半连接运算执行了两个关系的连接后，再将结果投影到第一个参与运算的关系的所有属性上。

![4.png](https://i.loli.net/2021/05/05/f1wbWg5rZ3l9RFX.png)

3. Describe the relations that would be produced by the following relational algebra operations:

   a) 列出所有房间价格高于50的酒店号列表

   b) 列出所有酒店的所有房间

   c) 列出所有房间价格高于50的酒店名

   d) 列出所有客人的详细资料，并显示预定结束时间在2007年1月1日后的客人的预定资料

   e) 列出所有房间价格高于50的酒店的详细资料

   f) 列出预定了伦敦的酒店的所有客人的姓名

4. Generate the relational algebra for the following queries:

  (a) List all hotels

  Hotel∨

  (b) List all single rooms with a price below £20 per night.

  σ<sub>type = "single" ∧ price < 20 </sub>(Room)

  (c) List the names and cities of all guests.

  Π<sub>guestName, guestAddress</sub>(Guest)

  (d) List the price and type of all rooms at the Grosvenor Hotel.

  Π<sub>price, type</sub>(Room 半连接<sub>Room.hotelNo = Hotel.hotelNo</sub>(σ<sub>hotelName = ‘Grosvenor’</sub>(Hotel)))

  (e) List all guests currently staying at the Grosvenor Hotel.

  TempBooking(hotelNo, guestNo, dateFrom, dateTo, roomNo) ⬅ Booking半连接<sub>Booking.hoteltNo = Hotel.hotelNo</sub>(σ<sub>hotelName = ‘Grosvenor’</sub>(Hotel))

  Guest 半连接<sub>Guest.guestNo = TempBooking.guestNo</sub>(σ<sub>currentDate >= dateFrom ∧ currentDate <= dateTo</sub>(TempBooking)

  (f) List the details of all rooms at the Grosvenor Hotel, including the name of the guest staying in the room, if the room is occupied.

![4.png](https://i.loli.net/2021/05/05/f1wbWg5rZ3l9RFX.png)

  房间 外接 客人姓名

  TempBooking(hotelNo, guestNo, dateFrom, dateTo, roomNo) ⬅ Booking半连接<sub>Booking.hoteltNo = Hotel.hotelNo</sub>(σ<sub>hotelName = ‘Grosvenor’</sub>(Hotel))

  (Room 半连接<sub>Room.hotelNo = Hotel.hotelNo</sub>(σ<sub>hotelName = ‘Grosvenor’</sub>(Hotel)) )左外接

  (Π<sub>hotelNo, roomNo, guestName</sub>(Guest 连接<sub>Guest.guestNo = TempBooking.guestNo</sub> TempBooking))

  (g) List the guest details (guestNo, guestName, and guestAddress) of all guests staying at the Grosvenor Hotel.

  TempBooking(hotelNo, guestNo, dateFrom, dateTo, roomNo) ⬅ Booking半连接<sub>Booking.hoteltNo = Hotel.hotelNo</sub>(σ<sub>hotelName = ‘Grosvenor’</sub>(Hotel))

  Π<sub>guestNo, guestName, guestAddress</sub>(Guest 半连接<sub>Guest.guestNo = TempBooking.guestNo</sub> TempBooking)

