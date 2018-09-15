---
title: MySQL和SQL
date: 2018-08-19 09:45:29
tags: [MySQL,SQL]
categories: 数据库
---

### 数据库的基本操作

​	进入MySQL：mysql -u root -p



#### 查看所有数据库

```mysql
SHOW DATABASES;
```



#### 创建数据库

```mysql
CREATE DATABASE 数据库名 CHARSET=utf8;
```



#### 删除数据库

```mysql
DROP DATABASE 数据库名;
```



#### 切换数据库

```mysql
USE 数据库名;
```



### 数据表的基本操作

#### 数据类型

##### 数值类型

| 数据类型     | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| TINYINT      | 一个非常小的整数，可以带符号。如果是有符号，它允许的范围是从-128到127。如果是无符号UNSIGNED，允许的范围是从0到255，可以指定多达4位数的宽度。 |
| SMALLINT     | 一个小的整数，可以带符号。如果有符号，允许范围为-32768至32767。如果无符号，允许的范围是从0到65535，可以指定最多5位的宽度。 |
| MEDIUMINT    | 一个中等大小的整数，可以带符号。如果有符号，允许范围为-8388608至8388607。 如果无符号，允许的范围是从0到16777215，可以指定最多9位的宽度。 |
| INT          | 正常大小的整数，可以带符号。如果是有符号的，它允许的范围是从-2147483648到2147483647。如果是无符号，允许的范围是从0到4294967295。 可以指定多达11位的宽度。 |
| BIGINT       | 一个大的整数，可以带符号。如果有符号，允许范围为-9223372036854775808到9223372036854775807。如果无符号，允许的范围是从0到18446744073709551615. 可以指定最多20位的宽度。 |
| FLOAT(M,D)   | 不能使用无符号的浮点数。可以定义显示长度(M)和小数位数(D)。这不是必需的，并且默认为10,2，其中2是小数的位数，10是数字(包括小数)的总位数。 |
| DOUBLE(M,D)  | 不能使用无符号的双精度浮点数。可以定义显示长度(M)和小数位数(D)。 这不是必需的，默认为16,4，其中4是小数的位数。 |
| DECIMAL(M,D) | 非压缩浮点数不能是无符号的。定义显示长度(M)和小数(D)的位数是必需的。 |

##### 字符串类型

| 数据类型 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| CHAR     | 固定长度的字符串，长度为1到255之间个字符长度(例如：CHAR(5))，存储时右侧填充空格到指定的长度。 MySQL5中char的长度单位为字符 。 |
| VARCHAR  | 可变长度的字符串，以长度为1到255之间字符数(高版本的MySQL超过255)，例如： VARCHAR(32)。创建VARCHAR类型字段时，必须定义长度。MySQL5中varchar的长度单位为字符 。 |
| TEXT     | 字段的最大长度是65535个字符。不用指定TEXT的长度。            |

char 和 varchar 的区别：

char(M)是固定长度的字符串， 在定义时指定字符串列长。当保存数据时如果长度不够在右侧填充空格以达到指定的长度。M 表示列的长度，M 的取值范围是0-255个字符。

varchar(M)是长度可变的字符串，M 表示最大的列长度。M 的取值范围是0-65535。varchar的最大实际长度是由最长的行的大小和使用的字符集确定的，而实际占用的空间为字符串的实际长度+1。

##### 日期类型

| 数据类型 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| DATE     | 以YYYY-MM-DD格式的日期，在1000-01-01和9999-12-31之间。 例如，1973年12月30日将被存储为1973-12-30。 |
| TATETIME | 日期和时间组合以YYYY-MM-DD HH:MM:SS格式，在1000-01-01 00:00:00 到9999-12-31 23:59:59之间。例如，1973年12月30日下午3:30，会被存储为1973-12-30 15:30:00。 |

#####  

#### 数据完整性

##### 主键约束PRIMARY KEY

唯一且非空【NULL】，通俗的讲，就是在表中增加记录时，在该字段下的数据不能重复，不能为空。



