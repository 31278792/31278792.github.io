---
title: SQL
date: 2020-02-12 15:03:10
tags: 
	- SQL
	- MySQL
categories: 数据库
password: 31278792
---
## SQL语言的概述
### SQL
- **SQL全称structured Query Language，即结构化程序语言**
- **SQL的作用：客服端使用SQL来操作服务器**
	>启动mysql.exe,连接服务器后，就可以用sql来操纵服务器了
	>将来会使用Java程序连接服务器，然后使用sql来操纵服务器
- **SQL标准：由国际化标准组织(ISO)制定的，对DBMS(即数据库管理系统)的统一操作方式**
	>目的是减少学习成本，不同数据库管理系统，相同语句进行操作
	>不过，每个数据库管理系统仍有一些差异，即使遵循国际化标准，但都预留了自己独有的方言
- **SQL方言：某种DBMS不只会支持SQL标准，而且还会有一些自己独有的语法，这就称之为方言**

<!--more-->
  
### SQL的用法
- **sql语句可以在单行或多行结尾书写，以分号结尾,但不是所有地方都需要分号结尾**
- **可以使用空格和缩进来增强语句的可读性**
- **MySQL不区别大小写，建议使用大写**

### SQL语句的分类
- **DDL(Date Definition Language)：数据定义语言，用来定义数据库对象：库、表、列等**（*重点*）
	>创建、删除、修改：库、表结构，即数据库或表的结果操作
- **DML(Date Manipulation Language)：数据操作语言，用来定义数据库记录(数据)**（*重点*）
    >增、删、改：表记录，即对表的记录进行更新(增、删、改)
- **DQL(Date Query Language)：数据查询语言，用来查询记录(数据)**（*重点+难点*）
	>对表记录的查询
- **DCL(Date Control Language)：数据库控制语言，用来定义访问权限和安全级别**
	>对用户的创建及授权

***

## DDL
### 数据库
- **查看所有数据库：SHOW DATEBASES**
- **切换（选择要操作的数据库）：USE 数据库名**
- **创建数据库：CREATE DATEBASES [IF NOT EXISTS] XXX [CHARSET=utf8]**（记住MySQL里utf没有‘-’）
- **删除数据库：DROP DATEBASES [IF EXISTS] XXX**
- **修改数据库编码：ALTER DATEBASES XXX CHARACTER SET utf8**

### 数据类型(列类型)
- **int:整型**
- **double:浮点型，示例double(6,2)表示最多6位，其中必须有2位小数，即最大值为9999.99**
- **decimal:浮点型，在表单关于钱方面使用该类型，因为不会出现精度缺失问题**
- **char:固定长度字符串类型：char(255)，数据长度不足指定长度，会补足指定长度**
- **varchar:可变长度字符串类型：varchar(65535),数据不足指定长度，不会补足指定长度，但需要消耗字节记录长度**
- **text(clob):字符串类型**
	a 很小
	b 小
	c 中
	d 大
- **blob:**
	a 很小
	b 小
	c 中
	d 大
- **date:日期类型，格式为：yyyy-MM-dd**
- **time:时间类型，格式为：hh:mm:ss**
- **timestamp:时间戳类型：既有时间又有年月日**

### 表
**创建表**
```
CREATE TABLE [IF NOT EXISTS] 表名(
	列名 列类型,
	列名 列类型,
	...
	列名 列类型,
);
```

**查看当前数据库中表名称**
`SHOW TABLES;`

**查看指定表的创建语句：SHOW CREATE TABLE 表名**（*了解*）

**查看表结构**
`DESC 表名;`

**删除表**
`DROP TABLE 表名;`

修改表：前缀：ALTER TABLE 表名
a 修改之添加列
```
ALTER TABLE 表名 ADD(
	列名 列类型,
	列名 列类型,
	...
	列名 列类型,
);
```

