结构化查询语言
关系型数据库MySQL（独有LIMIT）, Oracle, SQL Server
SQL语句的分类：
1. DDL (Data Definition Language) 定义数据库对象
2. DML (Data Manipulation Language) 定义数据库记录
3. DCL (Data Control Language) 定义访问权限和安全级别
4. DQL (Data Query Language) 查询语言
## DDL
```
show databases; 查看所有数据库
use mydb 切换到mydb数据库
create database [mydb]; 创建库，若已存在会报错
create database if not exist [mydb] 避免报错
drop database [mydb] 删除库，若不存在会报错

create table [tablename] (coloumname1 colomutype1, ...) 创建表
dese [tablename]; 查看表结构
drop table [tablename] 删除表
修改ALTER 
添加列 alter table [tablename] add (列名 类型)
修改列的类型 alter table [tablename] modify 列名 列类型；
修改列名 alter table [tablename] change [oldname] [newname] [type];
删除列 alter table [tablename] drop [coloumname];
修改表名称 alter table [tablename] rename to [newname];
```


修改数据库编码
```
ALTER database [mydb] character set utf8
```

数据类型
常规：int, double, char, decimal
varchar()固定字符长度字符串,text, blob, date (yyyy-mm-dd), time(    hh:mm:ss)

## DML 数据库操作语言
插入数据
```
INSERT INTO [tablename](coloum1, coloum2..) VALUES(....)
若没有指定要插入的列则顺序插入全部
INSERT INTO [tablename] VALUES(...)
全部字符串用单引号
插入多条 INSERT INTO [tablename](coloum1,..) VALUES(),(),....
```
修改数据
```
UPDATE [tablename] SET coloumname1=value1, ....
```
删除数据
```
DELETE FROM [tablename] 可回滚
TRUNCATE TABLE [tablename]; 效率更快，实际上是DDL语句，先DROP再CREATE，记录无法回滚
```

## DCL 数据控制语言
```
创建用户 地址是localhost等
create user '用户名'@地址 IDENTIFIED BY '密码';
授权用户
GRANT [权限名用,隔开] ON mydb.* TO '用户名'@地址
权限名有ALL, CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT
授权撤销 
REVOKE [权限] ON mydb .* FROM user@localhost
查看授权 SHOW GRANTS FOR '用户名'@地址
删除用户 DROP USER name@address
修改密码（以root身份） 
use mysql;
alter user '用户名'@地址 identified by 'pass';
```
## DQL 数据查询语言
不会改变数据库，只是发送给客户端
SQL语句执行过程from-->where-->group by-->having-->select-->order by
where不能和聚合函数一起使用
书写顺序select, from, where, group by, having, (select), order by, limit

```
SELECT 列名/*
FROM [tablename]
WHERE condition (=, !=, <>, <, <=, >, >=, BETWEEN...AND.., IN(set), IS NULL, AND, OR, NOT)
LIKE 模糊查询 %匹配0或多个字符，中文用2个%，_下划线匹配多个字符 like '__i'
GROUP BY 分组
HAVING 对分组后的数据进行约束
ORDER BY 对查询结果的排序，默认增序ASC，降序DESC
LIMIT 限定查询起始行 LIMIT start offset（查几行）
e.g. LIMIT 2从索引0读2个；LIMIT 2,1从第2条开始读1条信息；
LIMIT 2 OFFSET 1：从第1条（不包括）数据开始取出2条数据，limit后面跟的是2条数据，offset后面是从第1条开始读取，即读取第2,3条

字段控制查询
DISTINCT去重；可做运算 SELECT a+b AS alias；
把column中的NULL换成0:ISNULL(comm,0)

联合函数
COUNT(列) 统计不为NULL的个数
MAX, MIN, SUM, AVG 非数值列计为0
```

NULL
1. 与所有的比较都为false，少用于比较
2. 加所有的值都为NULL
3. 排序时默认为最小值
4. ISNULL(a,0) 列a不为NULL时返回值，为NULL时返回0
5. NULLIF(num,0) num为0时返回NULL，防止除0错误

