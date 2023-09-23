---
title: 第六章 数据定义
date: 2021-5-5 24:00:00
tags: 
categories: 数据库
---
## 第六章 数据定义

### 6.1 ISO数据类型

<!-- more -->

![1.png](https://i.loli.net/2021/05/05/l8u6EwZv4BzpS9d.png)

- CHAR: CHAR [VARYING] [length]

- 定点数：DEC(5, 3)

### 6.2 完整性增强特性

- 必须有值的数据
- 域约束
- 实体完整性
- 引用完整性
- 企业级约束

#### 6.2.1 必须有值的数据

某些列的值必须为有效值，不允许为空

#### 6.2.2 域约束

##### CHECK

CHECK子句只能引用已定义列。

CHECK(searchCondition)

例：

​	sex CHAR NOT NULL CHECK(sex IN('M', 'F'));

##### CREATE DOMAIN

可以创造值域

CREATE DOMAIN DomainName [AS] dataType

[DEFAULT defaultOption]

[CHECK(searchCondition)];

sex sexType NOT NULL;

##### DROP DOMAIN

撤销域约束

DROP DOMAIN DomainName[RESTRICT | CASCADE]

如果指定为RESTRICT，而该域正在被用于某个现存的表、视图或断言的定义，那么撤销失败。若指定为CASCADE，则任一表中基于该域的列都会自动地变为用该域的基类型定义。

#### 6.2.3 实体完整性

PRIMARY KEY(clientNo, propertyNo);

**UNIQUE**

每个表中只能使用一个PRIMARY KEY子句。但是，可用关键字UNIQUE保证列的唯一性。UNIQUE子句中出现的每个列必须被声明为NOT NULL。

UNIQUE(clientNo, propertyNo);

#### 6.2.4 引用完整性

FOREIGN KEY(branchNo) REFERENCES Branch[(branchNo)]

**UPDATE / DELETE**

- CASCADE: 删除父表中的行并且自动删除子表中匹配的行。
- SET NULL: 删除父表中的元组且设置子表中的外键为NULL。
- SET DEFAULT: 删除父表中的元组且设子表中的外键为默认值。
- NO ACTION: 拒绝对父表进行删除操作。ON DELETE规则的默认设置为ON ACTION。

FOREIGN KEY(branchNo) REFERENCES Branch[(branchNo)] ON UPDATE CASCADE；

#### 6.2.5 企业级约束

**|断言|**：跨表约束

CREATE ASSERTION AssertionName

CHECK(searchCondition)

### 6.3 数据定义

- CREATE SCHEMA / DOMAIN / TABLE / VIEW / INDEX
- DROP SCHEMA / DOMAIN / TABLE / VIEW / INDEX

- ALTER DOMAIN / TABLE

#### 6.3.1 创建数据库

CREATE SCHEMA[Name | AUTHORIZATION Creatorldentifier]

DROP SCHEMA Name[RESTRICT | CASCADE]

如果指定了RESTRICT，则模式必须为空才能删除。

#### 6.3.2 创建表

CREATE TABLE TableName
({colName dataType [NOT NULL] [UNIQUE] [PRIMARY KEY]
[DEFAULT defaultOption]
[CHECK searchCondition] [,...]}
[PRIMARY KEY (listOfColumns),]
{[UNIQUE (listOfColumns),] […,]}
{[FOREIGN KEY (listOfFKColumns)
REFERENCES ParentTableName
[(listOfCKColumns)],
[ON UPDATE referentialAction]
[ON DELETE referentialAction ]] [,…]}
{[CHECK (searchCondition)] [,…] })

**表约束可以用CONSTRAINT ConstraintName作为前缀**

#### 6.3.3 修改表定义

- 添加/删除列
- 添加/删除表约束
- 设置/删除列默认值

ALTER TABLE TabName
[ADD [COLUMN] columnName dataType
\[NOT NULL]\[UNIQUE]
[DEFAULT defaultOption]\[CHECK (searchCondition)]]
[DROP [COLUMN] columnName [RESTRICT | CASCADE ]]
[ADD [CONSTRAINT [constraintName]] tableConstraintDefinition]
[DROP CONSTRAINT constraintName [RESTRICT | CASCADE ]]
[ALTER [COLUMN] SET DEFAULT defaultOption]
[ALTER [COLUMN] DROP DEFAULT]

#### 6.3.4 删除表

DROP TABLE TableName [RESTRICT | CASCADE];

#### 6.3.5 创建索引

**|索引|**：索引是一种结构，它提供了基于一个或多个列值快速访问表中元组的方法。

CREATE [UNIQUE] INDEX IndexName ON TableName (columnName \[ASC |DESC][,…])

索引只能基于基表建立，而不能基于视图。对于每个列，可以指定升序或降序排列。

#### 6.3.6 删除索引

DROP INDEX IndexName;

### 6.4 视图

**|视图|**：为了得到另一个关系而对际关系进行一次或多次关系操作所得到的动态结果。视图是虚关系，即在数据库中不存在，需要时根据特定用户的要求临时生成。

​		当DBMS遇到视图引用时，一种方法是查找视图定义，并且将请求转换为对于视图源表的等价请求，然后完成等价的请求操作。这个转换的过程称为**试图分解**。

​		另一种可供选择的方法称为**视图物化**，即把视图存储在数据库的临时表中，并在基表变化时更新临时表以及时维护试图。

#### 6.4.1 创建视图

CREATE VIEW ViewName [ (newColumnName [,...]) ]
AS subselect [WITH [CASCADED | LOCAL] CHECK OPTION]

- subselect称为定义查询。如果说明WITH CHECK POTION， SQL将确保那些不满足定义查询中WHERE子句的行不会被添加到视图的基表中。

例：

CREATE VIEW StaffPropCnt (branchNo, staffNo, cnt)
AS SELECT s.branchNo, s.staffNo, COUNT(*)
FROM Staff s, PropertyForRent p
WHERE s.staffNo = p.staffNo
GROUP BY s.branchNo, s.staffNo;

#### 6.4.2 删除视图

DROP VIEW ViewName [RESTRICT| CASCADE]

#### 6.4.3 视图分解

例：

SELECT staffNo, cnt
FROM StaffPropCnt
WHERE branchNo = ‘B003’
ORDER BY staffNo;

​			↓

SELECT s.staffNo, COUNT(*)
FROM staff s, PropertyForRent p
WHERE s.staffNo = p.staffNo AND branchNo = ‘B003’
GROUP BY s.branchNo, s.staffNo
ORDER BY s.staffNo;

#### 6.4.4 视图的局限性

如果视图中某个列是基于聚集函数的，那么在访问该视图的查询语句中，该列只能出现在SELECT和ORDER BY子句里。具体地说，在基于该视图的查询语句中，该列不能出现在WHERE子句中，也不能作为任何聚集函数的参数。

#### 6.4.5 视图的可更新性

**|可更新视图|**：为了使视图可更新，对于任何一个行或列，DBMS必须都能追溯到其源表中相应的行或列。

#### 6.4.6 WITH CHECK OPTION

**|行迁移|**：视图中的行均满足定义查询中的WHERE条件。如果某行被修改后不再满足这种条件，那么它应当从视图中取出。进入或离开视图的行称为迁移行。通常来说，CREATE VIEW语句中的WITH CHECK POTION子句用于禁止行迁移出视图。

例：

CREATE VIEW LowSalary
AS SELECT * FROM Staff WHERE salary > 9000;

CREATE VIEW HighSalary
AS SELECT * FROM LowSalary
WHERE salary > 10000
WITH LOCAL CHECK OPTION;

CREATE VIEW Manager3Staff
AS SELECT * FROM HighSalary
WHERE branchNo = ‘B003’;

失败

UPDATE Manager3Staff SET salary = 9500
WHERE staffNo = ‘SG37’;

成功

UPDATE Manager3Staff SET salary = 8000
WHERE staffNo = ‘SG37’;

因为该行也会从LowSalary中去除。

#### 6.4.7 视图的优缺点

优点：

- 数据独立性
- 实时性
- 提高了安全性
- 降低了复杂性
- 方便
- 用户化
- 数据完整性

缺点：

- 更新局限性
- 结构局限性
- 性能开销

#### 6.4.8 视图物化

**|视图物化|**：把第一次访问视图的结果存储为数据库的临时表。

### 6.5 事务

#### 6.5.1 结束事务四种方式

- COMMIT语句成功提交事务
- ROLLBACK语句撤销事务
- 程序成功终止
- 不正常的程序终止撤销事务

#### 6.5.2 格式

SET TRANSACTION
[READ ONLY | READ WRITE] |
[ISOLATION LEVEL READ UNCOMMITTED
|READ COMMITTED|REPEATABLE READ
|SERIALIZABLE ]

隔离级表明事务执行过程中允许其他事务交互的程度。只有SERIALIZABLE隔离级是安全的。

### 6.6 自主访问控制

**授权标识符和所有权**

​		授权标识符是SQL用于辨别用户的一般标识符。每个数据库用户都由数据库管理员分配一个授权标识符。为了安全起见，授权标识符都和一个密码相关联。可用授权标识符来确定用户可访问哪些数据库对象，以及对哪些对象进行什么操作。

​		SQL中创建的每个对象都有一个所有者。所有者就是创建该对象所属的模式时AUTHORIZATION子句定义的授权标识符。最初只有所有者知道对象存在，并能对其进行任何操作。

**权限**

允许用户对指定基表或视图进行的操作。

- SELECT

- INSERT

- UPDATE

- DELETE

- REFERENCES

- USAGE

​        INSERT和UPDATE权限可以限制表中指定的列，允许改变这些列但不允许改变其他列。相似地，REFERENCES权限可以限制表中指定的列。

​		当用户用CREATE VIEW创建视图时，他自动成为视图的所有者但并不一定具有视图的所有权限。创建视图时，用户必须具有对组成视图的所有表的SELECT权限，以及视图中列的REFERENCES权限等。

#### 6.6.1 授予其他用户权限(GRANT)

GRANT {PrivilegeList | ALL PRIVILEGES}
ON ObjectName
TO {AuthorizationIdList | PUBLIC}
[WITH GRANT OPTION]

PrivilegeList由下列用逗号分开的一个或多个权限组成：
SELECT

DELETE

INSERT		[{cloName{...}}]

UPDATE		[{cloName{...}}]

REFERENCES	[{cloName{...}}]

USAGE

PUBLIC允许所有现在或未来授权用户访问数据，而不局限于DBMS系统当前已有的用户。

WITH GRANT OPTION子句允许AuthorizationIdList中的用户讲他们拥有的对指定对象的权限传递给其他用户。

#### 6.6.2 撤销用户权限(REVOKE)

REVOKE takes away privileges granted with GRANT.
REVOKE [GRANT OPTION FOR]
{PrivilegeList | ALL PRIVILEGES}
ON ObjectName
FROM {AuthorizationIdList | PUBLIC}
[RESTRICT | CASCADE]

​		可选的子句GRANT OPTION FOR允许通过GRANT语句中的WITH GRANT OPTION传递的那些权限被独立地撤销。

​		用户从其他用户那里获得的权限不受这个REVOKE语句的影响。

例：

A→B→C→D			E→D

A撤销给B的权限后C、D不受影响

![2.png](https://i.loli.net/2021/05/05/heclxiAfvDZE1OK.png)

### 作业

1. Discuss the functionality and importance of the Integrity Enhancement Feature (IEF).

   完整性约束共有五种类型：必须有值的数据、域约束、实体完整性、引用完整性和企业级约束。必须有值的数据让某些列的值必须为有效值，不允许为空。域约束限制每列合法值的集合。实体完整性让表中每一行的主关键字必须是唯一的非空值。引用完整性让外键必须是父表中已存在的有效的元组。企业级约束用来约束表的更新。

2. Discuss the advantages and disadvantages of views.

   优点：

   - 数据独立性
   - 实时性
   - 提高了安全性
   - 降低了复杂性
   - 方便
   - 用户化
   - 数据完整性

   缺点：

   - 更新局限性
   - 结构局限性
   - 性能开销

3. What restrictions are necessary to ensure that a view is updatable?

   - 没有指定DISTINCT，即重复元组未从查询结果中消除。
   - 定义查询的SELECT列表中的每个元素均为列名，且列名出现的次数不多于一次。
   - FROM子句只指定一个表，即视图只有一个源表且用户对该表有要求的权限。
   - WHERE子句不能包括任何引用了FROM子句中的表的嵌套SELECT操作。
   - 定义查询中不能有GROUP BY或HAVING子句。

4. Discuss how the Access Control mechanism of SQL works.

   SQL通过GRANT和REVOKE语句来支持自主访问控制。该机制基于授权标识符、所有权和权限的概念。

   授权标识符：授权标识符是SQL用于辨别用户的一般标识符。每个数据库用户都由数据库管理员分配一个授权标识符。为了安全起见，授权标识符都和一个密码相关联。可用授权标识符来确定用户可访问哪些数据库对象，以及对哪些对象进行什么操作。

   所有权：SQL中创建的每个对象都有一个所有者。所有者就是创建该对象所属的模式时AUTHORIZATION子句定义的授权标识符。最初只有所有者知道对象存在，并能对其进行任何操作。

   权限：允许用户对指定基表或视图进行的操作。有SELECT、INSERT、UPDATE、DELETE、REFERENCES和USAGE。其中，INSERT和UPDATE权限可以限制表中指定的列，允许改变这些列但不允许改变其他列。相似地，REFERENCES权限可以限制表中指定的列。

Answer the following questions using the relational schema from the Exercises at the end of Chapter 3.

![4.png](https://i.loli.net/2021/05/05/f1wbWg5rZ3l9RFX.png)

5. Create the Hotel table using the integrity enhancement features of SQL.

    CREATE DOMAIN HotelNoType AS CHAR(4);

     CREATE TABLE Hotel(

    hotelNo HotelNoType NOT NULL PRIMARY KEY,

    hotelName VARCHAR(15) NOT NULL,

    city VARCHAR(30) NOT NULL

    );

6. Now create the Room, Booking, and Guest tables using the integrity enhancement features of SQL with the following constraints:
    (a) Type must be one of Single, Double, or Family.
    (b) Price must be between £10 and £100.
    (c) roomNo must be between 1 and 100.
    (d) dateFrom and dateTo must be greater than today’s date.
    (e) The same room cannot be double booked.
    (f) The same guest cannot have overlapping bookings.

    CREATE TABLE Guest(

    guestNo INTEGER(10) NOT NULL PRIMARY KEY,

    guestName VARCHAR(20) NOT NULL,

    guestAddress VARCHAR(30) NOT NULL

    )

    CREATE TABLE Room(

    roomNo INTEGER(4) NOT NULL CHECK(roomNo BETWEEN 1 AND 100),

    hotelNo INTEGER(5) NOT NULL,

    type VARCHAR(10) CHECK(type IN('Single', 'Double', 'Family')),

    price DECIMAL(8, 2) CHECK(price BETWEEN 10 AND 100),

    PRIMARYR KEY(roomNo, hotelNo),

    FOREIGN KEY(hotelNo) REFERENCES Hotel(hotelNo)

    ON DELETE CASCADE ON UPDATE CASCADE

    );

    CREATE TABLE Booking(

    hotelNo INTEGER(5) NOT NULL, 

    guestNo INTEGER(10) NOT NULL,

    dateFrom DATE NOT NULL CHECK(VALUE > currentDate), 

    dateTo DATE NOT NULL CHECK(VALUE > currentDate), 

    roomNo INTEGER(4) NOT NULL,

    PRIMARY KEY(hotelNo, guestNo, dateFrom),

    FOREIGN KEY(hotelNo) REFERENCES Hotel(hotelNo) ON DELETE CASCADE ON UPDATE CASCADE,

    FOREIGN KEY(roomNo, hotelNo) REFERENCES Room(roomNo, hotelNo) ON DELETE CASCADE ON UPDATE CASCADE,

    FOREIGN KEY(guestNo) REFERENCES Guest(guestNo) ON DELETE CASCADE ON UPDATE CASCADE,

    CHECK(NOT EXISTS(SELECT * FROM Booking b WHERE b.dateTo > Booking.dateFrom AND b.dateFrom < Booking.dateTo AND

    b.roomNo = Booking.roomNo AND b.hotelNo = Booking.hotelNo)),

    CHECK(NOT EXISTS(SELECT * FROM Booking b WHERE b.dateTo > Booking.dateFrom AND b.dateFrom < Booking.dateTo AND b.guestNo = Booking.guestNo))

    ) ;

7. Consider the following view defined on the Hotel schema:
    CREATE VIEW HotelBookingCount (hotelNo, bookingCount)
    AS SELECT h.hotelNo, COUNT(*)
    FROM Hotel h, Room r, Booking b
    WHERE h.hotelNo = r.hotelNo AND r.roomNo = b.roomNo
    GROUP BY h.hotelNo;
    For each of the following queries, state whether the query is valid and for the valid ones should how each of the queries would be mapped onto a query on the underling base tables.
    (a) SELECT * FROM HotelBookingCount;

  ​	有效，视图分解为

  ​	SELECT h.hotelNo, COUNT(*)

​		FROM Hotel h, Room r, Booking b

​		WHERE h.hotelNo = r.hotelNo AND r.roomNo = b.roomNo

​		GROUP BY h.hotelNo;

  (b) SELECT hotelNo FROM HotelBookingCount WHERE hotelNo = ‘H001’;

  ​	有效，视图分解为

  ​	SELECT h.hotelNo

​		FROM Hotel h, Room r, Booking b

​		WHERE h.hotelNo = r.hotelNo AND r.roomNo = b.roomNo

​		AND hotelNo = ‘H001’

​		GROUP BY h.hotelNo;

  (c) SELECT MIN(bookingCount) FROM HotelBookingCount;

  ​	无效，bookingCount是基于聚集函数的，所以不能作为聚集函数的参数

  (d) SELECT COUNT(*) FROM HotelBookingCount;

  ​	无效，视图中的bookingCount是基于聚集函数的，不能作为聚集函数的参数

  (e) SELECT hotelNo FROM HotelBookingCount WHERE bookingCount > 1000;

  ​	无效，bookingCount是基于聚集函数的，不能出现在WHERE子句中

  (f) SELECT hotelNo FROM HotelBookingCount ORDER BY bookingCount;

  ​	有效，视图分解为

​		SELECT h.hotelNo,

​		FROM Hotel h, Room r, Booking b

​		WHERE h.hotelNo = r.hotelNo AND r.roomNo = 	b.roomNo

​		GROUP BY h.hotelNo

​		ORDER BY COUNT(*);