##### 外键约束FOREIGN KEY

```mysql
CONSTRAINT 自定义外键名 FOREIGN KEY(被外键约束的字段名) REFERENCES 主表名(主键字段名);
```

1、外键约束可以描述任意一个字段(包括主键)，可以为空，并且一个表中可以有多个外键。但是外键字段必须是另一张表中的主键。

2、子表(从表)是拥有外键字段的表，父表(主表)是被外键字段所指向的表。

3、子表被外键约束修饰的字段必须和父表的主键字段的类型一样。



##### 非空约束NOT NULL

被该约束修饰了的字段，不能为空，主键约束中就包括了这个约束。



##### 唯一约束UNIQUE

被唯一约束修饰了的字段，表示该字段中的值唯一，不能有相同的值，通俗点讲，就好比插入两条记录，这两条记录中处于该字段的值不能是一样的。 



##### 默认约束DEFAULT

 指定这一列的默认值 。



##### 自动增加AUTO_INCREMENT

一个表只能有一个字段使用AUTO_INCREMENT，并且使用这个约束的字段只能是整数类型，默认值是1，也就是说从1开始增加的。



#### 创建数据表

```mysql
CREATE TABLE 数据表名(
					字段名1  数据类型[列级别约束条件] , 
					字段名2  数据类型[列级别约束条件] , 
					字段名3  数据类型[列级别约束条件] 
					);
```

​

#### 查询表结构

```mysql
DESC 表名;【表基本结构】
SHOW CREATE TABLE 表名\G; 【创建表的语句】
```



#### 修改数据表ALTER

##### 修改表名

```mysql
ALTER TABLE 旧表名 RENAME 新表名;
```



##### 修改字段名

```mysql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 数据类型;
```



##### 修改字段数据类型

```mysql
ALTER TABLE 表名 MODIFY 字段名 新数据类型;
```



##### 增加字段

```mysql
ALTER TABLE 表名 ADD 新字段名 数据类型;
```



##### 删除字段

```mysql
ALTER TABLE 表名 DROP 字段名;
```



##### 修改表的存储引擎

```mysql
ALTER TABLE 表名 ENGINE=存储引擎名;
```



#### 删除数据表

```mysql
DROP TABLE 表名;
```



### 数据表中数据的修改

#### 插入数据INSERT

##### 插入单条数据

```mysql
INSERT INTO [表名(所有字段)] VALUES(和字段对应的值);
```



##### 插入多条数据

```mysql
INSERT INTO VALUES(记录行值1)，(记录行值2)...;
```



##### 插入指定字段数据

```mysql
INSERT INTO 表名(字段名1，字段名2) VALUES(值1，值2);
```



#### 更新数据UPDATE

```mysql
UPDATE 表名 SET 字段名1=值1，字段名2=值2 [WHERE <condition>];
```



#### 删除数据DELETE

```mysql
DELETE FROM 表名 [WHERE <condition>]; 
```



