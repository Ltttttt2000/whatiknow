## 直播题
直播题，一个主播开播一段时间内，如果没有观众观看，主播的体验相当差，请你帮忙监控体验差的开播次数占整体的比例：
主播表 table1：date, author, live_id, broadcast_start_time
观众表 table2：date, audience, live_id, watch_start_time
求开播开始的3分钟内没有观众观看的live_id数量占比

思路：
1. 获取主播开播时间
2. 通过left join 得到对应3分钟进入直播间的观众，找到null

```sql
-- 获取主播开播时间
select author, live_id, broadcast_start_time
from table 1;

-- left join 得到表a 房间号相同进入的时间比开始的时间晚三分钟以内
select live_id from table1 t1
left join table2 t2 on t1.live_id = t2.live_id
where timestampdiff(MINUTE, t1.broadcast_start_time, t2.watch_start_time)<=3)
-- OR
--DATE_ADD(t1.broadcast_start_time,interval 3 minute) >= t2.watch_start_time

-- 找出不在表a里的id
select live_id from table1 where live_id not in a;
-- select a.live_id from a where t2.live_id is null;

-- 计算count
select count(a.live_id)/count(t1.live_id)
from
(select live_id 
from table1
where live_id not in a);
```
## 留存率
求五月份每日新设备的次日留存率  
table1 (每日活跃设备表）：p_date 日期 ，device_id 设备id，is_new_device
```sql
select a.p_date,count(b.device_id)/count(a.device_id) rate
--先找出每日新增用户表
(select device_id,p_date
from table1
where is_new_device=1) a
left join 
(
--找出活跃用户表
select device_id,p_date
from table1 t1
left join table1 t2 on t1.device_id=t2.device_id
where t1.is_new_device=1 and t2.is_new_device=0 and datediff(t2.p_date,t1.p_date)=1) b

on a.p_date=b.p_date and month（a.p_date）=5
group by a.p_date
order by a.p_date

```

每天新增用户的次日留存率
```sql

select a.dt,round(count(b.uid)/count(a.uid),2)  uv_rate
from
-- 先找出当天新增的用户表
(select uid,min(date(in_time)) dt
from tb_user_log 
group by uid ) a
left join 
-- 用户活跃表
(select uid,date(in_time) dt
 from tb_user_log 
 union
 select uid,date(out_time) dt
 from tb_user_log) b
on a.uid=b.uid and a.dt=date_sub(b.dt,INTERVAL 1 day) -- 限制b表
where date_format(a.dt,'%Y-%m') = '2021-11'
group by a.dt
order by a.dt

```

用户表Users(id, log_date),求出每个日期对应的活跃用户数、次日留存用户数、次日留存率

```sql
-- 先得到每日活跃用户数
SELECT log_date AS '日期', COUNT(DISTINCT id) AS '活跃用户数'
FROM Users
GROUP BY log_date;
-- 次日留存用户数，改变datediff的天数即可

SELECT a.log_date AS '日期', COUNT(DISTINCT b.id) AS '活跃用户数'
FROM Users a
LEFT JOIN Users b
ON a.id = b.id
AND DATEDIFF(b.log_date, a.log_date) = 1
GROUP BY a.log_date

-- 次日留存率

SELECT a.log_date AS '日期',
		COUNT(DISTINCT a.id) AS '活跃用户'
		COUNT(DISTINCT CASE WHEN DATEDIFF(b.log_date, a.log_date)=1 THEN b.id END) AS '次日活跃用户'
		CONCAT(COUNT(DISTINCT CASE WHEN DATEDIFF(b.log_date, a.log_date)=1 THEN b.id END))/COUNT(DISTINCT(a.id)*100,'%') AS '次日留存率'
FROM Users a
LEFT JOIN Users b
ON a.id = b.id
GROUP BY a.id;
```

## 分组排序-窗口函数

<窗口函数> OVER(PARTITION BY  用于分组的列名) ORDER BY 
同时具有分组和排序的功能，不减少原表的行数
原则上只能写在select语句中
专用窗口函数
```sql
RANK()：若有并列的会占用下一个名次1，1，1，4
DENSE_RANK() 有并列的不会占用下一个 1,1,2
ROW_NUMBER() 不考虑并列名次情况
聚合函数SUM, AVG, MAX, MIN, COUNT
```
TOP N 问题模版
```sql
SELECT * FROM(
SELECT * ROW_NUMBER() OVER(PARTITION BY 分组的列名 ORDER BY 排序列名) AS ranking FROM table
) AS a
WHERE ranking <=N
```

```sql
SELECT *, AVG(成绩) OVER(ORDER BY 学号 rows 2 preceding) FROM table
row 和preceding 得到记录是自身数据和之前2行的平均
```

班级表中排名（学号，班级，成绩）
**group by分组汇总后改变了表的行数，一行只有一个类别。而partiition by和rank函数不会减少原表中的行数**。
```sql
select *, rank() over (partition by 班级 order by 成绩 desc) as ranking
from 班级表
```
有成绩表（姓名，科目，成绩）获得每个学生成绩最高的两门
```sql
select * from 
(select *, row_number() over (partition by 姓名 order by 成绩 desc) as rk
from 成绩表) as a 
where rk ‹= 2;
-- 如果不用子查询，SELECT还没被执行，where里不能识别出rk
```

查找单科成绩高于该科目平均成绩的学生名单
```sql
-- 1. 找到科目的平均成绩
SELECT 科目,AVG(成绩) FROM 成绩表 GROUP BY 科目；只能返回一个数值
将每一个科目的平均值求出，用窗口函数保留了每一行的信息
select *, avg(成绩) over (partition by 科目) as avg_score from 成绩表;

-- 2. 条件where
select * from (select *, avg(成绩) over (partition by 科目) as avg_score             
from 成绩表) as b where 成绩 › avg_score;
```
学生（id，课程，分数）
```sql
select t1.student_id,course_id
from t_mark t1
where t1.mark>
	(select AVG(mark)
	 from t_mark t2
	 where t2. student_id = t1.student_id)

```

求学生总成绩的前10名 TopN问题
```sql
SELECT * FROM (
select T.*,ROW_NUMBER()OVER(PARTITION BY 班级 order by 成绩 desc) RN
FROM T
)WHERE RN<=10
```
取出所有分数在平均分以上的学生的分数记录
找出低于平均分的id和成绩，NOT IN
```sql
select id
```

找出连续七天登陆的用户
```sql
-- 第一步:数据去重,一个用户每天只保留一条登录记录 只取日期
select user_id,SUBSTRING(dt,1,10) as dt 
from log_table
group by user_id,SUBSTRING(dt,1,10);

-- 第二步:在第一步的基础上按照用户进行分组,按照登录日期进行排序
select *, row_number() over(partition by a.user_id order by a.dt) as rn
from 
(
select user_id,SUBSTRING(dt,1,10) as dt from log_table group by user_id,SUBSTRING(dt,1,10)
) a

-- 第三步:在第二步的基础上,增加一个计算字段:dt-rn 当前时间-rn天（连续天数是相同的）
select *, DATE_SUB(dt,INTERVAL rn DAY) as base_dt
from 
(
select *,row_number() over(partition by a.user_id order by a.dt) as rn
	from 
	(select user_id,SUBSTRING(dt,1,10) as dt from log_table group by user_id,SUBSTRING(dt,1,10)
	) a
) b
-- 第四步:在第三步的基础上,按照user_id分组,对dt2进行计数,数量>=7的结果对应的用户ID,即为我们想得到的结果
select user_id, base_dt, count(1)
from
(
select *, DATE_SUB(dt,INTERVAL rn DAY) as base_dt
from 
(
select *, row_number() over(partition by a.user_id order by a.dt) as rn
from 
	(
	select user_id,SUBSTRING(dt,1,10) as dt from log_table group by user_id,SUBSTRING(dt,1,10)
	) a
) b

) c
group by user_id,base_dt HAVING COUNT(1)>=7

-- 连续登陆七天的人数，再在外面嵌套一层
select count(distinct user_id)
```

**怎么求连续七天工作日登陆的用户？**







### 满意率服务量窗口函数
table1:客服id，接打的客户电话号码num，电话的开始时间start_time，电话时长length，客户满意度（满意or不满意）sat 1/0  
求每个客服在这段时间内的服务量，每个客服的满意率，找出每个客服的第一个不满意的电话号码
```sql
# 1. Table_a: 每个id的服务量，只需group by然后count就行
SElECT id, COUNT(num) AS total_num
FROM table1
GROUP BY id;

# 2. Table_b: 满意率 再count出满意的
SElECT id, COUNT(num) AS sat_num
FROM table1
WHERE sta = 1
GROUP BY id;

# 3. Table_c 联合表a, b 算出每个ID的满意率
SELECT c.id, a.total_num AS 服务量, c.sat_num/c.total_num AS 满意率
FROM a, b
WHERE a.id = b.id

# 4. Table_e: 第一个不满意的需要对时间排序 分组排序，用窗口函数
SELECT d.id, d.num AS first_0 FROM(
SELECT id, num, RANK() OVER(PARTITION BY id ORDER BY start_time) AS rk
FROM table1
WHERE sta = 0)d
WHERE d.rk = 1;

# 5. 联合两个结果
SELECT c.id, c.服务量, c.满意率, e.first_0
FROM c, e
WHERE c.id = e.id
```

### 排序分类问题

求每门课前3名的记录
成绩表 table1：course_name，student_name，score

```sql
select a.course_name,a.student_name,a.score
from
(select course_name,student_name,score,
dense_rank() over (partition by course_name order by score desc) RN  
FROM table1 ) a
where a.RN between 1 and 3

```

将作者按照粉丝层级划分成1-10W，10W-100W，100W+三类，统计每类作者数 以及 每类作者5月份的人均活跃天数 
全量作者表table1: author_id, fans_count； 
每日作者活跃表 table2：p_date, author_id
```sql
# 表A：类型 + 每个类型作者数
select distinct a.type, count(*) as total_num
from 
(
 select author_id, 
 (
  case 
   when fans_count < 100000 then 0
   when fans_count < 1000000 then 1
   else 2
  end
 ) AS type %% 用不同数字表示不同的type %%
 from table1
) a %% id对应类型 %% 
group by a.type


%% 表B：类型 + 五月人均活跃天数 %%

select b.type, avg(c.alive_days)
from 
(
 select author_id, 
 (
  case 
   when fans_count < 100000 then 0
   when fans_count < 1000000 then 1
   else 2
  end
 ) type
 from table1
) b,  // id对应类型
(
 select author_id, count(*) as alive_days
 from table2 
 where month(p_date) = 5
 group by author_id
) c  // 每个作者五月份活跃天数

where b.author_id = c.author_id
group by b.type
最后再把表A和表B按照type连起来即可
```
另一个写法
```sql
--统计每类人数
select a.type,count(a.author_id) as num
from
--作者分好类
(select author_id,
case when fans_count>1 and fans_count<=100000 then '1~10w'
when fans_count<=1000000 then '10w~100w'
else '100w+' end as type
from table1 ) a

group by a.type


select b.type,avg(count(p_date))
from
(select author_id,
case when fans_count>1 and fans_count<=100000 then '1~10w'
when fans_count<=1000000 then '10w~100w'
else '100w+' end as type
from table1 ) b

left join table t2 
on b.author_id=t2.author_id
where month(p_date)=5
group by b.type

```