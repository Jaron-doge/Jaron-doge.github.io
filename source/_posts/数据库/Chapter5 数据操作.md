---
title: 第五章 数据操作
date: 2021-4-22 24:00:00
tags: 
categories: 数据库
---
## 第五章 数据操作

### 5.1 简介

<!-- more -->

SQL是一种**面向转换语言**，它由两个主要部分组成：

**DDL**：用于定义数据库结构和数据库的访问控制。

**DML**：用于检索和更新数据。

### 5.2 书写SQL命令

**扩展的巴克斯范式**

- 大写字母用于表示保留字。
- 小写字母用于表示用户自定义字。
- 竖线(|)表示从选项中进行**选择**，例如a|b|c。

- 大括号表示**所需元素**，例如{ a }。
- 中括号表示**可选择元素**，例如[a]。
- 省略号(…)表示某一项**可选择重复**零到多次。

### 5.3 数据操作

#### 5.3.1 简单查询

**SELECT** 		   [**DISTINCT** | **ALL**]
					     {* | [columnExpression [**AS** newName]] [,...] }
**FROM** 		     TableName [alias] [, ...]
[**WHERE** 		  condition]
[**GROUP BY** 	columnList] [**HAVING** condition]
[**ORDER BY** 	 columnList].

**处理过程顺序**

**FROM**				给出将用到的表

**WHERE**			  过滤满足条件的行

**GROUP BY**        将具有相同属性值的行分成组

**HAVING**			 过滤满足条件的组

**SELECT**  			 指定查询结果中出现的列

**ORDER BY**		 指定查询结果的顺序

---

**DISTINCT**

去掉重复的元组

**计算字段**

SELECT staffNo, fName, lName, salary/12 AS monthlySalary

FROM	Staff;

**WHERE**

- 比较运算、复合比较运算(NOT/ADD/OR)

- 范围(BETWEEN/NOT BETWEEN)

- 成员关系(IN/NOT IN)

  <img src="https://i.loli.net/2021/05/05/lWcMUs5FXrZkLhS.png" alt="1.png" style="zoom:80%;" />

- 模式匹配(LIKE/NOT LIKE)

  - **%**：百分号表示零或多个字符序列。
  - **_**：下划线表示任意单个字符。

  ---

  - address LIKE '**H%**' 意味着字符串第一个字符必须是H
  - address LIKE '**H_ _ _**'意味着字符串正好有四个字符，第一个字符为H
  - address LIKE '**%e**'意味着一个字符序列，长度最小为1，最后一个字符为e
  - address LIKE '**%H%**'意味着一个包含字符串H的任意长度序列
  - address NOT LIKE '**H%**'意味着字符串第一个字符不能为H

- 空查找(IS NULL/IS NOT NULL)

  SELECT 	clientNo

  FROM 	   Viewing

  WHERE	  comment IS NULL

#### 5.3.2 查询结果排序(ORDER BY)

**单列排序**

ORDER BY 子句包括所需排序的**列标识符**的列表。**列标识符**可能是列名字或列序号，如“1”标识列表中第一个元素。

SELECT 	  staffNo, fName, IName, salary

FROM	     Staff

ORDER BY  salary|4 DESC;

**多列排序**

ORDER BY 子句可能包含多个元素，**主排序键**决定查询结果总体的排序。**次排序键**控制主排序键相同行的顺序。

SELECT 		propertyNo, type, rooms, rent

FROM		    PropertyForRent

ORDER BY	 type, rent DESC;

查询结果首先以房产类型升序排列（默认值为ASC），对于相同的房产类型，以rent降序排列。

#### 5.3.3 SQL聚集函数

COUNT、SUM、AVG、MIN、MAX

​		SUM和AVG只能用于数值字段，除了COUNT(*)外，每一个函数首先要去掉空值，然后计算其非空值。

​		聚集函数只能用于SELECT列表和HAVING子句中，用在其他地方都是不正确的。如果SELECT列表包括聚集函数，却没有使用GROUP BY子句分组，那么SELECT列表的任何项都不能引用列，除了作为聚集函数的参数。

​		例：SELECT staffNo, COUNT(salary)

​				FROM Staff

​		是非法的

**COUNT、SUM**

SELECT 				COUNT(DISTINCT propertyNo) AS myCount, 							  							  SUM(salary) AS mySalary

FROM 				  Staff

WHERE				 position = 'Manager';

**MIN、MAX、AVG**

SELECT				  MIN(salary), MAX(salary), AVG(salary)

FROM					Staff;

#### 5.3.4 查询结果分组(GROUP BY)

  GROUP BY 按SELECT列表中的列进行分组，每一组产生一个综合查询结果。当使用GROUP BY时，SELECT列表中的项必须每组都有单一值。SELECT子句中的所有列除非用在聚集函数上，否则必须在GROUP BY子句中出现。当WHERE子句和GROUP BY子句同时使用时，必须首先使用WHERE子句。应用GROUP BY子句时，两个空值被认为是相等的。