```mysql
"""
学生表student(sno, sname, ssex, sbirth)[学号（主键），学生名，性别，出生年月]
课程表course(cno, cname, tno)[课程编号（主键），课程名，教师编号（外键）]
成绩表score(sno, cno, degree)[学号（外键），课程编号（外键），成绩]
教师表teacher(tno, tname, tsex, prof)[教师编号（主键），教师名，性别，职称]
"""
#查看所有数据库
SHOW DATABASES;

#创建数据库
CREATE DATABASE school CHARSET=utf8;

#切换数据库
USE school;

#创建学生表
CREATE TABLE student(
    sno INT PRIMARY KEY,
    sname VARCHAR(32) NOT NULL,
    ssex CHAR(2) NOT NULL,
    sbirth DATE
);

#插入数据
INSERT INTO student VALUES
(101,'曾华','男','1993-9-12'),
(102,'李军','男','1994-3-16'),
(103,'李飞飞','女','1995-1-22'),
(104,'刘小白','男','1994-9-18'),
(105,'王芳','女','1995-10-1'),
(106,'陆小凤','男','1993-3-19'),
(107,'周正','男','1994-5-8'),
(108,'张雪','女','1993-3-9'),
(109,'赵日天','男','1993-8-11'),
(110,'陈真','男','1993-3-28');

#创建教师表
CREATE TABLE teacher(
    tno INT PRIMARY KEY,
    tname VARCHAR(32) NOT NULL,
    tsex CHAR(2) NOT NULL,
    prof VARCHAR(16)
);

#插入数据
INSERT INTO teacher VALUES
(501,'张旭','男','讲师'),
(502,'李萍','女','助教'),
(503,'周雪','女','副教授'),
(504,'赵斌','男','讲师'),
(505,'王菲','女','讲师');

#创建课程表
CREATE TABLE course(
    cno INT PRIMARY KEY,
    cname VARCHAR(32) NOT NULL,
    tno INT NOT NULL,
    CONSTRAINT course_tno FOREIGN KEY(tno) REFERENCES teacher(tno)
);

#插入数据
INSERT INTO course VALUES
(10, '计算机科学', 501),
(11, '高等数学', 503),
(12, '线性代数', 505),
(13, '算法导论', 504),
(14, '编程方法学', 502),
(15, '编程范式', 501),
(16, '抽象编程', 503);

#创建成绩表
CREATE TABLE score(
    sno INT NOT NULL,
    cno INT NOT NULL,
    degree FLOAT,
    CONSTRAINT score_sno FOREIGN KEY(sno) REFERENCES student(sno),
    CONSTRAINT score_cno FOREIGN KEY(cno) REFERENCES course(cno)
);

#插入数据
INSERT INTO score VALUES
(101,10,94),
(101,11,92),
(101,13,88),
(101,16,84),
(102,10,90),
(102,11,74),
(102,13,88),
(102,15,80),
(103,10,92),
(103,11,85),
(103,13,79),
(103,16,80),
(104,10,91),
(104,11,83),
(104,13,90),
(104,14,81),
(105,10,86),
(105,11,77),
(105,13,56),
(105,14,68),
(106,10,55),
(106,11,69),
(106,12,70),
(106,13,52),
(107,10,74),
(107,11,66),
(107,13,58),
(107,14,50),
(108,10,88),
(108,11,83),
(108,12,78),
(108,13,74),
(109,10,70),
(109,11,66),
(109,13,58),
(109,15,55),
(110,10,88),
(110,11,90),
(110,13,90),
(110,16,86);
```



### 数据表中数据的查询SELECT

#### 单表查询

```mysql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
```



##### 聚合函数

```mysql
SUM
COUNT
AVG
MAX
MIN
```

```mysql
#1、查询学生表的所有记录
SELECT * FROM student;

#2、查询学生表中学生名和性别的所有记录
SELECT sname,ssex FROM student;

#3、查询学生表中男同学的所有记录
SELECT * FROM student WHERE ssex='男';

#4、查询成绩表中分数在60到80之间的所有记录
方式一：
SELECT * FROM score WHERE degree>60 AND degree<80;
方式二：
SELECT * FROM score WHERE degree BETWEEN 60 AND 80;

#5、查询成绩表中分数为66、77或88的记录
SELECT * FROM score WHERE degree IN(66,77,88);

#6、查询学生表中“李”姓同学的记录
SELECT * FROM student WHERE sname LIKE '李%';

#7、按课程编号升序、成绩降序查询成绩表的所有记录
SELECT * FROM score ORDER BY cno ASC, degree DESC;

#8、查询每门课的平均成绩
SELECT cno,AVG(degree) FROM score GROUP BY cno;

#9、查询平均成绩大于80分的同学的学号和平均成绩
SELECT sno,AVG(degree) FROM score GROUP BY sno HAVING AVG(degree)>80;
```



#### 多表查询

```mysql
INNER JOIN
LEFT/RIGHT JOIN
UNION
```