多表联合
内连接默认INNER JOIN = JOIN... ON
左连接left join tableName as b on a.key \==b.key
右连接right join  
连接union(无重复(过滤去重))和union all(有重复[不过滤去重])

多表连接
	--union 并集  
	--union all(有重复)  
	oracle(SQL server)数据库
	--intersect 交集   
	--minus(except) 相减(差集)

### 高级教程
```
SELECT TOP [num] FROM [tablename] 选取头条
SELECT * INTO [newtable] FROM [oldtable] 复制数据并插入新表
```

SQL 中的约束
NOT NULL; UNIQUE 保证每列的某行有唯一值
PRIMARY KEY (NOT NULL和UNIQUE结合)
FOREIGEN KEY 保证一个表的数据匹配另一个表的值
CHECK 保证列中值符合指定条件
DEFAULT 规定没有给列赋值时的默认值
AUTO_INCREMENT 插入新数据自主创建主key

视图VIEW
```
CREATE VIEW [viewname] AS ...;
CREATE OR REPLACE VIEW [viewname] AS... 更新视图
DROP VIEW [viewname]; 撤销
可以被嵌套，为保证视图是可更新的，不能包含以下语法：
集合操作符、DISTINCT、聚合函数、分析型函数、GROUP BY、ORDER BY、CONNET BY、START WITH、在SELECT之后列表中使用集合表达式，子查询JOIN

对于不可更新视图，用INSTEAD OF触发器修改数据，有TOP时可用ORDER BY
视图不能对临时表或表变量进行引用
```

窗口函数
```
SELECT ..()OVER(PARTITION BY name ORDER BY.. ASC) AS.. FROM.. 
```
SQL Scalar函数：基于input返回单一的值
```
UCASE() 转为大写。LCASE（）转为小写  MID（）提取字符
SUBSTRING（字段，1，end)提取字符
LEN（）返回文本长度
ROUND（）四舍五入小数
NOW（） 返回当前系统日期和时间
FORMAT（） 格式化显示方式
FIRST（）指定列中第一个记录的值
LAST（）
STUFF（）删除并插入
CHARINDEX() 返回指定字符串出现的起始位置，从1开始
LOCATE()
SOUNDEX() 返回字符串声音
COALESCE() 返回第一个非空的值，若都为空返回NULL
```

数据库系统有MS SQL SERVER、IBM DB2、Oracle、MySQL、MS ACCESS

单引号会导致SQL注入：数据库中的数据都会被盗取
子查询中，父查询一般使用IN运算符的是：单列多值嵌套查询
聚合函数不能嵌套使用MAX(SUM(score))不行，COUNT(DISTINCT())可以


数据库设计
1. 需求分析：通过调查分析用户业务流程，得出需求，用数据流程图，数据字典描述出需求。
2. 数据库概念设计：与具体DB管理系统无关
	1. 外模式：抽象出各用户所要求的数据视图
	2. 模式：综合为全局的数据视图
	3. 概念数据Model：ER模型/对象model
3. 数据库逻辑设计：Model转为关系模式，优化处理
4. 物理设计：组织储到物理介质上[内模式]
5. 安全设计：合法用户权限

三级模式
1. 模式：概念级，由DDL定义，所有数据的逻辑结构和特征总体描述
2. 外模式：用户级，DML数据视图
3. 内模式：存储，物理级
三模式间映射（二级映像）
1. 逻辑性：通过外模式+模式映射，定义和建立对应关系，当模式改变只要改变映射，即可使用外模式保持不变，对应应用程序不变
2. 物理性：模式+内模式映射，建立逻辑-存储对应关系

SQL Server使用存储过程中的优点：
执行速度快；封装复杂操作；允许模块化程序设计；减少网络流量（只用传送调用语句，不用发很多代码）


