---
title: SQL复合查询（多表查询）
date: 2019-03-21 16:29:01
tags: ['sql']
categories: ['sql']
---
### 1.复合查询分类
* 内连接（inner Join 或 Join）
* 外连接（outer Join） 
  * 左外连接（left outer Join 或 left Join）
  * 右外连接（right outer Join 或 right Join）
  * 全外连接（full outer Join 或 full Join）
* 交叉连接 （cross Join）
* 结果集链接 （union 和 union all）

### 2.复合查询如何写
A表：
![A表](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/tableA.png)

B表：
![B表](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/tableB.png)

C表：

![C表](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/tableC.png)

#### 2.1内连接（Inner Join）
内连接：仅显示两个表中匹配行，即两表中都有才显示。

SQL如下：
``` sql
SELECT
    A.id AS AID,
    A.content AS AContent,
    B.id AS BID,
    B.content AS BContent
FROM
    A
INNER JOIN B ON (A.id = B.id)
```

查询结果： 

![结果](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/result1.png)

> 由查询结果可以看出，内连接根据连接条件（A.id=B.id）查询出了A、B两表中都存在的数据信息。2个表的联合查询结果如此，那么3个表甚至更多表联合查询的结果呢？

A、B、C三表联合内查询SQL

``` sql
SELECT
    A.id AS AID,
    A.content AS AContent,
    B.id AS BID,
    B.content AS BContent,
    C.id AS CID,
    C.content AS CContent
FROM
    A
INNER JOIN B ON (A.id = B.id)
INNER JOIN C ON (A.id = C.id)
```

查询结果： 

![结果](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/result2.png)

> 啊？怎么多了一行数据？不用惊讶，其实C表中有2个id为1的记录，然而我们怎么理解得到的查询结果呢？ 
> 可以把A、B两表的查询结果作为T表（中间结果表），然后T表内连接C表，连接条件为T.A.id=C.id。
> 简单来说n(n>=2)都可以看做两张表的联合查询，后面的小节将只介绍两个表的联合查询。

#### 2.2外连接（Outer Join）

##### 2.2.1左外连接（Left outer Join）

左外连接：左表有就显示，不论右表。

SQL：

``` sql
SELECT
    A.id AS AID,
    A.content AS AContent,
    B.id AS BID,
    B.content AS BContent
FROM
    A
LEFT JOIN B ON (A.id = B.id);
```

查询结果： 

![结果](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/result3.png)

> 左连接并不是把B表左连接到A表上，而是把A表作为基准表。由查询结果可以看出，A、B两表左连接，只要A中有结果，无论B表中有无结果，都会被查询出来。

##### 2.2.2右外连接（Right outer Join）

右外连接：右表有就显示，不论左表。

SQL：

``` sql
SELECT
    A.id AS AID,
    A.content AS AContent,
    B.id AS BID,
    B.content AS BContent
FROM
    A
RIGHT JOIN B ON (A.id = B.id);
```

查询结果： 

![结果](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/result4.png)

> 右连接和左连接类似，只是把B表（连接的表）作为基准表。由查询结果可以看出，无论A表是否存在其他数据，只要B表数据存在就会被查询出来。

##### 2.2.3全外连接（Full outer Join）

全外连接：左表/右表，有一个有就显示。

SQL：

``` sql
SELECT
    A.id AS AID,
    A.content AS AContent,
    B.id AS BID,
    B.content AS BContent
FROM
    A
FULL OUTER JOIN B ON (A.id = B.id);
```

查询结果： 

![结果](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/result5.png)

> 全外连接查询就字面意思也不难看出是查询出两表（A、B）中的所有记录信息。 
> 注：MySQL中不支持全外连接（但是可以union来实现，后面会介绍）。

#### 2.2交叉连接（Cross Join）

SQL:

``` sql
SELECT
    A.id AS AID,
    A.content AS AContent,
    B.id AS BID,
    B.content AS BContent
FROM
    A
CROSS JOIN B;
```

查询结果： 

![结果](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/result6.png)

> 由结果可以看出，交叉连接是对A、B量表进行笛卡尔积的结果查询出来。即A的每条记录都有和B中所有记录相对应的信息。

#### 2.3 SQL Union

> SQL Union用于将多个select结果集进行合并。值得注意的是，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。

SQL:

``` sql
SELECT * FROM A UNION SELECT * from B;
```

查询结果： 


![结果](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/result7.png)

> Union是把2个Select结果集进行合并，由查询结果也不难看出，A、B两表的结果数据进行了合并，并且都被查询出来了。 
> 如果2个Select结果集中存在相同的结果，用Union则会把相同的记录进行合并，查询结果中仅仅会显示一条。那么如果想都显示出来，把Union换成Union All 即可。

Union实现Full outer Join：

1.首先获取A、B表中id的不同组合。

SQL:

``` sql
CREATE VIEW v as  SELECT A.id from A UNION SELECT B.id from B;
```

视图内存如下： 

![结果](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/result8.png)

2.以视图V为基本表，Left Join A、B表即可。

SQL:

``` sql
SELECT
    A.id,
    A.content,
    B.id,
    B.content
FROM
    v
LEFT JOIN A ON (A.id = v.id)
LEFT JOIN B ON (B.id = v.id);
```

查询结果如下： 

![结果](https://raw.githubusercontent.com/wangmingdong/docImg/master/2019-03-21-sql-muti-table-query/result9.png)