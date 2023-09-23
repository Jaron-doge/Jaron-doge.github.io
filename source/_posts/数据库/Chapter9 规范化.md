---
title: 第九章 规范化
date: 2021-6-2 00:00:00
tags: 
categories: 数据库
---
## 第九章 规范化

### 9.1 规范化的目的

<!-- more -->

**|规范化|**：生成一组既具有所期望的特性又能满足企业数据需求的关系的技术。

数据库拥有一组合适关系的好处：数据库易于用户访问，数据易于维护，在计算机上占有较小的存储空间。

规范化可以识别键和属性间的依赖关系。规范化是消除数据冗余的过程，可以消除数据冗余带来的异常。

### 9.2 数据冗余与更新异常(anomalies)

关系数据库设计的一个主要目标就是降低数据冗余与减少存储基本关系所需的文件存储空间。

Staff		(staffNo, sName, position, salary, branchNo)

Branch	(branchNo, bAddress)

StaffBranch	(staffNo, sName, position, salary, branchNo, bAddress)

在关系StaffBranch中存在冗余数据：同一个分公司的信息在每一个属于该分公司员工的信息里反复出现。相反，在关系Branch中，每个分公司的信息只出现了一次，而且在关系Staff中只有分公司的编号这一属性的值重复出现。

#### 9.2.1 插入异常

插入异常主要有两类

- 在StaffBranch中插入新员工信息时，必须包括分公司信息，并且必须确保B007分公司信息和关系StaffBranch已有的B007的元组的分公司信息保持一致。
- 在向StaffBranch中插入一个新的分公司信息时，若该公司没员工，有必要在录入staffNo时设置为null，但staffNo是主键，会违反实体完整性约束。

#### 9.2.2 删除异常

若删除某分公司最后一名员工后，公司信息也会丢失。

#### 9.2.3 修改异常

如果要修改StaffBranch中B003的某个属性值，那么必须更新所有B003的员工的元组。若修改操作未能在所有元组上执行，数据库则会产生不一致。

---

当把较大的关系分解成较小的关系时，有两个很重要的特性：

- **无损连接**(lossless-join)特性：确保原关系的任一实例能通过较小关系的对应实例确定出来。
- **依赖保持**(dependency preservation)特性：确保只需简单地在较小关系上支持某些约束，就可以继续支持在原关系上存在的约束。

### 9.3 函数依赖

**|函数依赖|**：描述一个关系中属性之间的联系。若A的每个值都和B中的一个唯一的值相对应，则称B函数依赖于A，记为A→B（A、B可能由一个或多个属性组成）。

**|决定方|**：位于函数依赖箭头左边的属性或属性组。

**|完全函数依赖|**：假设A和B是某一关系的属性（组），若B函数依赖于A，但不函数依赖于A的任一真子集，则称B完全函数依赖于A。

如果去掉A中的某些属性，依赖仍然成立，那么函数依赖A→B就是**部分函数依赖**。

**|传递依赖|**：若A→B，B→C，则称C通过B传递依赖于A。

- 由一组函数依赖X所隐含的函数依赖集合称为X的闭包（X<sup>+</sup>）

- 函数依赖具有自反性、增广性、传递性

#### 利用函数依赖确定主键

所有不属于主键的属性都应该函数依赖于主键。

### 9.5 第一范式(1NF)

**|非范式(UNF)|**：包含一个或多个**重复组**的表。

**|第一范式(1NF)|**：属于第一范式的关系，其每一行和每一列相交的位置有且仅有一个值。

为了将非规范化的表转化为第一范式，我们需要确定并删除表中的重复组。

两种方法：

1. 在含有重复数据的那些行的空白列上输入合适的数据。

2. 将重复数据单独移到一个新的关系中，同时也将原来关系中的关键属性复制到这个新的关系中。

### 9.6 第二范式(2NF)

**|第二范式|**：满足第一范式的要求并且每个非主键属性都完全函数依赖于主键的关系。

将1NF关系规范化为2NF关系需要消除部份依赖，如果存在部份依赖，就要将部份依赖的属性从原关系移出，移到一个新的关系中去，同时将这些属性的决定方复制到新的关系中。