```mysql
#1、查询所有学生的姓名、课程号和成绩
SELECT b.sname,a.cno,a.degree FROM score AS a,student AS b WHERE a.sno=b.sno;

#2、查询成绩表中的最高分的记录
SELECT * FROM score WHERE degree=(SELECT MAX(degree) FROM score);

#3、查询“10”课程比“11”课程成绩高的所有学生的学号 
SELECT a.sno FROM 
(SELECT sno,degree FROM score WHERE cno=10) a,
(SELECT sno,degree FROM score WHERE cno=11) b 
WHERE a.degree>b.degree AND a.sno=b.sno;   

#4、查询所有同学的学号、姓名、选课数、总成绩
 SELECT a.sno,b.sname,COUNT(a.cno),SUM(a.degree) 
 FROM score AS a LEFT JOIN student AS b 
 ON a.sno=b.sno GROUP BY sno;

#5、查询学过“李萍”老师课的同学的学号、姓名
SELECT sno,sname FROM student WHERE sno IN
(SELECT DISTINCT(score.sno) FROM course,teacher,score 
WHERE teacher.tname='李萍' AND teacher.tno=course.tno AND course.cno=score.cno);

#6、查询学过“周雪”老师所教的所有课的同学的学号、姓名
SELECT sno,sname FROM student WHERE sno IN
(SELECT sno FROM score,course,teacher 
WHERE score.cno=course.cno AND teacher.tno=course.tno AND teacher.tname='周雪' 
GROUP BY sno HAVING COUNT(score.cno)=
(SELECT COUNT(cno) FROM course,teacher WHERE teacher.tno=course.tno AND tname='周雪')); 

#7、查询所有课程成绩大于80分的同学的学号、姓名
SELECT sno,sname FROM student WHERE sno NOT IN 
(SELECT a.sno FROM student AS a,score AS b WHERE a.sno=b.sno AND degree<80); 

#8、统计各科成绩,各分数段人数:课程ID,课程名称,100至85分,85至70分,70至60分,小于60分 
SELECT score.cno AS 课程ID,cname AS 课程名称,
SUM(CASE WHEN degree BETWEEN 85 AND 100 THEN 1 ELSE 0 END) AS 100至85分,
SUM(CASE WHEN degree BETWEEN 70 AND 85 THEN 1 ELSE 0 END) AS 85至70分,
SUM(CASE WHEN degree BETWEEN 60 AND 70 THEN 1 ELSE 0 END) AS 70至60分, 
SUM(CASE WHEN degree < 60 THEN 1 ELSE 0 END) AS 小于60分
FROM score,course 
WHERE score.cno=course.cno 
GROUP BY score.cno;
```



### 索引INDEX

MySQL官方对索引的定义为：索引(Index)是帮助MySQL高效获取数据的数据结构。 更通俗的说，数据库索引好比是一本书前面的目录，能加快数据库的查询速度。 MySQL在300万条记录左右性能开始逐渐下降，所以大数据量建立索引是非常有必要的。 

##### 普通索引

允许在定义索引的列中插入重复值和空值 。

```mysql
 CREATE INDEX 索引名 ON 表名(字段名);
```



##### 唯一索引

索引列中的值必须是唯一的，但是允许为空值 。

```mysql
 CREATE UNIQUE INDEX 索引名 ON 表名(字段名）;
```



##### 主键索引

是一种特殊的唯一索引，不允许有空值。



##### 删除索引

```mysql
DROP INDEX 索引名 ON 表名;
```



##### 查询索引

```mysql
SHOW INDEX FROM 表名\G;
```



##### 注意

索引虽然能非常高效的提高查询速度，同时却会降低更新表的速度。实际上索引也是一张表，该表保存了主键与索引字段，并指向实体表的记录，所以索引列也是要占用空间的。 



### 参考资料

[MySQL(三) 数据库表的查询操作【重要】][1]

[1]: https://www.cnblogs.com/whgk/p/6149009.html







 