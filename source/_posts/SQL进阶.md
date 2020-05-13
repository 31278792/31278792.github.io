---
title: SQL进阶
date: 2020-02-20 17:47:30
tags: 
	- SQL
	- MySQL
categories: 数据库
password: 31278792
---
## SQL进阶
### SQL编码问题
使用`SHOW VARIABLES LIKE 'char%';`可以查看当前窗口sql相关编码,在dos窗口内使用`set 你想修改的选项=gbk;`，这种修改在窗口关闭后将失效，也就是一次性修改。每次修改或许会很繁琐，所以也可以找到my.ini文件在里面进行修改，重启数据库系统，使修改配置生效   

<!--more-->

### mysql备份与恢复命令(即导入和导出)
```
mysqldump -uroot -proot mydb>d:/yzw.sql//备份
mysql -uroot -proot mydb<d:/yzw.sql//恢复
source d:/yzw.sql//在当前数据库恢复 
```

### 约束 

#### 主键约束(唯一标识)
**主键特点：非空、唯一、被引用(外键体现这一点)**
当表的某一列被指定为主键后，该列就不能为空，不能出现重复值
```
//当创建表时指定主键的两种方式
CREATE TABLE stu{
	sid		CHAR(6) PRIMARY KEY,
	sname   VARCHAR(20),
	sage    INT
};
CREATE TABLE stu{
	sid		CHAR(6),
	sname   VARCHAR(20),
	sage    INT,
	PRIMARY KEY(sid)
};

//修改表时指定主键
ALTER TABLE stu ADD PRIMARY KEY(sid);

//删除主键
ALTER TABLE stu DROP PRIMARY KEY;
```

#### 主键自增长
```
//创建表时指定主键自增长
CREATE TABLE stu{
	sid     INT PRIMARY KEY AUTO_INCREMENT,//自增长，数据类型必须是int
	sname   VARCHAR(20),
	age     INT
};
```

#### 非空约束
```
//sname设置非空约束
CREATE TABLE stu{
	sid     INT PRIMARY KEY AUTO_INCREMENT,
	sname   VARCHAR(20) NOT NULL,
	age     INT
};
```

#### 唯一约束
```
//sname设置唯一约束
CREATE TABLE stu{
	sid     INT PRIMARY KEY AUTO_INCREMENT,
	sname   VARCHAR(20) UNIQUE,
	age     INT
};
```

#### 外键约束
**外键必须引用主键，外键可以重复且可以为空，一张表中可以有多个外键**
对象模型：可以双向关联，而且引用的是对象，而不是一个主键
关系模型：只能多方引用一方，而且引用的只是主键，而不是一整行记录
```
CREATE TABLE dept{
	id		INT PRIMARY KEY AUTO_INCREMENT,
	dname 	VARCHAR(20)
};
//设置外键两种方式
//创建表时设置外键
CREATE TABLE emp{
	empno   INT PRIMARY KEY AUTO_INCREMENT,
	ename   VARCHAR(50),
	dno		INT,
	CONSTRAINT fk_emp_dept FOREIGN KEY(dno) REFERENCES dept(id)
};
//修改表设置外键
ALTER TABLE emp ADD CONSTRAINT fk_emp_dept FOREIGN KEY(dno) REFERENCES dept(id)
```

### 三种关联关系

#### 一对一
```
CREATE TABLE hasband{
	hid 	INT PRIMARY KEY AUTO_INCREMENT,
	hname 	VARCHAR(25)
}

CREATE TABLE wife{
	wid 	INT PRIMARY KEY AUTO_INCREMENT,//wid即作主键又作外键，非空，唯一，引用hid
	wname 	VARCHAR(25),
	hid		INT,
	CONSTRAINT fk_wife_hasband FOREIGN KEY(wid) REFERENCES hasband(hid)
}
```
#### 一对多
上面写外键约束用过，略

