---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-SQL-基础知识/","title":"tryhackme-SQL 基础知识","tags":["sql","tryhackme"]}
---


这次来看看sql的基础知识

![sql](/img/user/images/tryhackme-SQL-基础知识/sql.png)

学习目标：
- 了解数据库是什么以及关键术语和概念
- 了解不同类型的数据库 
- 了解SQL​
- 理解并能够使用SQL CRUD 操作
- 理解并能够使用SQL子句操作
- 理解并能够使用SQL操作
- 理解并能够使用SQL运算符
- 理解并能够使用SQL函数

## task2

数据库是结构化信息或数据的有组织的集合

两种主要类型：
- 关系数据库（Relational databases）（又名SQL）
- 非关系数据库（non-Relational databases）（又名 NoSQL）

关系数据库： 存储结构化数据，这意味着插入到此数据库中的数据遵循某种结构。

非关系数据库：要以非表格格式存储数据

关系数据库中存储的所有数据都将存储在表（table）中

创建此表时，您需要定义定义书籍记录所需的信息，例如“id”、“Name”和“Published_date”。这些将成为列（columns）定义这些列时，还需要定义此列应包含的数据类型

创建了定义列的列表之后 数据进入比如“id=1 name=ikun publish_data=2018"这串数据将会表示成行（row）

数据库中有两种键：

- 主键
  主键是唯一的，确保在某一列中收集的数据是唯一的 不会重复

- 外键
  外键是数据库中另一个表中也存在的表中的一个列（或多个列），因此提供了两个表之间的链接。

![task2](/img/user/images/tryhackme-SQL-基础知识/task2.png)

## task3
这次介绍的是sql

数据库通常使用数据库管理系统 (DBMS) 进行控制。DBMS 是最终用户和数据库之间的接口

以下是学习和使用SQL 的一些好处：

- 速度快： 关系数据库（即SQL用于的数据库）由于使用的存储空间少且处理速度快，因此几乎可以立即返回大量数据。 

- 易于学习： 与许多编程语言不同，SQL是用浅显易懂的英语编写的，因此更容易掌握。该语言的可读性高，这意味着用户可以专注于学习函数和语法。

- 可靠： 如前所述，关系数据库可以通过定义数据集必须符合的严格结构来保证数据的准确性。

- 灵活：  SQL在查询数据库时提供了各种功能；这使用户能够非常高效地执行大量数据分析任务。

![task3](/img/user/images/tryhackme-SQL-基础知识/task3.png)

## task4
### 数据库语法

1. 创建数据库
mysql> CREATE DATABASE database_name;

2. 显示数据库
mysql> SHOW DATABASES;

3. 使用数据库
mysql> USE database_name

4. 删除数据库
mysql> DROP database database_name; 

### 表语句
1. 创建表
```
mysql> CREATE TABLE example_table_name (
    example_column1 data_type,
    example_column2 data_type,
    example_column3 data_type
);
```

2. 显示表 
show table_name；

3. 描述
DESCRIBE也可以简称为DESC
查看表中包含哪些列（以及它们的数据类型）

mysql> DESCRIBE table_name；

来看问题 练练手

![task](/img/user/images/tryhackme-SQL-基础知识/tasl4.png)

第一问就是直接 show databases;就能看到flag

![task4](/img/user/images/tryhackme-SQL-基础知识/flag1.png)

第二问就是使用问题说的task4_db这个数据库 然后show databases;就能看到

![task4](/img/user/images/tryhackme-SQL-基础知识/flag2.png)

## task5
CRUD 语法

crub语法可以理解为增删改查这四个操作    

- INSERT 语句 - 向表中添加新记录。
- SELECT 语句 -从表中检索记录。
- UPDATE 语句 - 修改表中现有数据。
- DELETE 语句 - 从表中删除记录。

1. 创建操作（insert）
这个可以用insert into语法来解决
eg

```
insert into ("id","Name","Published_date") VALUE("1","ikun","2018"  )
```