<img src="https://i.loli.net/2021/05/05/DBQCkuFxegm6n94.png" alt="2.png" style="zoom:80%;" />

##### 分组约束（HAVING）

限定哪些**分组**将出现在最终查询结果中。

<img src="https://i.loli.net/2021/05/05/1YTeauwd24ZVhFX.png" alt="3.png" style="zoom:80%;" />

**HAVING和WHERE的差别**

​		WHERE子句将单个行过滤到查询结果中，而HAVING子句则将分组过滤到查询结果表中。HAVING子句使用的列名必须出现在GROUP BY子句列表中，或包含在聚集函数中。

#### 5.3.5 子查询

- 标量子查询
- 行子查询
- 表子查询

**用于相等判断的子查询**

列出在位于“163 Main St”的分公司中工作的员工的情况。

SELECT 	staffNo, fName, lName, position
FROM 	  Staff
WHERE     branchNo = (SELECT    branchNo
										 FROM       Branch
										 WHERE     street = ‘163 Main St’);

**用于聚集函数的子查询**

列出个人工资高于平均工资的所有员工，并求出多于平均数的值。

SELECT 		staffNo, fName, lName, position,
					  salary – (SELECT AVG(salary) FROM Staff) As SalDiff
FROM 		  Staff
WHERE         salary >(SELECT AVG(salary)
					  FROM Staff);

**子查询规则**

- ORDER BY子句不能用于子查询。
- 子查询SELECT列表必须由单个列名或表达式组成，除非子查询使用了关键词**EXISTS**。

- 默认情况下，子查询中列名取自子查询的FROM子句中给出的表。

- 当子查询是比较表达式中的一个操作数时，子查询必须出现在表达式的右边。

  - 例：WHERE (SELECT AVG(salary) FROM Staff) < salary

    是不正确的

**嵌套子查询：IN**

当子查询结果为行时，使用**IN**关键字

<img src="https://i.loli.net/2021/05/05/yFVXqPkOih9BjsN.png" alt="4.png" style="zoom:80%;" />

#### 5.3.6 ANY和ALL

用于产生单个列的子查询

**ANY/SOME**

列出工资高于分公司B003中至少一位员工的工资的所有员工

SELECT 			staffNo, fName, lName, position, salary
FROM 			  Staff
WHERE 		    salary > SOME (SELECT salary
													  FROM Staff
                                                      WHERE branchNo = ‘B003’);