b 修改之修改列类型：(如果被修改的列已存在数据,那么新的类型可能会影响到已存在的数据)
`ALTER TABLE 表名 MODIFY 列名 列类型;`

c 修改之修改列名：
`ALTER TABLE 表名 CHANGE 原列名 新列名 列类型;`

d 修改之删除列：
`ALTER TABLE 表名 DROP 列名;`

e 修改表名称：
`ALTER TABLE 原表名 RENAME TO 新表名;`

---

## DML(数据操作语言，对表记录的增、删、改)
### 插入数据
```
INTSERT INTO 表名(
	列名1,
	列名2,
	...
)VALUES(
列名 列类型,
	列值1,
	列值2,
	...
);
```

**示例：**
```
INTSERT INTO stu(
	id, name, age
)VAUES(
	‘01’, 'zhangsan', 18
);
```

a 上面示例为指定列名插入所有值；
b 若指定部分列名插入部分值，则剩余未指定值的列，默认值为null；
c 若未指定列名，只给出值，则表示插入全部列，顺序按照建表时列名顺序插入值

### 修改数据
```
UPDATE 表名 SET 列名1=列值1,列名2=列值2,...[WHERE 条件];
```

- **条件**
	>条件必须满足是一个boolean类型的值或表达式，通常修改都需要写条件
	>涉及运算符：!=、=、<>、<、>、<=、>=、BETWEEN...AND、IN(...)、IS NULL、IS NOT NULL、NOT、OR、AND
	>这些运算符需要关注的是<>与!=都表示不等于的意思，而当=与SET一起使用是表示赋值
	>条件不能用XXX=null,只能用XXX IS NULL替代，用前一个条件永远为false
	
### 删除数据
```
DELETE FROM 表名 (WHERE 条件);
```

TRUNCATE TABLE 表名：TRUNCATE是DDL语句，它是先删除drop该表，再create该表，而且无法回滚！！！

---

## DQL(数据查询语言)
### 基本查询(即单表查询)
**一.字段(即列、属性，都行)**
1)查询所有列
```
SELECT * FROM 表名;
//这里"*"表示查询所有列
```

2)查询指定列
```
SELECT 列1, 列2, 列3,... 列n FROM 表名;
```

3)完全重复的记录只一次（即完全相同的记录只显示一个）
```
/*当查询结果中多行记录一模一样时，只显示一行，一般查询所有列时很少会有这种情况，但只查询一列（或几
列）时，这种可能就很大*/
SELECT DISTINCT * | 列1, 列2, 列3,...列n FROM 表名;
```

4)列运算
1.数量类型的列可以做加减乘除运算(切记查询不会对表进行修改，数据库不会发生变化，查询的结果只是临时视图)
```
SELECT 列名 * 1.4 FROM 表名;
SELECT 列名1 + 列名2 FROM 表名; //注意倘若其中一个列名中的列值为NULL做运算,结果为NULL
SELECT *,列名3 FROM 表名; //这个语句查询所有列且列名3将多出现1次
```

2.字符串类型的连接(但切记MySQL中不能用"+"连接字符串，不过下面的CONCAT可以实现字符串的连接)
```
SELECT CONCAT(列名1,列名2,...列名n) FROM 表名;
SELECT CONCAT('$',列名) FROM 表名;
```

3.转换NULL值
下面这句就解决了1.的列名中列值为NULL的运算问题
```
SELECT 列名1 + IFNULL(列名2,0) FROM 表名; /*这句意思是如果列名2中含有为NULL的列值，该值用0代
替，再进行运算*/
```

4.给列起列名
当我们在进行列运算后，查询出的结果集中的列名称不好看，这时我们需要为列名取别名，查询的结果集中列名就显示别名了
```
SELECT IFNULL(列名3,0)+1000 AS 别名 FROM 表名; //其中AS可以省略
SELECT 列名1 AS 别名1,列名2 AS 别名2 FROM 表名;
SELECT 列名1 别名1, 列名2 别名2 FROM 表名;//给列起别名，列名与别名中间没有逗号
```