2. 读操作（SELECT）
用select语句，用于从表中读取或检索信息。
```
select * from database_name
# *是检索所有列

select id,name from database_name
```

3. 更新操作（UPDATE）
更新操作会修改表中现有的记录
```
update database_name  set Name="xiaoheizi" where id=1

SET后跟要更新的列名。WHERE子句指定在满足子句时要更新哪一行，在本例中为id 为 1 的行。
```
4. 删除操作 (DELETE)
```
mysql> DELETE FROM books WHERE id = 1;
```

练练手

![task5](/img/user/images/tryhackme-SQL-基础知识/task5.png)

主要还是用select工具查出来 

![taKS](/img/user/images/tryhackme-SQL-基础知识/hacking.png)

第二问也一样 只是去找一下他们的类别

![task](/img/user/images/tryhackme-SQL-基础知识/usb.png)

## task6
子句可以帮助我们定义数据的类型以及如何检索或排序数据

1. DISTINCT子句
DISTINCT子句用于在查询时避免重复记录，仅返回唯一的值。

```
mysql> SELECT * FROM books;
+----+----------------------------+----------------+--------------------------------------------------------+
| id | name                       | published_date | description                                            |
+----+----------------------------+----------------+--------------------------------------------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture   |
|  2 | Bug Bounty Bootcamp        | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities |
|  3 | Car Hacker's Handbook      | 2016-02-25     | A Guide for the Penetration Tester                     |
|  4 | Designing Secure Software  | 2021-12-21     | A Guide for Developers                                 |
|  5 | Ethical Hacking            | 2021-11-02     | A Hands-on Introduction to Breaking In                 |
|  6 | Ethical Hacking            | 2021-11-02     |                                                        |
+----+----------------------------+----------------+--------------------------------------------------------+

6 rows in set (0.00 sec)
```

使用了distinct子句之后

```
mysql> SELECT DISTINCT name FROM books;
+----------------------------+
| name                       |
+----------------------------+
| Android Security Internals |
| Bug Bounty Bootcamp        |
| Car Hacker's Handbook      |
| Designing Secure Software  |
| Ethical Hacking            |
+----------------------------+

5 rows in set (0.00 sec)
```

把原来重复的数据排掉了 只返回唯一的值

2. GROUP BY子句
GROUP BY子句聚合来自多个记录的数据并将查询结果分组到列中。

```
mysql> SELECT name, COUNT(*)
    FROM books
    GROUP BY name;
+----------------------------+----------+
| name                       | COUNT(*) |
+----------------------------+----------+
| Android Security Internals |        1 |
| Bug Bounty Bootcamp        |        1 |
| Car Hacker's Handbook      |        1 |
| Designing Secure Software  |        1 |
| Ethical Hacking            |        2 |
+----------------------------+----------+

5 rows in set (0.00 sec)
```

3. ORDER BY 子句
ORDER BY子句可用于按升序或降序对查询返回的记录进行排序。使用ASC(升序)和之类的函数DESC(降序)

![task6](/img/user/images/tryhackme-SQL-基础知识/task6.png)

第一问 我们要加一个count(*)函数和GROUP BY函数
```
select category,count(*) from hacking_tools group by category;
```
![task6](/img/user/images/tryhackme-SQL-基础知识/6.png)

第二问
```
select * from hacking_tools order by name asc;
```
![task](/img/user/images/tryhackme-SQL-基础知识/asc.png)

能查出第一个是 bash bunny

第三问也是异曲同工之妙
```
select * from hacking_tools order by name desc;
```
![task](/img/user/images/tryhackme-SQL-基础知识/desc.png)

## task7
逻辑运算符
1. LIKE
LIKE运算符通常与子句一起使用

2. AND 运算符
运算符AND在查询中使用多个条件，如果所有条件都为真则返回。

3. OR 运算符
该OR运算符组合查询中的多个条件，并返回其中至少有一个条件为真时的结果

4. NOT 运算符
NOT运算符反转布尔运算符的值,能够排除特定条件。