-- SQL
DELETE FROM <表名> [WHERE <条件>] 没有*
-- 完整的SELECT查询顺序
SELECT DISTINCT column, AGG_FUNC(column_or_expression),col_expression AS expr_description …
FROM mytable AS mytable
    INNER/LEFT/RIGHT/FULL JOIN another_table
      ON mytable.column = another_table.column
    WHERE constraint_expression AND/OR column IS/IS NOT NULL
    GROUP BY column
    HAVING constraint_expression
    ORDER BY column ASC -- 升序/DESC降序 （默认是升序）
    LIMIT count OFFSET COUNT;


DISTINCT 不会有重复的

AS 别名可以用在表上和属性上

-- 多表查询:Normalization 数据库范式是数据表设计的规范，在范式规范下，数据库里每个表存储的重复数据降到最少（这有助于数据的一致性维护），同时在数据库范式下，表和表之间不再有很强的数据耦合，可以独立的增长。主键(primary key) 唯一标识，在一个表中不能重复，可以把两个表中具有相同 主键ID的数据连接起来。

A join B, left join:保留A的全部和交集；right join保留B的所有行和交集；inner join交集；full join全部

*MYSQL中，NULL值与任何值值比较永不为真*

GROUP BY 可以对具有相同属性的行进行求和等计算
HAVING group_condition; 对分组之后的数据再删选，用HAVING进行筛选=不用group by时简单的where

LIMIT 和 OFFSET 子句通常和ORDER BY 语句一起使用，当我们对整个结果集排序之后，我们可以 LIMIT来指定只返回多少行结果 ,用 OFFSET来指定从哪一行开始返回。你可以想象一下从一条长绳子剪下一小段的过程，我们通过 OFFSET 指定从哪里开始剪，用 LIMIT 指定剪下多少长度。从第三位开始OFFSET=2

-- operator关键字
AND OR
=, !=, <>, <, <=, >, >=
BETWEEN A AND B (inclusive)
NOT BETWEEN A AND B (exclusive)
IN(...) exist in a list
NOT IN(..)
-- 模糊查询LIKE 
LIKE = "="
NOT LIKE = "!="
-- 通配符%
LIKE "%AT%" 表示AT前后可以有任何字符
LIKE "AN_" 代表AN后有且仅有一个字符

-- 统计函数
COUNT(*), COUNT(column)	计数！COUNT(*) 统计数据行数，COUNT(column) 统计column非NULL的行数.
MIN(column)	找column最小的一行.
MAX(column)	找column最大的一行.
AVG(column)	对column所有行取平均值.
SUM(column)	对column所有行求和.


-- 例题

SELECT * FROM TableName;  查看全表
SELECT 1+1 可以直接做计算

-- 列出所有在Chicago西部的城市，从西到东排序(包括所有字段)
SELECT * FROM north_american_cities where Longitude<
(SELECT Longitude from north_american_cities where city="Chicago") 
ORDER BY Longitude ASC;

-- 找到所有办公室里的所有角色（包含没有雇员的）,并做唯一输出(DISTINCT) 所有的办公室都需要列出来
SELECT DISTINCT Building_name,Role FROM Buildings
LEFT JOIN employees on building_name=building;

-- 按角色(Role)统计一下每个角色的平均就职年份 
SELECT Role, AVG(Years_employed) FROM employees 
group by Role;

-- 按角色分组算出每个角色按有办公室和没办公室的统计人数(列出角色，数量，有无办公室,注意一个角色如果部分有办公室，部分没有需分开统计） 属性有：Role	Name	Building	Years_employed
SELECT COUNT(Name), Role,CASE when Building is NOT NULL 
THEN '1' ELSE '0' END AS bn
FROM Employees
GROUP BY Role,bn;

-- 按导演分组计算销售总额,求出平均销售额冠军（统计结果过滤掉只有单部电影的导演，列出导演名，总销量，电影数量，平均销量) 
SELECT Director, SUM(Domestic_sales+International_sales)as total,
count(*) as number,SUM(Domestic_sales+International_sales)/count(title) as average
FROM movies 
INNER JOIN Boxoffice on movie_id=id
group by director
HAVING COUNT(Title) > 1
order by average desc
limit 1;