#### 多对多
```
CREATE TABLE student{
	sid 	INT PRIMARY KEY AUTO_INCREMENT,
	sname 	VARCHAR(25),
};
CREATE TABLE teacher{
	tid 	INT PRIMARY KEY AUTO_INCREMENT,
	tname 	VARCHAR(25),
};
CREATE TABLE stu_tea{
	sid		INT,
	tid		INT,
	CONSTRAINT fk_sutdent FOREIGN KEY(sid) REFERENCES subject(sid),
	CONSTRAINT fk_teacher FOREIGN KEY(tid) REFERENCES teacher(tid)
};
INSERT INTO student VALUES(NULL,"刘德华");
INSERT INTO student VALUES(NULL,"张学友");
INSERT INTO student VALUES(NULL,"郭富城");
INSERT INTO student VALUES(NULL,"黎明");
INSERT INTO student VALUES(NULL,"梁朝伟");
INSERT INTO student VALUES(NULL,"梁家辉");

INSERT INTO teacher VALUES(NULL,"崔老师");
INSERT INTO teacher VALUES(NULL,"史老师");
INSERT INTO teacher VALUES(NULL,"王老师");

INSERT INTO stu_tea VALUES(1,1);
INSERT INTO stu_tea VALUES(2,1);
INSERT INTO stu_tea VALUES(3,1);
INSERT INTO stu_tea VALUES(4,1);
INSERT INTO stu_tea VALUES(5,1);

INSERT INTO stu_tea VALUES(1,2);
INSERT INTO stu_tea VALUES(2,2);
INSERT INTO stu_tea VALUES(3,2);

INSERT INTO stu_tea VALUES(3,3);
INSERT INTO stu_tea VALUES(4,3);
INSERT INTO stu_tea VALUES(6,3);
```

### 多表查询

#### 合并结果集(合并两张表格)
```
CREATE TABLE ab{a INT,b VARCHAR(11)};
INSERT INTO ab VALUES(1,"1");
INSERT INTO ab VALUES(2,"2");
INSERT INTO ab VALUES(3,"3");

CREATE TABLE ab2{c INT,d VARCHAR(11)};
INSERT INTO ab2 VALUES(3,"3");
INSERT INTO ab2 VALUES(4,"4");
INSERT INTO ab2 VALUES(5,"5");

SELECT * FROM ab 
UNION ALL 		  //ALL去掉的话，完全相同的行将保留1个
SELECT * FROM ab2;//要求两张表结果集完全相同（即要查询的列类型相同）
```

#### 连接查询

##### 连接查询之内连接
方言：`SELECT * FROM 表1 别名1,表2 别名2 WHERE 别名1.xx=别名2.xx`
标准：`SELECT * FROM 表1 别名1 INNER JOIN 表2 别名2 ON 别名1.xx=别名2.xx`
自然：`SELECT * FROM 表1 别名1 NATURAL JOIN 表2 别名2`

##### 连接查询之外连接
左外连接：外连接一主一次，左外部左表为主，主表所有记录无论满足条件与否，都打印出来，不满足字段内容为null
```
SELECT * FROM 表1 别名1 LEFT OUTER JOIN 表2 别名2 ON 别名1.xx=别名2.xx
//示例：
SELECT e.ename,e.sal,IFNULL(d.dname,"无内容") AS dname
FROM emp e LEFT OUTER JOIN dept d
ON e.deptno = d.deptno;
```

右外连接：与左连接理解类似
```
SELECT * FROM 表1 别名1 RIGHT OUTER JOIN 表2 别名2 ON 别名1.xx=别名2.xx
//示例：
SELECT e.ename,e.sal,d.dname
FROM emp e RIGHT OUTER JOIN dept d
ON e.deptno = d.deptno;
```

全外连接：`SELECT * FROM 表1 别名1 FULL OUTER JOIN 表2 别名2 ON 别名1.xx=别名2.xx`*mysql不支持*
```
解决方式：
SELECT e.ename,e.sal,IFNULL(d.dname,"无内容") AS dname
FROM emp e LEFT OUTER JOIN dept d
ON e.deptno = d.deptno;
UNION //去除结果集实现全连接 
SELECT e.ename,e.sal,d.dname
FROM emp e RIGHT OUTER JOIN dept d
ON e.deptno = d.deptno;
```

#### 子查询
子查询就是查询中有查询（即出现多个select关键字）
```
//示例1：
SELECT * FROM emp WHERE sal=(SELECT MAX(sal) FROM emp);

//示例2：
SELECT e.empno,e.ename 
FROM (SELECT * FROM emp WHERE deptno=30) e

//示例3：
SELECT * FROM emp WHERE sal > ALL (SELECT sal FROM emp WHERE deptno=30)

//示例4：
SELECT * FROM emp WHERE sal > ANY (SELECT sal FROM emp WHERE deptno=20)
```

总结：
1.单行单列：`SELECT * FROM 表1 别名1 WHERE 列1 [=、>、<、<=、>=、!=] (SELECT 列 FROM 表2 别名2 WHERE 条件)`
2.多行单列：`SELECT * FROM 表1 别名1 WHERE 列1 [IN、ALL、ANY] (SELECT 列 FROM 表2 别名2 WHERE 条件)`
3.单行多列：`SELECT * FROM 表1 别名1 WHERE (列1,列2) IN (SELECT 列1 列2 FROM 表2 别名2 WHERE 条件)`
4.多行多列；`SELECT * FROM 表1 别名1 , (SELECT ...) 别名2 WHERE 条件`