5. BETWEEN 运算符
BETWEEN运算符允许我们测试某个值是否存在于定义的范围内。

比较运算符
用数学符号的运算符

| 运算符      | 说明                          |
|------------|------------------------------|
| `=`        | 等于运算符，判断是否相等      |
| `!=` 或 `<>` | 不等于运算符，判断是否不相等 |
| `<`        | 小于运算符，判断是否小于      |
| `>`        | 大于运算符，判断是否大于      |
| `<=`       | 小于或等于运算符              |
| `>=`       | 大于或等于运算符              |


![task7](/img/user/images/tryhackme-SQL-基础知识/task7.png)

第一问 用like 筛选出Multi-tool
```
select * from hacking_tools where category like "%Multi-tool%";
```

![task](/img/user/images/tryhackme-SQL-基础知识/zero.png)

第二问 能看到category是RFID cloning
```
 select * from hacking_tools where amount >="300";
```

![task](/img/user/images/tryhackme-SQL-基础知识/300.png)

第三问 需要用到where like和＜
```
select * from hacking_tools where category like "%Network intelligence%" and amount <"100";
```

![task](/img/user/images/tryhackme-SQL-基础知识/100.png)

## task8
### 字符串函数 

1. CONCAT() 函数
此函数用于将两个或多个字符串相加。它对于合并不同列的文本非常有用。

2. GROUP_CONCAT() 函数
此函数可以帮助我们将多行数据连接到一个字段。

3. SUBSTRING() 函数
此函数将从查询中的字符串中检索子字符串，从确定的位置开始。

4. LENGTH() 函数
此函数返回字符串中的字符数。这包括空格和标点符号。

### 聚合函数
函数聚合查询中一个指定条件内的多行值；它可以将多个值组合成一个结果。

1. COUNT() 函数
此函数返回表达式内的记录数

2. SUM() 函数
此函数对确定列的所有值（非 NULL）求和。

3. MAX() 函数
此函数计算表达式中提供的列中的最大值。

4. MIN() 函数
此函数计算表达式中提供的列中的最小值。

![task](/img/user/images/tryhackme-SQL-基础知识/task8.png)

第一问 用order by函数
```
select name from hacking_tools order by length(name);
```
![task](/img/user/images/tryhackme-SQL-基础知识/chang.png)

第二问 需要用到sum函数
```
select sum(amount) from hacking_tools;
```
![task](/img/user/images/tryhackme-SQL-基础知识/1444.png)


第三问
```
SELECT GROUP_CONCAT(name SEPARATOR " & ") AS name_nonzero FROM hacking_tools WHERE SUBSTRING(amount, -1, 1) != 0;

# SELECT 语句
SELECT GROUP_CONCAT(name SEPARATOR " & ") AS name_nonzero：查询的这一部分指定您想要选择一个字段，name_nonzero它将包含工具的连接名称。
GROUP_CONCAT(name SEPARATOR " & ")：
GROUP_CONCAT()是一个聚合函数，将多行的值连接成一个字符串。
指定SEPARATOR " & "连接字符串中的每个名称应以“&”分隔。
例如，如果名称是“工具 A”和“工具 B”，则结果将是“工具 A & 工具 B”。

# FROM 子句
FROM hacking_tools：这指定从中检索数据的表，在本例中为hacking_tools。

# WHERE 子句
WHERE SUBSTRING(amount, -1, 1) != 0：此子句根据与列相关的条件过滤行amount。
SUBSTRING(amount, -1, 1)：
该SUBSTRING()函数提取字符串的一部分。
这里，它用于获取amount字符串的最后一个字符：
第一个参数是amount列。
第二个参数-1表示它应该从最后一个字符开始。
第三个参数1表示它应该只提取一个字符。
!= 0：检查的最后一个字符是否amount不等于0。这意味着查询将仅考虑amount不以零结尾的行。
```

![task](/img/user/images/tryhackme-SQL-基础知识/last.png)

最后一个真的有点难度 查了一波资料和wp

sql就简单介绍到这