-- 找出每部电影和单部电影销售冠军之间的销售差，列出电影名，销售额差额 变态难啊阿啊
SELECT Title, (SELECT MAX(Domestic_sales+International_sales) 不能是单个数值
FROM Boxoffice) - SUM(Domestic_sales+International_sales) 
AS Diff FROM Movies INNER JOIN Boxoffice 
ON Movies.ID = Boxoffice.Movie_id 
GROUP BY Title; 


数据定义（SQL DDL）用于定义SQL模式、基本表、视图和索引的创建和撤消操作。
数据操纵（SQL DML）数据操纵分成数据查询和数据更新两类。数据更新又分成插入、删除、和修改三种操作。
数据控制（DCL）包括对基本表和视图的授权，完整性规则的描述，事务控制等内容。
嵌入式SQL的使用规定（TCL）涉及到SQL语句嵌入在宿主语言程序中使用的规则。


Rank()函数：为结果集分区中每一行分配一个排名，行等级由一加上前面的等级指定。 
RANK（） OVER（ 
    PARTITION BY　表达式      ##将结果集划分为分区
    ORDER BY　表达式　［ASC｜DESC］ ##对分区内的进行排序
） 

创建视图的语法为CREATE VIEW view_name[column] AS select_statement [WITH CHECK OPTION]

Mysql中表student_table(id,name,birth,sex)，查询男生、女生人数分别最多的3个姓氏及人数，正确的SQL是（）？ 使用UNION all
SELECT * from
(SELECT sex ,substr(name,1,1) as first_name ,count(*) as c1 
from student_table 
where length(name) >=1 and sex = '男'
group by first_name 
order by sex ,c1 desc limit 3
)t1
UNION all
SELECT * from
(SELECT sex ,substr(name,1,1) as first_name ,count(*) as c1 
from student_table 
where length(name) >=1 and sex = '女'
group by first_name 
order by sex ,c1 desc limit 3
)t2;

在用union组合查询时，只能使用一条order by子句，它必须位于最后一条select语句之后。对于结果集，不存在用一种方式排序一部分，而用另一种方式排序另一部分的情况，因此不允许使用多条order by子句。


授予用户SQLTest对数据库Sales的CUSTOMERS表的列cid、cname的查询权限
grant select on CUSTOMERS(cid,cname) to SQLTest


SQL Server存储过程中的优缺点：
优点：执行速度快，允许组件式编程，减少网络流量，提高系统安全性
缺点：移植性差，难以调试和维护，服务器不能负载均衡

SELECT ROUND(2.35) 四舍五入保留整数 2
SELECT ROUND(1.96,1) 四舍五入保留小数点后一位 2.0
SELECT TRUNCATE(1.99,1) 对前面参数进行截取操作，截至小数点后一位，结果为1.9
SELECT TRUNCATE(2.83,0) 对前面参数进行截取操作，截至小数点，结果为2

```
SELECT STUFF('lo ina', 3, 1, 've ch') 在第三个位置删掉1个字符并插入后面的
--> love china

CASE WHEN..THEN.. WHEN.. THEN.. ELSE.. END 
```

数据库备份
1. 完整DB备份： ALL包括事务日志部分
2. 差异DB备份：仅记录最近一次备份改变数据
3. 事务日志备份：不备DB本身，只有日志
4. 文件和文件组备份

sql合法标识符：不用数字开头
批处理：
1. 一条或多条T-SQL语句组
2. 不能把规则和默认值绑定到表字段或自定义字段上后立即同一批使用
3. 修改一个表中字段名后，不能在同批次引用新字段
4. create default, create rule等语句同一批处理中可同时提交多个

# SQL

## 完整的SELECT查询顺序
```
SELECT DISTINCT column, AGG_FUNC(column_or_expression),col_expression AS expr_description …
FROM mytable AS mytable
    INNER/LEFT/RIGHT/FULL JOIN another_table
      ON mytable.column = another_table.column
    WHERE constraint_expression AND/OR column IS/IS NOT NULL
    GROUP BY column
    HAVING constraint_expression
    ORDER BY column ASC升序/DESC降序 （默认是升序）
    LIMIT count OFFSET COUNT;
```