![2.png](https://i.loli.net/2021/05/22/Iz48LfhrcXowmnb.png)

主键：clientNo, propertyNo

### 9.7 第三范式(3NF)

**|第三范式|**：满足第一范式和第二范式的要求并且所有非主键属性都不传递依赖于主键的关系。

将2NF关系规范化为3NF关系需要消除传递依赖，如果存在传递依赖，就要将传递依赖的属性移到一个新的关系中去，同时将这些属性的决定方也复制到新的关系中。

![3.png](https://i.loli.net/2021/05/22/2JjliXuqkxz8sNB.png)

### 9.8 2NF和3NF的一般化定义

**|第二范式|**：满足第一范式的要求并且每个非主属性都完全函数依赖于任何一个候选键的关系。

**|第三范式|**：满足第一范式和第二范式的要求并且所有非主属性都不传递依赖于任何一个候选键的关系。

**|非主属性|**：候选键之外的属性。

### 9.9 Byoce-Codd Normal Form范式(BCNF)

**|BCNF|**:当且仅当每个函数依赖的决定方都是候选键时，某一关系才是BCNF。

3NF和BCNF的区别在于，对于一个函数依赖A→B，3NF允许B是主键而A不是候选键

而BCNF坚持在依赖关系中A必须是候选键

例：

![4.png](https://i.loli.net/2021/05/22/31xdjKoJIrpiF5M.png)	

2NF：

![5.png](https://i.loli.net/2021/05/22/QiFuy3p5ELdR49g.png)

3NF：

![6.png](https://i.loli.net/2021/05/22/WD7pFE9kxs5QYGr.png)

BCNF：

![7.png](https://i.loli.net/2021/05/22/MsonR1yZ53ai4fp.png)

### 作业

1. Describe the types of update anomalies that may occur on a relation that has redundant data.

   StaffBranch	(staffNo, sName, position, salary, branchNo, bAddress)

   插入异常

   - 在StaffBranch中插入新员工信息时，必须包括分公司信息，并且必须确保B007分公司信息和关系StaffBranch已有的B007的元组的分公司信息保持一致。
   - 在向StaffBranch中插入一个新的分公司信息时，若该公司没员工，有必要在录入staffNo时设置为null，但staffNo是主键，会违反实体完整性约束。

   删除异常

   - 若删除某分公司最后一名员工后，公司信息也会丢失。

   修改异常

   - 如果要修改StaffBranch中B003的某个属性值，那么必须更新所有B003的员工的元组。若修改操作未能在所有元组上执行，数据库则会产生不一致。

2. Describe the concept of functional dependency.

   函数依赖：描述一个关系中属性之间的联系。若A的每个值都和B中的一个唯一的值相对应，则称B函数依赖于A，记为A→B（A、B可能由一个或多个属性组成）。

   

3. How is the concept of functional dependency associated with the process of normalization?

   函数依赖描述了属性之间的联系，而规范化是依靠键和属性之间的函数依赖对关系进行验证的形式化技术。

   

4. Describe the concept of full functional dependency and describe how this concept relates to 2NF. Provide an example to illustrate your answer.

   **完全函数依赖**：假设A和B是某一关系的属性（组），若B函数依赖于A，但不函数依赖于A的任一真子集，则称B完全函数依赖于A。规范化过程中的2NF要求满足1NF，并且每个非主键属性都完全函数依赖于主键的关系。

   2NF举例

   fd1 a, b→c, d

   fd2 a→e

   fd3 e→f

   2NF: A(a, b, c, d)

   ​		B(a, e, f)

5. Describe the concept of transitive dependency and describe how this concept relates to 3NF. Provide an example to illustrate your answer.

   **传递依赖**：若A→B，B→C，则称C通过B传递依赖于A。3NF要求满足2NF，并且所有非主键属性都不传递依赖于主键的关系。

   3NF举例

   fd1 a, b→c, d

   fd2 a→e

   fd3 e→f

   3NF: A(a, b, c, d)

   ​			B(a, e)

   ​			C(e, f)

6. Examine the Patient Medication Form for the Wellmeadows Hospital case study shown in Figure in next page.
    (a) Identify the functional dependencies represented by the data shown in the form in Figure 13.25.
    
      

​	  (b) Describe and illustrate the process of normalizing the data shown in Figure 13.25 to first (1NF), second (2NF), third (3NF), and BCNF.
 	 (c) Identify the primary, alternate, and foreign keys in your BCNF relations.

![8.png](https://i.loli.net/2021/05/22/1cVBktrguoy9D2I.png)

