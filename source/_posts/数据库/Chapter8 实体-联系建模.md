---
title: 第八章 实体-联系建模
date: 2021-5-19 24:00:00
tags: 
categories: 数据库
---
## 第八章 实体-联系建模

<!-- more -->

**|ER建模|**：ER建模是一种自上而下的数据库设计方法， 该方法首先确定那些被称为实体的重要数据和这些数据之间的联系，实体和联系是ER模型中必备的元素。然后再添加更多的细节信息，例如描述实体和联系的属性信息，以及施加在实体、联系和属性上的约束。

我们采用的图形化符号集是一种日益流行的面向对象建模语言的符号集，称为**统一建模语言(Unified Modeling Language, UML)**。

![1.png](https://i.loli.net/2021/05/22/yJxDh3CHKY8FArN.png)

### 8.1 实体类型

**|实体类型|**：能够独立存在的具有相同属性的一组对象。

例：Staff

**|实体出现|**：实体类型中可唯一标识的一个对象。（实例）

例：S01

**实体类型的图形化表示**

实体类型用标有名字的矩形表示。

### 8.2 联系类型

**|联系类型|**：实体类型间的一组有意义的关联。

例：POwns

**|联系出现|**：由参与该联系的各个实体类型的一个出现组成的可被唯一标识的关联。

例：B01	Has	S01

**|语义网|**：●表示实体，◈表示联系。

**联系类型的图形化表示**

用线将相关的实体类型联系起来，并在线上标上联系的名字，通常为一个动词，并标记有方向。

![2.png](https://i.loli.net/2021/05/22/63HeyMZS1XQNOnl.png)

#### 8.2.1 联系类型的度

**|联系类型的度|**：参与联系的实体类型的个数。

**复杂联系的图形化表示**

UML用菱形符号表示大于2的联系，联系的名字放在菱形内部。

![3.png](https://i.loli.net/2021/05/22/859n4HZ1uoILKJd.png)

#### 8.2.2 递归联系

**|递归联系|**：同一个实体类型以不同的角色多次参与了同一个联系类型。

递归联系有时又叫一元联系。

![4.png](https://i.loli.net/2021/05/22/wur4jWQ3LncDUJl.png)

可以添加**角色名称**来表明每一个参与联系的实体类型在该联系中的意义，当两个实体之间存在多于一种联系时也可以使用角色名称。

<img src="https://i.loli.net/2021/05/22/lAJCFO7e413kWoP.png" alt="5.png" style="zoom:80%;" />

### 8.3 属性

**|属性|**：实体或联系类型所具有的某一特性。

**|属性域|**：单个属性或多个属性所允许的取值集合。

#### 8.3.1 简单属性和组合属性

**|简单属性|**：由独立存在的单个部分组成的属性。

简单属性不能再被划分为更小的部分，有时又称为原子属性。

**|组合属性|**：由多个部分组成的属性，每个部分都可独立存在。

例：address（163 Main St, Bei jing, 100083）

可以拆分为street, city和postcode三个属性

#### 8.3.2 单值属性和多值属性

**|单值属性|**：在实体类型的每个实例出现都只取一个单值的属性。

**|多值属性|**：对于实体类型的某些实例出现可能取多个值的属性。

例：电话号码可能有多个值

#### 8.3.3 导出属性

**|导出属性|**：属性的值是从相关的一个或一组属性（不一定来自同一个实体类型）的值导出来的属性。

例：age从Date中导出

#### 8.3.4 键

**|候选键|**：能够唯一标识每个实体的实例出现的最小属性组。

**|主键|**：被指定用来唯一标识实体类型的每个实例出现的候选关键字。

实体主键的选择要考虑属性的长度，以及该属性在以后是否仍具有唯一性。

**|合成键|**：包括两个或两个以上属性的候选键。

**属性的图形化表示**

如果要在一个实体类型中显示它的属性，可以将表示实体的矩形分成两部分。上面部分是实体的名字，下面部分列出属性的名字。

![6.png](https://i.loli.net/2021/05/22/FvSiJA3bEYwcC4L.png)

- 在UML中，属性名字用小驼峰。

- 在ER图中只显示主键属性（属性组）时，可以省略{PK}标记。

- 对于组合属性，可以在紧跟组合属性名的下一行开始以右缩进格式列出子属性名。

- 对于多值属性，要知名属性取值的个数范围。

- 对于导出属性，在属性名前加上符号“/”。

### 8.4 强实体类型与弱实体类型

**|强实体类型|**：该实体类型的存在不依赖于其他的实体类型。

**|弱实体类型|**：该实体类型的存在依赖于其他实体类型的存在。

强实体类型可以使用该实体类型的主键唯一标识每个实体的实例出现，弱实体类型没有主键，仅使用该实体类型的属性无法唯一标识每个实体的实例出现。

![7.png](https://i.loli.net/2021/05/22/RzbhlNsSyEem6pT.png)

### 8.5 联系的属性

**联系的属性的图形化表示**

采用与实体类型相同的符号，将表示属性的矩形和联系用虚线连接起来。

![8.png](https://i.loli.net/2021/05/22/z4vmxJHFteVqkAj.png)

### 8.6 结构化约束

联系的主要约束称为多重性。

**|多重性|**：指一个参与实体类型通过某一联系与另一参与实体类型的某个出现发生关联的数目。

多重性约束了实体间关联的方式，最常见的联系的度是二元的。

#### 8.6.1 一对一(1:1)联系

![9.png](https://i.loli.net/2021/05/22/rdhzoSxgFEMl2fc.png)

#### 8.6.2 一对多(1:*)联系

![10.png](https://i.loli.net/2021/05/22/sGVOD2HTXeUycuW.png)

#### 8.6.3 多对多(\*:*)联系

![11.png](https://i.loli.net/2021/05/22/bz4JD2LaHI53tfn.png)

#### 8.6.4 复杂联系的多重性

**|多重性(复杂联系)|**：在一个n元联系中，当其他(n - 1)个实体类型的值固定以后，另外一个实体类型可能参与联系的实例出现的个数。

![13.png](https://i.loli.net/2021/05/22/Kco8zY4pZh6f7Gn.png)

#### 8.6.5 基数约束和参与性约束

**|基数|**：在指定的联系类型中，一个实体可能参与的联系出现的最大数目。

- 多重性范围中的最大值

**|参与性|**：说明所有实体出现是否都参与了联系。

- 多重性范围中的最小值

参与性约束表示在一个联系中，时所有实体出现都参与了该联系（**强制参与**），还是只有一部分实体出现参与（**可选参与**）。

要注意的是，某个实体的参与性是由在联系中另外一方实体的多重性的最小值决定的。

![14.png](https://i.loli.net/2021/05/22/PNXKHrkYZlGyF21.png)

例：Staff是可选的，Branch是强制的

### 8.7 将ER转换关系

**|将ER转换为关系数据模型|**：为逻辑数据模型创建关系，用来表示已确定的实体、联系和属性。

#### 8.7.1 强实体类型

创建一个包含这个实体所有简单属性的关系

例：Staff(staffNo, sex, DOB)

#### 8.7.2 弱实体类型

创建一个包含该实体所有简单属性的关系，其中部分主关键字甚至全部都是从所有者实体中获取的。（主键要等到该弱实体与每一个父实体的联系转化完成后才能确定）

例：Preference(staffNo, prefType)

#### 8.7.3 1:*二元联系模型

左边作为父实体，右边作为子实体。将父实体的主键属性副本放到对应于子实体的关系中，作为该关系的外键。

![15.png](https://i.loli.net/2021/05/22/soMGDHW6yjAKXOP.png)

#### 8.3.4 1:1 二元联系模型

**双方都强制参与的1:1联系**

将联系中的两个实体合并到一个关系里，并选择原实体的一个主键作为新关系的主键。

例：Client (clientNo, fName, lName, telNo, prefType, maxRent, staffNo)

主键：clientNo

外键：staffNo references Staff(staffNo)

**一方强制参与的1:1联系**

联系中可选参与的实体被指定为父实体，强制参与的实体被指定为子实体。将父实体的主键副本放到子实体的关系中。如果这个联系本身还有一个或多个属性，则这些属性也应该放到子实体中。

![16.png](https://i.loli.net/2021/05/22/iN9mpRXLIlHca4q.png)

**双方都可选参与的1:1联系**

可以随意指定谁是父实体谁是子实体。

#### 8.7.5 1:1递归联系类型

与上述方法一样，区别在于需要重命名

#### 8.7.6 \*:*二元联系类型

为每个\*:*二元联系 创建一个关系，关系中包含了\*:\*二元联系的所有属性以及参与这个联系的实体的主键属性的副本，主键属性的副本作为外键。

![17.png](https://i.loli.net/2021/05/22/LhfMlC12yZ4sum5.png)

#### 8.7.7 复杂联系类型

为每一个复杂联系都创建一个关系，该关系包含了属于这个联系的所有属性，并将参与该联系的实体的主键属性也复制到这个关系中，作为关系的外键。

![18.png](https://i.loli.net/2021/05/22/BkyZ9azKPLY2onp.png)

#### 8.7.8 多值属性

对于实体中的每一个多值属性，都创建一个新的关系来表示这个多值属性，并且将实体的主键也复制到新建立的关系中，作为新关系的外键。除非多值属性本身是实体的可替换键，否则新关系的主键就是多值属性和实体的主键的组合。

![19.png](https://i.loli.net/2021/05/22/1mnO5SxZyJX3esc.png)

#### 8.7.9 将实体和联系转化为关系小结

![20.png](https://i.loli.net/2021/05/22/jscHvZ25L7NUnF6.png)

### 作业

1. Describe what entity types represent in an ER model and provide examples of entities with a physical or conceptual existence.

   实体类型是能够独立存在的具有相同属性的一组对象。实体类型可以是物理存在的对象，也可以是概念存在的对象。物理存在的对象有学生、货物，概念存在的对象有工作经验、熟练程度。

2. Describe what relationship types represent in an ER model and provide examples of unary, binary, ternary, and quaternary relationships.

   联系类型是实体类型间的一组有意义的关联。一元联系也被称为递归联系，只有一个实体类型参与。如员工（监管者）管理员工（被监管者）。二元联系如分公司雇有员工。三元联系如员工在某分公司登记了一名客户。四元联系如买主在法律顾问和金融机构的支持下进行投标。（画图）

3. Describe what attributes represent in an ER model and provide examples of simple, composite, single-value, multi-value, and derived attributes.

   属性是实体或联系类型所具有的某一特性。简单属性是由独立存在的单个部分组成的属性，如staffNo是关系Staff的简单属性。复合属性是由多个部分组成的属性，每个部分都可独立存在。如name属性可以由fname和lname组成。单值属性是在实体类型的每个实例都只取一个单值的属性，如Staff关系的staffNo属性在每个实例中只有一个值。多值属性是在实体类型的某些实例可能取多个值的属性，如一个人员工可能有多个电话，telNo属性在每个实例中有可能有多于一个的值。导出属性是从相关的一个或一组属性的值导出来的属性，如age属性可以从birthDate属性中导出。

4. Describe what the multiplicity constraint represents for a relationship type.

   多重性指一个参与实体类型通过某一联系与另一参与实体类型的某个出现发生关联的数目。多重性约束了实体间关联的方式。最常见的联系的度是二元的。二元联系通常又可以分为一对一、一对多或多对多的。

5. What are enterprise constraints and how does multiplicity model these constraints?

   企业级约束是由数据库用户或数据库管理员所指定的附加规则，它约束企业的某些方面。多重性约束了实体间关联的方式，它是用户或企业建立的策略的一种表示。

6. Describe the rules for deriving relations that represent:
    strong entity types;

  创建一个包含这个实体所有简单属性的关系

  weak entity types;

  创建包含所有简单属性的关系（主键要等到该弱实体与每一个父实体的联系转化完成后才能确定）

  one-to-many (1:\*) binary relationship types;

  将“一方”作为父实体，将“多方”作为子实体，将父实体的主键属性副本放到对应子实体的关系中，作为该关系的外键，该联系的所有属性也放到子实体的关系中。

  one-to-one (1:1) binary relationship types;

  (a)双方都为强制参与

  ​	将两个实体组合起来，用一个关系表示

  (b)一方强制参与

  ​	将可选参与的实体作为父实体，将强制参与的实体作为子实体，将父实体的逐渐属性副本放到对应子实体的关系中，作为该关系的外键，该联系的所有属性也放到子实体的关系中。

  (c)双方都为可选参与

  ​	可以随意选择父实体和子实体，并按照上述方法进行转换。

  one-to-one (1:1) recursive relationship types;

  ​	与1:1二元关系转换一样，区别在于需要重命名

  many-to-many (\*:*) binary relationship types;

  ​	为每个\*:*二元联系 创建一个关系，关系中包含了\*:\*二元联系的所有属性以及参与这个联系的实体的主键属性的副本，主键属性的副本作为外键。

  complex relationship types;

  ​	为每一个复杂联系都创建一个关系，该关系包含了属于这个联系的所有属性，并将参与该联系的实体的主键属性也复制到这个关系中，作为关系的外键。

  multi-valued attributes.

  ​	对于实体中的每一个多值属性，都创建一个新的关系来表示这个多值属性，并且将实体的主键也复制到新建立的关系中，作为新关系的外键。

7. Create an ER diagram for each of the following descriptions:
    (a) Each company operates four departments, and each department belongs to one company.
    (b) Each department in part (a) employs one or more employees, and each employee works for one department.
    (c) Each of the employees in part (b) may or may not have one or more dependents, and each dependent belongs to one employee.
    (d) Each employee in part (c) may or may not have an employment history.
    (e) Represent all the ER diagrams described in (a), (b), (c), and (d) as a single ER diagram.

  ![22.png](https://i.loli.net/2021/05/22/PvDw73jOpdlFSm9.png)

​	注：Employment History是0..\*不是1..\*

8. You are required to create a conceptual data model of the data requirements for a company that specializes in IT training. The Company has 30 instructors and can handle up to 100 trainees per training session. The Company offers five advanced technology courses, each of which is taught by a teaching team of two or more instructors. Each instructor is assigned to a maximum of two teaching teams or may be assigned to do research. Each trainee undertakes one advanced technology course per training session.
   (a) Identify the main entity types for the company.
   (b) Identify the main relationship types and specify the multiplicity for each relationship. State any assumptions you make about the data.
   (c) Using your answers for (a) and (b), draw a single ER diagram to represent the data requirements for the company.

9. Derive relations for the following conceptual data model:

![21.png](https://i.loli.net/2021/05/22/KhPxSC9I6tTUQvr.png)