**二.条件控制**
1)条件查询(与update、delete语句一样也可以使用where语句设限)
```
SELECT 列名1, 列名2, 列名3,...列名n FROM 表名 WHERE语句;
```

2)模糊查询
示例：
当你想查询姓张，并且姓名一共两个字的员工时，这时可以使用模糊查询
```
SELECT * FROM 表名 WHERE 列名 LIKE '张_';
/*
  上面语句查询的是姓张，名字由两个字组成的员工
  模糊查询需要使用运算符：LIKE，其中_匹配一个任意字符，注意：只匹配一个而不是多个  
*/
SELECT * FROM 表名 WHERE 列名 LIKE '___';//姓名由三个字组成的员工
```

如果我们想查询姓张，名字几个字都可以的员工时需使用'%'了
```
SELECT * FROM 表名 WHERE 列名 LIKE '张%';//其中%匹配0~n个任意字符，所以该句是查询姓张的所有员工
SELECT * FROM 表名 WHERE 列名 LIKE '%张%';/*特别注意这里不止代表查询名字中间含张字的员工，因为%
可以取0,所以以张字开头或结尾的也能查到*/
SELECT * FROM 表名 WHERE 列名 LIKE '%';//该条件等同于不存在，不过姓名为NULL的查询不出来
```

### 排序
1.升序
```
SELECT * FROM WHERE 表名 ORDER BY 列名 ASE;//排序升序是默认的，所以ASE可以省略
```

2.降序
```
SELECT * FROM WHERE 表名 ORDER BY 列名 DESC;//DESC不能省略
```

3.使用多列作为排序条件
```
SELECT * FROM WHERE 表名 ORDER BY 列名1 ASE, 列名2 DESC,...;
//该句意思是若升序排序后有相等情况，才执行相等的记录按降序排序
```

### 聚合函数（用来做某列的纵向查询）
#### COUNT
```
SELECT COUNT(*) FROM 表名;//所有行中，若有一行记录全为null不计数，也可以用任意数字替代
SELECT COUNT(列名) FROM　表名;//该列中没有null的行数
```

#### MAX
```
SELECT MIN(列名) FROM 表名;//该列中最大的列值
```

#### MIN
```
SELECT SUN(列名) FROM 表名;//该列中最小的列值
```

#### SUM
```
SELECT SUN(列名) FROM 表名;//该列中所有列值求和，字符串和NULL会被当0来参与运算
```

#### AVG
```
SELECT SUN(列名) FROM 表名;//该列中所有列值求平均值
```

#### 聚合函数综合示例
```
SELECT COUNT(*) AS 人数, SUN(xxx) 总和, MIN(xxx) 最小, MAX(xxx) 最大, AVG(xxx) 平均 FROM yzw; 
```

### 分组查询(查询都是组信息)
```
SELECT 分组列, 聚合函数1,... FROM 表名 GROUP BY 分组列;

SELECT job, COUNT(*), MAX(money) FROM yzw GROUP BY job;
//示例，意思是当前各职务(各组)人数,当前各职务中最大工资

SELECT 分组列, 聚合函数1,... FROM 表名 WHERE条件 GROUP BY 分组列 HAVING条件;
//where是分组前的条件，having是分组后的条件，不过分组后条件需用聚合函数作条件，参考下面示例
SELECT job, COUNT(*), MAX(money) FROM yzw WHERE num>10000 GROUP BY job HAVING COUNT(*) >= 2;
```

上面关键字的语句顺序或执行顺序：select、from、where、group by、having、order by 

### limit子句（方言）
LIMIT用来限定查询结果的起始行，以及总行数。
例如：查询起始行第五行，一共查询三行记录
`SELECT * FROM yzw LIMIT 4, 3;`
其中4表示从第五行开始，3表示一共查询三行，即第5、6、7行记录

---

## DCL(理解即可，不进行描述)

---












