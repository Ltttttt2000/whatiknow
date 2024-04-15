OVER()窗口函数
OVER用于为行定义一个窗口，它对一组值进行操作，不需要使用GROUP BY子句对数据进行分组，能够在同一行中同时返回基础行的列和聚合列。

OVER ( [ PARTITION BY column ] [ ORDER BY column ] )
PARTITION BY 子句进行分组；
ORDER BY 子句进行排序。
OVER开窗函数必须与[聚合函数](https://so.csdn.net/so/search?q=%E8%81%9A%E5%90%88%E5%87%BD%E6%95%B0&spm=1001.2101.3001.7020)或排序函数一起使用
聚合函数：SUM(),MAX(),MIN,COUNT(),AVG()
排序函数：RANK(), ROW_NUMBER(),DENSE_RANK(),NTILE()

Leetcode SQL 50题
## 分类
### 普通查询

!= 
IS NULL
length(content) > 15

### 连接
left join, right join, full join, inner join (join)
CROSS JOIN 需要把两个表的每一行都一一合并，在MySQL中，当CROSS JOIN不使用WHERE子句时，CROSS JOIN产生了一个结果集，该结果集是两个关联表的行的乘积。通常，如果每个表分别具有n和m行，则结果集将具有n*m行,cross join的时候是不需要on或者using关键字的，这个是区别于inner join和join的
```sql
Datadiff(d1, d2)
To_DAYS(date)
Subdate(date,1) -- 日期-1

```
1161. 每台机器平均运行时间
通过不同activity_type的timestap相减获得每个process_id, machine_id的时间，用两个表中相同的相减。
算COUNT和SUM计算平均
ROUND(date,3)保留三位小数

577. 员工奖金
一般很少用full join，注意NULL

1280. 参加各科测试的次数
三个表项链，要保证所有student和所有subject都在。
非必要慎用cross_join，没有主键，可能会有重复行

570. 至少有5名直接下属的经理
id, name, department, managerID

1934. 确认率
2356. 每位教师所教的科目种类GROUP BY COUNT(DISTINCT subjectid)

1141. 近30天活跃用户

1084. 仅在某个时间段

# 聚合函数
1633. 各赛事的用户注册率
```sql
-- 将子查询放在查询上
SELECT contest_id, ROUND(COUNT(user_id)*1.0 /(SELECT COUNT(user_id) AS total FROM Users) * 100,2) AS percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage DESC, contest_id;;
```

1211. 查询结果的数量比
```
Queries 表：(query_name, result, position, rating) 
此表可能有重复的行。
此表包含了一些从数据库中收集的查询信息。
“位置”（`position`）列的值为 **1** 到 **500** 。
“评分”（`rating`）列的值为 **1** 到 **5** 。评分小于 3 的查询被定义为质量很差的查询。
将查询结果的质量 `quality` 定义为：
>各查询结果的评分与其位置之间比率的平均值。

将劣质查询百分比 `poor_query_percentage` 为：
评分小于 3 的查询结果占全部查询结果的百分比。

编写解决方案，找出每次的 `query_name` 、 `quality` 和 `poor_query_percentage`。

`quality` 和 `poor_query_percentage` 都应 **四舍五入到小数点后两位** 。
```
# 高级查询和连接

610. 判断三角形 CASE的用法
```sql
SELECT
x, y, z,
CASE
	WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes'
	ELSE 'No'
END AS 'triangle'
FROM Triangle;
```

1907. 按分类统计薪水
```sql
SELECT 'Low Salary' AS category, SUM(IF(income<20000,1,0)) AS accounts_count
FROM Accounts
UNION
SELECT 'Average Salary' AS category, SUM(IF(income>=20000 AND income<=50000,1,0)) AS accounts_count
FROM Accounts
UNION
SELECT 'High Salary' AS category, SUM(IF(income>50000,1,0)) AS accounts_count
FROM Accounts

IF(condition, if_condition_true, else)

CASE 
	WHEN condition1 THEN result1
	WHEN condition2 THEN result2
	ELSE resultN
END
```

1204. 最后一个能进入巴士的人
```sql
SELECT a.person_name
FROM Queue a, Queue b
WHERE a.turn >= b.turn
GROUP BY a.person_id HAVING SUM(b.weight) <= 1000
%% group by里是a包括自己的前面的所有人 %%
ORDER BY a.turn DESC
LIMIT 1
```


1164. 指定日期的产品价格
```sql
IFNULL(value, 10) 如果value是NULL则返回10
```

550. 时间的算法
```sql
DATE_ADD(MIN(event_date), INTERVAL 1 DAY) 比最小的天数多一天
```
# 子查询
1978. 上级经理已离职的公司员工
```sql
表: `Employees`

+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| manager_id  | int      |
| salary      | int      |
+-------------+----------+
在 SQL 中，employee_id 是这个表的主键。
这个表包含了员工，他们的薪水和上级经理的id。
有一些员工没有上级经理（其 manager_id 是空值）。

查找这些员工的id，他们的薪水严格少于`$30000` 并且他们的上级经理已离职。当一个经理离开公司时，他们的信息需要从员工表中删除掉，但是表中的员工的`manager_id`  这一列还是设置的离职经理的id。

返回的结果按照`employee_id` 从小到大排序。
```

```sql
-- 解析：manager_id不能在表中找到对应的employee_id
SELECT employee_id FROM Employees
WHERE manager_id NOT IN (SELECT DISTINCT employee_id FROM Employees)
AND salary < 30000
ORDER BY employee_id;
```

626. 换座位
```sql
表 Seat
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| student     | varchar |
+-------------+---------+
`id` 是该表的主键（唯一值）列。
该表的每一行都表示学生的姓名和 ID。
id 是一个连续的增量。

编写解决方案来交换每两个连续的学生的座位号。如果学生的数量是奇数，则最后一个学生的id不交换。
按 `id` 升序 返回结果表。
```

```sql
1. 三个子表的联合UNION（有去重） UNION ALL不去重
(SELECT (id-1) AS id, student
FROM Seat
WHERE id%2=0)

UNION

SELECT (id+1) as id, student
FROM Seat
WHERE id%2=1 AND id NOT IN (SELECT MAX(id) FROM Seat)

UNION

(SELECT id, student
FROM Seat
WHERE id IN (SELECT MAX(id) FROM Seat) AND id%2=1)

ORDER BY id;

2. 用CASE和MOD
先找出最大的id用counts表示
SELECT 
	(CASE WHEN MOD(id, 2) != 0 AND counts != id THEN id + 1 
		  WHEN MOD(id, 2) != 0 AND counts = id THEN id 
		  ELSE id - 1 
	 END) AS id, student 
FROM seat, (SELECT COUNT(*) AS counts FROM seat) AS seat_counts 
ORDER BY id ASC; 

```

1341. 电影评分
	 `Movie (movie_id, title)`
	 `Users (user_id, name)`
	 `MovingRating (movie_id, user_id, rating, created_at)`
	 created_at 是时间类型
	 查找评论电影数量最多的用户名。如果出现平局，返回字典序较小的用户名。
	 查找在 `February 2020` **平均评分最高** 的电影名称。如果出现平局，返回字典序较小的电影名称。
	 **字典序** ，即按字母在字典中出现顺序对字符串排序，字典序较小则意味着排序靠前。
```sql
分别求解再合并，注意UNION ALL
ORDER BY CONVERT(title USING gbk) 字典序排序

SELECT a.name AS results FROM (
SELECT name, COUNT(rating) as number
FROM Movies m, Users u, MovieRating mr
WHERE m.movie_id = mr.movie_id AND u.user_id = mr.user_id
GROUP BY u.user_id
ORDER BY number DESC, CONVERT(name USING gbk)
LIMIT 1)a

UNION ALL

SELECT b.title AS results FROM (
SELECT title, AVG(rating) as average
FROM Movies m, MovieRating mr
WHERE m.movie_id = mr.movie_id
AND created_at BETWEEN '2020-02-01' AND '2020-02-29'
GROUP BY title
ORDER BY average DESC, CONVERT(title USING gbk)
LIMIT 1)b;
```

1321. 餐馆营业额变化增长
```sql
找两个日期间的条件
SELECT C1.visited_on, SUM(C2.amount) AS amount, ROUND(SUM(C2.amount) / 7, 2) AS average_amount
FROM (SELECT DISTINCT visited_on FROM Customer) AS C1
         LEFT JOIN
     Customer AS C2 
     ON DATEDIFF(C1.visited_on, C2.visited_on) BETWEEN 0 AND 6
WHERE C1.visited_on >= (SELECT MIN(visited_on) FROM Customer) + 6
GROUP BY C1.visited_on
ORDER BY C1.visited_on;


DATEDIFF如果前面的小会有负值的 
```

602. 好友申请 II ：谁有最多的好友
```sql
`RequestAccepted` 表：

+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| requester_id   | int     |
| accepter_id    | int     |
| accept_date    | date    |
+----------------+---------+
(requester_id, accepter_id) 是这张表的主键(具有唯一值的列的组合)。
这张表包含发送好友请求的人的 ID ，接收好友请求的人的 ID ，以及好友请求通过的日期。
编写解决方案，找出拥有最多的好友的人和他拥有的好友数目。
生成的测试用例保证拥有最多好友数目的只有 1 个人。
```
```sql
SELECT a.id, COUNT(a.id) as num
FROM
(SELECT requester_id as id FROM RequestAccepted
UNION ALL
SELECT accepter_id as id FROM RequestAccepted)a
GROUP BY a.id
ORDER BY num DESC LIMIT 0,1;  %% offset=0, limit=1 %%
```

585. 2016年的投资
```sql
`Insurance` 表：

+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| pid         | int   |
| tiv_2015    | float |
| tiv_2016    | float |
| lat         | float |
| lon         | float |
+-------------+-------+
pid 是这张表的主键(具有唯一值的列)。表中的每一行都包含一条保险信息，其中：
pid 是投保人的投保编号。
tiv_2015 是该投保人在 2015 年的总投保金额，tiv_2016 是该投保人在 2016 年的总投保金额。
lat 是投保人所在城市的纬度。题目数据确保 lat 不为空。
lon 是投保人所在城市的经度。题目数据确保 lon 不为空。

编写解决方案报告 2016 年 (`tiv_2016`) 所有满足下述条件的投保人的投保金额之和：

- 他在 2015 年的投保额 (`tiv_2015`) 至少跟一个其他投保人在 2015 年的投保额相同。
- 他所在的城市必须与其他投保人都不同（也就是说 (`lat, lon`) 不能跟其他任何一个投保人完全相同）。

`tiv_2016` 四舍五入的 **两位小数** 。
```
```sql
SELECT ROUND(SUM(tiv_2016),2) AS tiv_2016
FROM Insurance
WHERE tiv_2015 IN
(SELECT tiv_2015 FROM Insurance
GROUP BY tiv_2015
HAVING COUNT(tiv_2015) > 1)
AND lat IN
(SELECT lat FROM Insurance
GROUP BY lat, lon
HAVING COUNT(*) = 1);
```

185. 困难！部门工资前三高的所有员工
```
表: `Employee (id, name, salary, departmentId)`
表：`Department (id, name)`
员工的工资在该部门不同的工资中排名前三的员工
```

```sql
-- 前三意味着有不超过三个人的薪水比它高
select e1.Name as 'Employee', e1.Salary 
from Employee e1 
where 3 > ( 
	select count(distinct e2.Salary) 
	from Employee e2 
	where e2.Salary > e1.Salary );

-- 用窗口函数
SELECT d.name AS Department, b.name AS Employee, b.salary AS salary
FROM (
	SELECT a.id, a.name, a.departmentId, a.salary, a.rk FROM(
		SELECT id, name, departmentId, salary, DENSE_RANK() over(PARTITION BY departmentId ORDER BY salary DESC) as rk
		FROM Employee) a
		WHERE rk < 4) b
JOIN
Department d
ON b.departmentId = d.id;
```
# 高级字符串函数/正则表达式/子句
1167. 修复表中的名字，将第一个字母大写其余都小写
```sql
SELECT user_id, concat(upper(left(name, 1)), lower(substring(name, 2))) as name
FROM Users
ORDER BY user_id;
```
concat:
upper:
left
substring
ORDER BY column ASC/DESC


1517. 查询有效邮箱的账户
```sql
REGEXP '' 正则表达式
^ 表示开头，匹配以该字符后面的字符开头的字符串
+ 匹配一个或多个,不包括空
[] 表示集合里的任意一个 [a-zA-Z]小写大写
* 一个量词，表示匹配前面的元素零次或多次。
\\ 用于转义特殊字符
a{m,n} 匹配m到n个a，左侧不写为0，右侧不写为任意
$ 表示以什么为结尾
\w 匹配字母数字字符包括下划线

正则写法
^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\\.com$
^[a-zA-Z]+[\\w/.\\-]*(@leetcode.com)$

SELECT * FROM Users
WHERE mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode[.]com$'
```

1527. 患某种疾病的患者
```
模糊查询
SELECT * FROM Patients
WHERE conditions LIKE '% DIAB1%' OR conditions LIKE 'DIAB1%';

正则表达式
SELECT patient_id, patient_name, conditions FROM Patients 
WHERE conditions REGEXP '\\bDIAB1.*'; 


.* 任意字符
\b 是一个特殊的元字符，表示单词边界。它匹配一个单词的开始或结束的位置，而不匹配任何实际的字符
\B 是 \b 的反义符号，它表示非单词边界，即匹配不在单词边界处的位置。
```
196. 删除重复的电子邮箱
```
首先Person表与自身在Email列中连接起来，然后需要找到具有相同Email地址的更大ID，这就是我们要删除的记录。
DELETE p1
FROM Person p1, Person p2
WHERE p1.email = p2.email and p1.id > p2.id;
```
176. 第二高的薪水
```sql
SELECT
(SELECT DISTINCT Salary
FROM Employee
ORDER BY Salary DESC LIMIT 1 OFFSET 1)
AS SecondHighestSalary;

LIMIT  查几个值
OFFSET 0 从第一个开始
SELECT()保证如果没有返回null
```
1484.  按日期分组销售产品
```
编写解决方案找出每个日期、销售的不同产品的数量及其名称。
Activities(sell_date, product)

GROUP_CONCAT 分组后进行拼接

SELECT sell_date, COUNT(DISTINCT product) num_sold, group_concat(distinct product) products
FROM Activities
GROUP BY sell_date;
```
1327. 列出指定时间段内所有的下单产品
```
Products
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| product_id       | int     |
| product_name     | varchar |
| product_category | varchar |
+------------------+---------+
product_id 是该表主键(具有唯一值的列)。
该表包含该公司产品的数据。

表: Orders

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| order_date    | date    |
| unit          | int     |
+---------------+---------+
该表可能包含重复行。
product_id 是表单 Products 的外键（reference 列）。
unit 是在日期 order_date 内下单产品的数目。

要求获取在 2020 年 2 月份下单的数量不少于 100 的产品的名字和数目。
```

```sql
SELECT product_name, SUM(unit) AS unit
FROM Products p, Orders o
WHERE p.product_id = o.product_id AND order_date BETWEEN '2020-02-01' AND '2020-02-29'
GROUP BY product_name
HAVING unit >= 100;  %% 直接用alis就行 %%
```