>
> DISTINCT 不会有重复的

> AS 别名可以用在表上和属性上

> 多表查询:Normalization 数据库范式是数据表设计的规范，在范式规范下，数据库里每个表存储的重复数据降到最少（这有助于数据的一致性维护），同时在数据库范式下，表和表之间不再有很强的数据耦合，可以独立的增长。主键(primary key) 唯一标识，在一个表中不能重复，可以把两个表中具有相同 主键ID的数据连接起来。
> A join B, left join:保留A的全部和交集；right join保留B的所有行和交集；inner join交集；full join全部

> GROUP BY 可以对具有相同属性的行进行求和等计算
> HAVING group_condition; 对分组之后的数据再删选，用HAVING进行筛选=不用group by时简单的where

> LIMIT 和 OFFSET 子句通常和ORDER BY 语句一起使用，当我们对整个结果集排序之后，我们可以 LIMIT来指定只返回多少行结果 ,用 OFFSET来指定从哪一行开始返回。你可以想象一下从一条长绳子剪下一小段的过程，我们通过 OFFSET 指定从哪里开始剪，用 LIMIT 指定剪下多少长度。从第三位开始OFFSET=2
>


## operator关键字
AND OR
=, !=, <>, <, <=, >, >=
BETWEEN A AND B (inclusive)
NOT BETWEEN A AND B (exclusive)
IN(...) exist in a list
NOT IN(..)
模糊查询LIKE 
LIKE = "="
NOT LIKE = "!="
通配符%
LIKE "%AT%" 表示AT前后可以有任何字符
LIKE "AN_" 代表AN后有且仅有一个字符

统计函数
COUNT(*), COUNT(column)	计数！COUNT(*) 统计数据行数，COUNT(column) 统计column非NULL的行数.
MIN(column)	找column最小的一行.
MAX(column)	找column最大的一行.
AVG(column)	对column所有行取平均值.
SUM(column)	对column所有行求和.


## 例题
```
SELECT * FROM TableName;  查看全表
SELECT 1+1 可以直接做计算

-- 列出所有在Chicago西部的城市，从西到东排序(包括所有字段)
SELECT * FROM north_american_cities where Longitude<
(SELECT Longitude from north_american_cities where city="Chicago") 
ORDER BY Longitude ASC;

-- 找到所有办公室里的所有角色（包含没有雇员的）,并做唯一输出(DISTINCT) 所有的办公室都需要列出来
SELECT DISTINCT Building_name,Role FROM Buildings
LEFT JOIN employees on building_name=building;

-- 按角色(Role)统计一下每个角色的平均就职年份 
SELECT Role, AVG(Years_employed) FROM employees 
group by Role;

-- 按角色分组算出每个角色按有办公室和没办公室的统计人数(列出角色，数量，有无办公室,注意一个角色如果部分有办公室，部分没有需分开统计） 属性有：Role	Name	Building	Years_employed
SELECT COUNT(Name), Role,CASE when Building is NOT NULL 
THEN '1' ELSE '0' END AS bn
FROM Employees
GROUP BY Role,bn;

-- 按导演分组计算销售总额,求出平均销售额冠军（统计结果过滤掉只有单部电影的导演，列出导演名，总销量，电影数量，平均销量) 
SELECT Director, SUM(Domestic_sales+International_sales)as total,
count(*) as number,SUM(Domestic_sales+International_sales)/count(title) as average
FROM movies 
INNER JOIN Boxoffice on movie_id=id
group by director
HAVING COUNT(Title) > 1
order by average desc
limit 1;

-- 找出每部电影和单部电影销售冠军之间的销售差，列出电影名，销售额差额 变态难啊阿啊
SELECT Title, (SELECT MAX(Domestic_sales+International_sales) 不能是单个数值
FROM Boxoffice) - SUM(Domestic_sales+International_sales) 
AS Diff FROM Movies INNER JOIN Boxoffice 
ON Movies.ID = Boxoffice.Movie_id 
GROUP BY Title; 
```