<img src="https://i.loli.net/2021/05/05/GQ9SuXpVmKA5nYs.png" alt="5.png" style="zoom:80%;" />![6.png](https://i.loli.net/2021/05/05/1edubHYRnNExQq8.png)

**ALL**

列出工资高于分公司B003中任何员工的工资的所有员工

<img src="https://i.loli.net/2021/05/05/1edubHYRnNExQq8.png" alt="6.png" style="zoom:80%;" />

#### 5.3.7 多表查询

如果最终结果表包括了多个表中的列，则必须用连接操作。

##### 5.3.7.1 简单连接

- FROM	Client c	JOIN    Viewing v	ON	c.clientNo = v.clientNo

- FROM    Client c    JOIN    Viewing    Using    clientNo
- FROM    Client c NATURAL JOIN    Viewing

后两种是自然连接

##### 5.3.7.2 三表连接

FROM    (Branch b     JOIN    Staff s    USING    branchNo)    AS  bs

​				JOIN    PropertyForRent p    USING    staffNo

##### 5.3.7.3 外连接

SELECT b.\*, p.\*

FROM	Branch b    LEFT JOIN    PropertyForRent p    ON    b.city = p.city

##### 5.3.7.4 全外连接

FULL JOIN

#### 5.3.8 EXISTS 和 NOT EXISTS

仅用于子查询中，返回结果为真/假。

仅检查子查询结果表中是否存在行。

SELECT staffNo FROM Staff s

WHERE EXISTS(SELECT * FROM Branch b

​							WHERE s.branchNo = b.branchNo);

#### 5.3.9 合并结果表

**UNION**、**INTERSECT**、**EXCEPT**

operator [ALL] [CORRESPONDING [BY {col}]]

**例：**

- (SELECT city FROM Branch WHERE city IS NOT NULL)

  ​	        UNION

  ​				(SELECT city FROM PropertyForRent)

- (SELECT * FROM Branch WHERE city IS NOT NULL)

  ​			UNION CORRESPONDING BY city

  ​					(SELECT * FROM PropertyForRent)

#### 5.3.10 数据库更新

**INSERT**、**UPDATE**、**DELETE**

##### 5.3.10.1 INSERT

**默认插入**

INSERT INTO TableName[(col)]

VALUES(dataValue)

**批量插入**

INSERT INTO TableName[{col}]

​	SELECT ...

##### 5.3.10.2 UPDATE

UPDATE TableName

SET [col = value], [col2 = value2]

##### 5.3.10.3 DELETE

**删除指定行**

DELETE FROM TableName WHERE

**删除所有行**

DELETE FROM Viewing

### 作业

1. What are the advantages and disadvantages of SQL?

   优点：

   1. 安全性，只允许授权的用户进行访问

   2. 唯一一个得到普遍认可的数据库标准语言
   3. 可移植性高

   缺点：

   1. sql查询语句需要知道有关数据库结构的详细信息
   2. 处理聚合函数中的空值

2. What restrictions apply to the use of the aggregate functions within the SELECT statement? How do nulls affect the aggregate functions?

   ​		聚集函数只能用于SELECT列表和HAVING子句中，用在其他地方都是不正确的。如果SELECT列表包括聚集函数，却没有使用GROUP BY子句分组，那么SELECT列表的任何项都不能引用列，除了作为聚集函数的参数。除了COUNT(*)外，每一个函数首先要去掉空值，然后计算其非空值。

3. Explain how the GROUP BY clause works. What is the difference between the WHERE and HAVING clauses?

   ​		GROUP BY 按SELECT列表中的列进行分组，每一组产生一个综合查询结果。当使用GROUP BY时，SELECT列表中的项必须每组都有单一值。SELECT子句中的所有列除非用在聚集函数上，否则必须在GROUP BY子句中出现。当WHERE子句和GROUP BY子句同时使用时，必须首先使用WHERE子句。应用GROUP BY子句时，两个空值被认为是相等的。

   ​		WHERE子句将单个行过滤到查询结果中，而HAVING子句则将分组过滤到查询结果表中。HAVING子句使用的列名必须出现在GROUP BY子句列表中，或包含在聚集函数中。


![4.png](https://i.loli.net/2021/05/05/f1wbWg5rZ3l9RFX.png)

For the following exercises, use the Hotel schema defined at the start of the Exercises at the end of Chapter 3.

**Simple Queries**

4. List all double or family rooms with a price below £40.00 per night, in ascending order of price.

   SELECT * FROM Room

   WHERE price < 40000 AND type in ('double', 'family')

   ORDER BY price;

5. List the bookings for which no dateTo has been specified.

   SELECT * FROM Booking

   WHERE dateTo IS NULL;

**Aggregate Functions**

6. How many different guests have made bookings for August?

   SELECT COUNT(DISTINCT guestNo) FROM Booking

   WHERE (dateFrom >= '2021-08-01' and dateTo <= '2021-08-31');

**Subqueries and Joins**

7. List the details of all rooms at the Grosvenor Hotel, including the name of the guest staying in the room, if the room is occupied.

   SELECT * from Room r LEFT JOIN

   ​		(SELECT b.hotelNo, g.guestNo FROM Hotel h,Booking b, Guest g

   ​		 WHERE h.hotelNo = b.hotelNo and b.guestNo = g.guestNo

   ​		 AND h.hotelName = 'Grosvenor' 

   ​    	 AND dateFrom <= currentDate AND dateTo >= currentDate

   ​		 ) AS t

   ON r.hotelNo = t.hotelNo AND r.roomNo = t.roomNo

8. What is the total income from bookings for the Grosvenor Hotel today?

   SELECT SUM(price) From Room r, Booking b

   WHERE r.roomNo = b.roomNo 

   AND dateFrom <= currentDate AND dateTo  >= currentDate 

   AND r.hotelNo IN (SELECT h.hotelNo FROM Hotel h

   ​								  WHERE h.hotelName = 'Grosvenor')

9. What is the lost income from unoccupied rooms at the Grosvenor Hotel?

   SELECT SUM(price) From Room r

   WHERE r.roomNo NOT IN(SELECT b.roomNo 

   ​											   FROM Hotel h, Booking b

   ​                                               WHERE h.hotelName = 'Grosvenor'

   ​											   AND dateFrom <= currentDate 

   ​                                               AND dateTo  >= currentDate 

   ​                                               AND h.hotelNo = b.hotelNo)

**Grouping**

10. What is the average number of bookings for each hotel in August?

    SELECT hotelNo, COUNT(roomNo)/31 AS CNT

    FROM Booking b

    WHERE dateFrom >= '2021-08-01' AND dateTo  <='2021-08-31'	GROUP BY hotelNo

11. What is the most commonly booked room type for each hotel in London?

    SELECT t.hotelNo, MAX(CNT) FROM

    ​	(SELECT hotelNo,type,COUNT(type) AS CNT 

    ​	FROM Hotel h, Room r, Booking b

    ​	WHERE h.city = 'London' 

    ​	AND h.hotelNo = r.hotelNo AND r.roomNo = b.roomNo

    ​	GROUP BY hotelNo, type) AS t

    GROUP BY t.hotelNo

**Creating and Populating Tables**

12. Insert records into each of these tables.

    INSERT INTO Hotel VALUES('H01', 'Grosvenor', 'London');

    INSERT INTO Room VALUES('101', 'H01', 'small', 100);

    INSERT INTO Booking VALUES('H01', 'G01', '2021-4-14', '2021-4-15', '101');

    INSERT INTO Guest VALUES('G01', 'ZHANG SAN', 'London');

13. Update the price of all rooms by 5%.

    UPDATE Room SET price = price * 1.05;

