# 数据库面试题

## 数据库基本操作

### mysql数据库的使用

```bash
sudo apt-get install mysql-server # 安装mysql服务端
sudo server mysql status # 查看mysql服务状态
sudo server mysql stop # 停止mysql服务
sudo server mysql start # 启动mysql服务
sudo server mysql restart # 重启mysql服务
sudo apt-get install mysql-client # 安装mysql客户端
mysql --help # 查看mysql命令的使用帮助
mysql -u用户名 -p密码 # mysql客户端连接服务端命令
mysql -u用户名 -p密码 数据库名称<name.sql # 导入sql文件
quit/exit/ctrl+d # 退出
```

### mysql数据类型和约束

数据类型和约束保证了表中数据的准确性和完整性

#### 数据类型

创建表时为表中字段指定的数据类型，使用原则：尽量选取值范围小的，节省空间

* **整形类型**

| 类型 | 字节大小 | 有符号范围\(Signed\) | 无符号范围\(Unsigned\) |
| :--- | :--- | :--- | :--- |
| TINYINT | 1 | -128 ~ 127 | 0 ~ 255 |
| SMALLINT | 2 | -32768 ~ 32767 | 0 ~ 65535 |
| MEDIUMINT | 3 | -8388608 ~ 8388607 | 0 ~ 16777215 |
| INT/INTEGER | 4 | -2147483648 ~2147483647 | 0 ~ 4294967295 |
| BIGINT | 8 | -9223372036854775808 ~ 9223372036854775807 | 0 ~ 18446744073709551615 |

* **小数类型**：decimal（浮点数），decimal\(5, 2\) 共存5位数，小数占 2 位
* **字符串**：varchar（可变字符串）char（固定长度字符串）char\(3\)，存'ab'为'ab '

| 类型 | 说明 | 使用场景 |
| :--- | :--- | :--- |
| CHAR | 固定长度，小型数据 | 身份证号、手机号、电话、密码 |
| VARCHAR | 可变长度，小型数据 | 姓名、地址、品牌、型号 |
| TEXT | 可变长度，字符个数大于 4000 | 存储小型文章或者新闻 |
| LONGTEXT | 可变长度， 极大型文本数据 | 存储极大型文本数据 |

* **时间类型**

| 类型 | 字节大小 | 示例 |
| :--- | :--- | :--- |
| DATE | 4 | '2020-01-01' |
| TIME | 3 | '12:29:59' |
| DATETIME | 8 | '2020-01-01 12:29:59' |
| YEAR | 1 | '2017' |
| TIMESTAMP | 4 | '1970-01-01 00:00:01' UTC ~ '2038-01-01 00:00:01' UTC |

#### 约束

在数据类型限定的基础上额外增加的要求

| 约束 | constraint | 介绍 |
| :--- | :--- | :--- |
| 主键 | primary key | 物理上的存储的 |
| 非空 | not null | 不许为空 |
| 唯一 | unique | 不许重复 |
| 默认 | default | 不填写时使用默认值 |
| 外键约束 | foreign key | 对关系字段进行约束, 关联的表中查询值是否存在, |

### sql语句基本使用

* **数据库操作**

```sql
select now(); # 查看当前时间
show databases; # 查看所有数据库
create database 数据库名 charset=utf8; # 创建数据库
use 数据库名; # 使用数据库
select database(); # 查看当前使用的数据库
drop database 数据库名; # 删除数据库
source areas.sql; # 执行sql文件给areas表导入数据
```

* **表结构操作**

```sql
show tables; # 查看当前数据库中所有表

create table 表名(
字段名称 数据类型  可选的约束条件,
);
create table students(
 id int unsigned primary key auto_increment not null,
 name varchar(20) not null,
 age tinyint unsigned default 0,
 height decimal(5,2),
 gender enum('男','女','人妖','保密')
);

alter table 表名 add 列名 类型 约束; # 修改表-添加字段
alter table students add birthday datetime;

alter table 表名 modify 列名 类型 约束; # 修改表-修改字段类型
alter table students modify birthday date not null;

alter table 表名 change 原名 新名 类型及约束; # 修改表-修改字段名和字段类型
alter table students change birthday birth datetime not null;

alter table 表名 drop 列名; # 修改表-删除字段
alter table students drop birthday;

show create table 表名; # 查看创表SQL语句
show create table students;

show create database 数据库名; # 查看创库SQL语句
show create database mytest;

drop table 表名; # 删除表
drop table students;
```

* **表数据操作**

```sql
select * from 表名; # 查询所有列
select * from students;

select 列1,列2,... from 表名; # 查询指定列
select id,name from students;

insert into 表名 values (...) # 全列插入：值的顺序与表结构字段的顺序完全一一对应
insert into students values(0, 'xx', default, default, '男');

insert into 表名 values (...),(...)...; # 全列多行插入
insert into students values(0, '张飞', 55, 1.75, '男'),(0, '关羽', 58, 1.85, '男');

insert into 表名(列1,...) values(值1,...) # 部分列插入：值的顺序与给出的列顺序对应
insert into students(name, age) values('王二小', 15);

insert into 表名(列1,...) values(值1,...),(值1,...)...; # 部分列多行插入
insert into students(name, height) values('刘备', 1.75),('曹操', 1.6);
-- 主键列是自动增长，但是在全列插入时需要占位，通常使用空值(0或者null或者default)
-- 在全列插入时，如果字段列有默认值可以使用 default 来占位，插入后的数据就是之前设置的默认值

update 表名 set 列1=值1,列2=值2... where 条件 # 修改数据
update students set age = 18, gender = '女' where id = 6;

delete from 表名 where 条件 # 删除数据
delete from students where id=5;
```

* **表查询操作**

```sql
-- 使用 as 给字段起别名，可以通过 as 给表起别名
select id as 序号, name as 名字, gender as 性别 from students;
select s.id,s.name,s.gender from students as s;

-- distinct可以去除重复数据行
select distinct 列1,... from 表名;
select distinct name, gender from students; # 看到了很多重复数据 想要对其中重复数据行进行去重操作可以使用 distinct

-- where条件查询
-- 比较运算符：= > >= < <= != <>
select * from students where id > 3;
-- 逻辑运算符：and or not 多个条件结合()
select * from students where not (age >= 10 and age <= 15);
-- 模糊查询：like是模糊查询关键字/%表示任意多个任意字符/_表示一个任意字符
select * from students where name like '黄%' or name like '_靖';
-- 范围查询：between .. and .. 表示在一个连续的范围内查询/in 表示在一个非连续的范围内查询
select * from students where id between 3 and 8;
select * from students where id in [1, 2, 3, 4];
-- 空判断：判断为空使用: is null/判断非空使用: is not null
select * from students where height is null;

-- asc升序查询和desc降序查询
select * from 表名 order by 列1 asc|desc [,列2 asc|desc,...]
select * from students order by age desc,height desc; # 先按照年龄从大->小排序，年龄相同时按身高从高->矮排序

-- limit 关键字实现分页查询 start表示开始行索引，默认是0 count表示查询条数
select * from 表名 limit start,count
select * from students where gender=1 limit 0,3;
select * from students limit (n-1)*m,m # 每页显示m条数据，求第n页显示的数据

-- 聚合函数 可结合分组(group by)使用，用于统计和计算分组数据
count(col): 表示求指定列的总行数
max(col): 表示求指定列的最大值
min(col): 表示求指定列的最小值
sum(col): 表示求指定列的和
avg(col): 表示求指定列的平均值
select avg(height) from students where gender = 1;
select avg(ifnull(height,0)) from students where gender = 1;

-- 分组查询按照指定字段分组，数据相等的分为一组
-- GROUP BY 列名 [HAVING 条件表达式] [WITH ROLLUP]
-- group by的使用 
select name, gender from students group by name, gender; # 根据name和gender字段进行分组
-- group by + group_concat()的使用 
select gender,group_concat(name) from students group by gender; # group_concat(字段名): 统计每个分组指定字段的信息集合，每个信息之间使用逗号进行分割
-- group by + 聚合函数的使用
select gender,count(*) from students group by gender; # 统计不同性别的人的个数
-- group by + having的使用
select gender,count(*) from students group by gender having count(*)>2; # 根据gender字段进行分组，统计分组条数大于2的
-- group by + with rollup的使用
select gender,group_concat(age) from students group by gender with rollup; # 根据gender字段进行分组，汇总所有人的年龄

-- 内连接查询 左连接查询 右连接查询 自连接查询  on就是连接查询条件
-- 查询两个表中符合条件的共有记录
select 字段 from 表1 inner join 表2 on 表1.字段1 = 表2.字段2
select * from students as s inner join classes as c on s.cls_id = c.id;
-- 以左表为主根据条件查询右表数据，如果根据条件查询右表数据不存在使用null值填充
select 字段 from 表1 left join 表2 on 表1.字段1 = 表2.字段2
-- 以右表为主根据条件查询左表数据，如果根据条件查询左表数据不存在使用null值填充
select 字段 from 表1 right join 表2 on 表1.字段1 = 表2.字段2
-- 自连接查询就是把一张表模拟成左右两张表，然后进行连表查询
select c.id, c.title, c.pid, p.title from areas as c inner join areas as p on c.pid = p.id where p.title = '山西省';
-- 一个select语句嵌入另外一个select语句, 被嵌入select语句为子查询语句，外部select语句为主查询
select * from students where age > (select avg(age) from students); # 查询大于平均年龄的学生
select name from classes where id in (select cls_id from students where cls_id is not null); # 查询学生在班的所有班级名字
select * from students where (age, height) =  (select max(age), max(height) from students); # 查找年龄最大,身高最高的学生
```

![](../.gitbook/assets/image%20%2837%29.png)

![](../.gitbook/assets/image%20%2818%29.png)

![](../.gitbook/assets/image.png)

## Mysql 练习题 <a id="mysql-&#x7EC3;&#x4E60;&#x9898;"></a>

**我使用的Mysql版本是5.7.19。答案可能会因版本会有少许出入**。

### 练习数据 <a id="&#x7EC3;&#x4E60;&#x6570;&#x636E;"></a>

数据表 --1.学生表 Student\(SId,Sname,Sage,Ssex\)

--SId 学生编号,Sname 学生姓名,Sage 出生年月,Ssex 学生性别

--2.课程表 Course\(CId,Cname,TId\) --CId --课程编号,Cname 课程名称,TId 教师编号

--3.教师表 Teacher\(TId,Tname\) --TId 教师编号,Tname 教师姓名

--4.成绩表 SC\(SId,CId,score\) --SId 学生编号,CId 课程编号,score 分数

创建测试数据

学生表 Student

```text
create table Student(Sid varchar(10),Sname varchar(10),Sage datetime,Ssex varchar(10));
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
insert into Student values('02' , '钱电' , '1990-12-21' , '男');
insert into Student values('03' , '孙风' , '1990-05-20' , '男');
insert into Student values('04' , '李云' , '1990-08-06' , '男');
insert into Student values('05' , '周梅' , '1991-12-01' , '女');
insert into Student values('06' , '吴兰' , '1992-03-01' , '女');
insert into Student values('07' , '郑竹' , '1989-07-01' , '女');
insert into Student values('09' , '张三' , '2017-12-20' , '女');
insert into Student values('10' , '李四' , '2017-12-25' , '女');
insert into Student values('11' , '李四' , '2017-12-30' , '女');
insert into Student values('12' , '赵六' , '2017-01-01' , '女');
insert into Student values('13' , '孙七' , '2018-01-01' , '女');
```

科目表 Course

```text
create table Course(Cid varchar(10),Cname varchar(10),Tid varchar(10));
insert into Course values('01' , '语文' , '02');
insert into Course values('02' , '数学' , '01');
insert into Course values('03' , '英语' , '03');
```

教师表 Teacher

```text
create table Teacher(Tid varchar(10),Tname varchar(10));
insert into Teacher values('01' , '张三');
insert into Teacher values('02' , '李四');
insert into Teacher values('03' , '王五');
```

成绩表 SC

```text
create table sc(Sid varchar(10),Cid varchar(10),score decimal(18,1));
insert into sc values('01' , '01' , 80);
insert into sc values('01' , '02' , 90);
insert into sc values('01' , '03' , 99);
insert into sc values('02' , '01' , 70);
insert into sc values('02' , '02' , 60);
insert into sc values('02' , '03' , 80);
insert into sc values('03' , '01' , 80);
insert into sc values('03' , '02' , 80);
insert into sc values('03' , '03' , 80);
insert into sc values('04' , '01' , 50);
insert into sc values('04' , '02' , 30);
insert into sc values('04' , '03' , 20);
insert into sc values('05' , '01' , 76);
insert into sc values('05' , '02' , 87);
insert into sc values('06' , '01' , 31);
insert into sc values('06' , '03' , 34);
insert into sc values('07' , '02' , 89);
insert into sc values('07' , '03' , 98);
```

### 练习题目 <a id="&#x7EC3;&#x4E60;&#x9898;&#x76EE;"></a>

1. 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数 1.1 查询同时存在" 01 "课程和" 02 "课程的情况 1.2 查询存在" 01 "课程但可能不存在" 02 "课程的情况\(不存在时显示为 null \) 1.3 查询不存在" 01 "课程但存在" 02 "课程的情况
2. 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩
3. 查询在 SC 表存在成绩的学生信息
4. 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩\(没成绩的显示为 null \) 4.1 查有成绩的学生信息
5. 查询「李」姓老师的数量sho, 
6. 查询学过「张三」老师授课的同学的信息
7. 查询没有学全所有课程的同学的信息
8. 查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息
9. 查询和" 01 "号的同学学习的课程 完全相同的其他同学的信息
10. 查询没学过"张三"老师讲授的任一门课程的学生姓名
11. 查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩
12. 检索" 01 "课程分数小于 60，按分数降序排列的学生信息
13. 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩
14. 查询各科成绩最高分、最低分和平均分： 以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率 及格为&gt;=60，中等为：70-80，优良为：80-90，优秀为：&gt;=90 要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列
15. 按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺 15.1 按各科成绩进行排序，并显示排名， Score 重复时合并名次
16. 查询学生的总成绩，并进行排名，总分重复时保留名次空缺 16.1 查询学生的总成绩，并进行排名，总分重复时不保留名次空缺
17. 统计各科成绩各分数段人数：课程编号，课程名称，\[100-85\]，\[85-70\]，\[70-60\]，\[60-0\] 及所占百分比
18. 查询各科成绩前三名的记录
19. 查询每门课程被选修的学生数
20. 查询出只选修两门课程的学生学号和姓名
21. 查询男生、女生人数
22. 查询名字中含有「风」字的学生信息
23. 查询同名同性学生名单，并统计同名人数
24. 查询 1990 年出生的学生名单
25. 查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列
26. 查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩
27. 查询课程名称为「数学」，且分数低于 60 的学生姓名和分数
28. 查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）
29. 查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数
30. 查询不及格的课程
31. 查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名
32. 求每门课程的学生人数
33. 成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩
34. 成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩
35. 查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩
36. 查询每门功成绩最好的前两名
37. 统计每门课程的学生选修人数（超过 5 人的课程才统计）。
38. 检索至少选修两门课程的学生学号
39. 查询选修了全部课程的学生信息
40. 查询各学生的年龄，只按年份来算
41. 按照出生日期来算，当前月日 &lt; 出生年月的月日则，年龄减一
42. 查询本周过生日的学生
43. 查询下周过生日的学生
44. 查询本月过生日的学生
45. 查询下月过生日的学生

### 参考答案 <a id="&#x53C2;&#x8003;&#x7B54;&#x6848;"></a>

1.查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数

```text
select *
from (select SId ,score from sc where sc.CId='01')as t1 , (select SId ,score from sc where sc.CId='02') as t2
where t1.SId=t2.SId
and   t1.score>t2.score
```

1.1 查询同时存在" 01 "课程和" 02 "课程的情况

```text
select *
from (select SId ,score from sc where sc.CId='01')as t1 , (select SId ,score from sc where sc.CId='02') as t2
where t1.SId=t2.SId
```

1.2 查询存在" 01 "课程但可能不存在" 02 "课程的情况\(不存在时显示为 null \)

```text
select *
from (select SId ,score from sc where sc.CId='01')as t1 left join (select SId ,score from sc where sc.CId='02') as t2
on t1.SId=t2.SId
```

1.3 查询不存在" 01 "课程但存在" 02 "课程的情况

```text
select *
from sc
where sc.SId not in (select SId from sc where sc.CId='01')
and  sc.CId='02'
```

1. 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩

```text
select Student.*,t1.avgscore
from Student inner JOIN(
select sc.Sid ,AVG(sc.score)as avgscore
from sc 
GROUP BY sc.Sid
HAVING AVG(sc.score)>=60)as t1 on Student.Sid=t1.Sid;
```

1. 查询在 SC 表存在成绩的学生信息

```text
select DISTINCT student.*
from student ,sc
where student.SId=sc.SId
```

4.查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩\(没成绩的显示为null\)

```text
select student.SId,student.Sname,t1.sumscore,t1.coursecount
from student ,(
select SC.SId,sum(sc.score)as sumscore ,count(sc.CId) as coursecount
from sc 
GROUP BY sc.SId) as t1
where student.SId =t1.SId
```

4.1 查有成绩的学生信息

```text
select *
from student
where EXISTS(select * from sc where student.SId=sc.SId)
```

1. 查询「李」姓老师的数量

```text
select count(*)
from teacher
where teacher.Tname like '李%
```

1. 查询学过「张三」老师授课的同学的信息

```text
select student.*
from teacher  ,course  ,student,sc
where teacher.Tname='张三'
and   teacher.TId=course.TId
and   course.CId=sc.CId
and   sc.SId=student.SId
```

1. 查询没有学全所有课程的同学的信息

* 解法1

```text
select student.*
from sc ,student
where sc.SId=student.SId
GROUP BY sc.SId
Having count(*)<(select count(*) from course)
```

**但这种解法得出来的结果不包括什么课都没选的同学。**

* 解法2

```text
select DISTINCT student.*
from 
(select student.SId,course.CId
from student,course ) as t1 LEFT JOIN (SELECT sc.SId,sc.CId from sc)as t2 on t1.SId=t2.SId and t1.CId=t2.CId,student
where t2.SId is null
and   t1.SId=student.SId
```

**利用笛卡尔积可以把什么课都没选的同学查询出来**

1. 查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息

```text
select DISTINCT student.*
from  sc ,student
where sc.CId in (select CId from sc where sc.SId='01')
and   sc.SId=student.SId
```

9.查询和" 01 "号的同学学习的课程完全相同的其他同学的信息

```text
select DISTINCT student.*
from (
select student.SId,t.CId
from student ,(select sc.CId from sc where sc.SId='01') as t) as t1 LEFT JOIN sc on t1.SId=sc.SId and t1.CId=sc.CId,student
where sc.SId is null 
and   t1.SId=student.SId
```

10.查询没学过"张三"老师讲授的任一门课程的学生姓名

```text
select *
from student 
where student.SId not in 
(
select student.SId
from student left join sc on student.SId=sc.SId 
where EXISTS 
(select *
from teacher ,course
where teacher.Tname='张三'
and   teacher.TId=course.TId
and 	course.CId=sc.CId))
```

11.查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩

```text
select student.SId,student.Sname,avg(sc.score)
from student ,sc
where student.SId=sc.SId
and   sc.score<60
GROUP BY sc.SId
HAVING count(*)>=2
```

1. 检索" 01 "课程分数小于 60，按分数降序排列的学生信息

```text
select student.*
from student,sc
where sc.CId ='01'
and   sc.score<60
and   student.SId=sc.SId
```

1. 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩

```text
select 
sc.SId,sc.CId,sc.score,t1.avgscore 
from  sc left join (select sc.SId,avg(sc.score) as avgscore 
from sc 
GROUP BY sc.SId) as t1 on sc.SId =t1.SId 
ORDER BY t1.avgscore DESC
```

1. 查询各科成绩最高分、最低分和平均分： 以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率 及格为&gt;=60，中等为：70-80，优良为：80-90，优秀为：&gt;=90 要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列

```text
select sc.CId ,max(sc.score)as 最高分,min(sc.score)as 最低分,AVG(sc.score)as 平均分,count(*)as 选修人数,sum(case when sc.score>=60 then 1 else 0 end )/count(*)as 及格率,sum(case when sc.score>=70 and sc.score<80 then 1 else 0 end )/count(*)as 中等率,sum(case when sc.score>=80 and sc.score<90 and sc.score<80 then 1 else 0 end )/count(*)as 优良率,sum(case when sc.score>=90 then 1 else 0 end )/count(*)as 优秀率 
from sc
GROUP BY sc.CId
ORDER BY count(*)DESC,sc.CId asc
```

1. 按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺

```text
select sc.CId ,@curRank:=@curRank+1 as rank,sc.score
from (select @curRank:=0) as t ,sc
ORDER BY sc.score desc
```

15.1 按各科成绩进行排序，并显示排名， Score 重复时合并名次

```text
select sc.CId , case when @fontscore=score then @curRank when @fontscore:=score then @curRank:=@curRank+1  end as rank,sc.score
from (select @curRank:=0 ,@fontage:=null) as t ,sc
ORDER BY sc.score desc
```

1. 查询学生的总成绩，并进行排名，总分重复时保留名次空缺

```text
select t1.*,@currank:= @currank+1 as rank
from (select sc.SId, sum(score)
from sc
GROUP BY sc.SId 
ORDER BY sum(score) desc) as t1,(select @currank:=0) as t
```

16.1 查询学生的总成绩，并进行排名，总分重复时不保留名次空缺

```text
select t1.*, case when @fontscore=t1.sumscore then @currank  when @fontscore:=t1.sumscore  then @currank:=@currank+1  end as rank
from (select sc.SId, sum(score) as sumscore
from sc
GROUP BY sc.SId 
ORDER BY sum(score) desc) as t1,(select @currank:=0,@fontscore:=null) as t
```

1. 统计各科成绩各分数段人数：课程编号，课程名称，\[100-85\]，\[85-70\]，\[70-60\]，\[60-0\] 及所占百分比

```text
select course.CId,course.Cname,t1.*
from course LEFT JOIN (
select sc.CId,CONCAT(sum(case when sc.score>=85 and sc.score<=100 then 1 else 0 end )/count(*)*100,'%') as '[85-100]',
CONCAT(sum(case when sc.score>=70 and sc.score<85 then 1 else 0 end )/count(*)*100,'%') as '[70-85)',
CONCAT(sum(case when sc.score>=60 and sc.score<70 then 1 else 0 end )/count(*)*100,'%') as '[60-70)',
CONCAT(sum(case when sc.score>=0 and sc.score<60 then 1 else 0 end )/count(*)*100,'%') as '[0-60)'
from sc
GROUP BY sc.CId) as t1 on course.CId=t1.CId
```

1. 查询各科成绩前三名的记录

思路：前三名转化为若大于此成绩的数量少于3即为前三名。

```text
select *
from sc  
where  (select count(*) from sc as a where sc.CId =a.CId and  sc.score <a.score )<3
ORDER BY CId asc,sc.score desc
```

1. 查询每门课程被选修的学生数

```text
select sc.CId,count(*)
from sc 
GROUP BY sc.CId
```

1. 查询出只选修两门课程的学生学号和姓名

```text
select student.SId,student.Sname
from sc,student
where student.SId=sc.SId  
GROUP BY sc.SId
HAVING count(*)=2
```

21.查询男生、女生人数

```text
select student.Ssex ,count(*) as 人数
from student 
GROUP BY student.Ssex
```

1. 查询名字中含有「风」字的学生信息

```text
select *
from student 
where student.Sname like '%风%'
```

23.查询同名同性学生名单，并统计同名人数

```text
select *
from student LEFT JOIN (select Sname,Ssex,COUNT(*)同名人数 from Student group by Sname,Ssex) as t1
on student.Sname =t1.Sname and student.Ssex=t1.Ssex
where t1.同名人数>1
```

24.查询 1990 年出生的学生名单

```text
select *
from student
where YEAR(student.Sage)=1990
```

25.查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列

```text
select sc.CId,AVG(sc.score)
from sc 
GROUP BY sc.CId
ORDER BY AVG(sc.score) desc ,sc.CId asc
```

26.查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩

```text
select student.SId,student.Sname,t1.avgscore
from student INNER JOIN (select sc.SId ,AVG(sc.score) as avgscore from sc GROUP BY sc.SId HAVING AVG(sc.score)>85) as t1 on 
student.SId=t1.SId
```

1. 查询课程名称为「数学」，且分数低于 60 的学生姓名和分数

```text
select student.Sname ,t1.score
from student INNER JOIN  (select sc.SId,sc.score 
from sc,course
where sc.CId=course.CId
and   course.Cname='数学'
and   sc.score<60)as t1 on student.SId=t1.SId 
```

1. 查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）

```text
select student.SId,sc.CId,sc.score from Student  left join sc  on student.SId=sc.SId 
```

1. 查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数

```text
select student.Sname,course.Cname,sc.score
from student,sc,course
where sc.score>=70
and  student.SId=sc.SId
and sc.CId=course.CId
```

30.查询存在不及格的课程

```text
select DISTINCT sc.CId
from sc
where sc.score <60
```

31.查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名

```text
select student.SId,student.Sname
from student ,sc
where sc.CId='01'
and  student.SId=sc.SId
and  sc.score>80
```

1. 求每门课程的学生人数

```text
select sc.CId,count(*) as 学生人数
from sc
GROUP BY sc.CId
```

1. 成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩

```text
select student.*,sc.score
from student ,course ,teacher ,sc
where course.CId=sc.CId
and course.TId=teacher.TId
and teacher.Tname='张三'
and student.SId =sc.SId
LIMIT 1
```

1. 成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩

```text
select student.*,t1.score
from student INNER JOIN (select sc.SId,sc.score, case when @fontage=sc.score then @rank when @fontage:=sc.score then @rank:=@rank+1 end  as rank
from course ,teacher ,sc,(select @fontage:=null,@rank:=0) as t
where course.CId=sc.CId
and course.TId=teacher.TId
and teacher.Tname='张三'
ORDER BY sc.score DESC) as t1 on student.SId=t1.SId
where t1.rank=1
```

1. 查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩

```text
select *
from sc as t1
where exists(select * from sc as t2 where t1.SId=t2.SId and t1.CId!=t2.CId and t1.score =t2.score )
```

36.查询每门功成绩最好的前两名

```text
select *
from sc as t1
where (select count(*) from sc as t2 where t1.CId=t2.CId and t2.score >t1.score)<2
ORDER BY t1.CId
```

37.统计每门课程的学生选修人数（超过 5 人的课程才统计）

```text
select sc.CId as 课程编号,count(*) as 选修人数
from sc 
GROUP BY sc.CId
HAVING count(*)>5
```

38.检索至少选修两门课程的学生学号

```text
select DISTINCT t1.SId
from sc as t1 
where (select count(* )from sc where t1.SId=sc.SId)>=3
```

1. 查询选修了全部课程的学生信息

```text
select student.*
from sc ,student 
where sc.SId=student.SId
GROUP BY sc.SId
HAVING count(*) = (select DISTINCT count(*) from course )
```

40.查询各学生的年龄，只按年份来算

```text
select student.SId as 学生编号,student.Sname  as  学生姓名,TIMESTAMPDIFF(YEAR,student.Sage,CURDATE()) as 学生年龄
from student
```

1. 按照出生日期来算，当前月日 &lt; 出生年月的月日则，年龄减一

```text
select student.SId as 学生编号,student.Sname  as  学生姓名,TIMESTAMPDIFF(YEAR,student.Sage,CURDATE()) as 学生年龄
from student
```

42.查询本周过生日的学生

```text
select *
from student 
where YEARWEEK(student.Sage)=YEARWEEK(CURDATE())
```

1. 查询下周过生日的学生

```text
select *
from student 
where YEARWEEK(student.Sage)=CONCAT(YEAR(CURDATE()),week(CURDATE())+1)
```

44.查询本月过生日的学生

```text
select *
from student 
where EXTRACT(YEAR_MONTH FROM student.Sage)=EXTRACT(YEAR_MONTH FROM CURDATE())
```

45.查询下月过生日的学生

```text
select *
from student 
where EXTRACT(YEAR_MONTH FROM student.Sage)=EXTRACT(YEAR_MONTH FROM DATE_ADD(CURDATE(),INTE
```

## 数据库视图

### 1. 是什么

视图是虚拟的表，本身不包含数据，数据都存储在原始表中

### 2. 创建视图

```sql
CREATE VIEW myview AS
SELECT C1, Concat(C2, C3)
FROM mytable
WHERE C1 <= 2;
```

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427171937.png)

### 3. 有什么用

* 简化复杂的查询，比如复杂的连接查询；
* 只使用实际表的一部分数据；
* 通过只给用户访问视图的权限，保证数据的安全性；
* 更改数据格式和表示。

### 4. 何时可以更新

因为视图不存储数据，所以更新视图需要去更新原始表。如果视图定义只依赖于一个原始表，就很容易进行更新操作。但如果视图定义中有以下操作，那么就不能进行视图的更新：

* 分组查询
* 连接查询
* 子查询
* Union
* 聚集函数
* DISTINCT
* 计算字段

## 乐观锁

### 1. 并发控制

在多线程环境下，为了保证线程安全，需要使用并发控制。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427165230.png)

数据库管理系统中有事务的概念，它是一组操作，并且满足 ACID 特性。一个事务可以看成一组任务，而任务是由线程驱动的，因此事务也可以并发地执行。并发执行多个事务时，为了保证每个事务都具有 ACID 特性，也同样需要使用并发控制。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427164629.png)

主要有两种并发控制方法：悲观并发控制和乐观并发控制，应该注意到它们都是思想，而不是具体的实现。

### 2. 悲观并发控制

悲观并发控制保持着悲观的态度，认为并发执行过程一定会出现问题，也就一定要做点什么去防止出现问题。

传统的锁机制都是悲观并发控制的实现，通过加锁从而保证多线程互斥地修改和访问共享数据，那么多线程就不会竞争使用共享数据，也就不会产生线程安全问题。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427165523.png)

在下面的 Java 示例中，通过使用 synchronized 锁机制为 add\(\) 和 get\(\) 方法加锁，从而保证了多线程互斥地修改和访问 cnt 共享数据。

```text
public class PressimisticLocking {

    private int cnt = 0;

    public synchronized void add() {
        cnt++;
    }

    public synchronized int get() {
        return cnt;
    }
}
```

在数据库管理系统中提供了两种锁：共享锁（S）和排它锁（X），也称为读锁和写锁。

* 一个事务对数据 A 加了 X 锁，就可以对 A 进行读取和更新，加锁期间其它事务不能对 A 加任何锁。
* 一个事务对数据 A 加了 S 锁，可以对 A 进行读取操作，但是不能进行更新操作，加锁期间其它事务能对 A 加 S 锁，但是不能加 X 锁。

MySQL 的 InnoDB 存储引擎可以使用以下方法分别加 S 锁和 X 锁：

```text
SELECT ... LOCK In SHARE MODE;
SELECT ... FOR UPDATE;
```

悲观并发控制能够保证线程安全性，但是无论共享数据是否真的会出现竞争，它都要进行加锁。而加锁操作需要涉及用户态核心态转换、维护锁计数器和检查是否有被阻塞的线程需要唤醒等操作，代价非常高。所以悲观并发控制只适合共享数据经常出现竞争的场景，但是对于共享数据基本不会出现竞争的场景，它花费了很多不必要的锁开销。

### 3. 乐观并发控制

针对悲观并发控制对于共享数据基本不会出现竞争的情况下也需要加锁的问题，出现了乐观并发控制。它保持乐观的态度，认为并发执行过程不会对共享数据出现竞争问题。它只在修改数据之后检测一下修改期间该共享数据有没有出现竞争问题，也就是说有没有其它线程也同样修改了该共享数据，如果没有则修改成功，如果有则需要重做。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427171324.png)

#### CAS 实现

CAS 指令是硬件支持的操作：比较并交换（Compare-and-Swap），它需要有 3 个操作数，分别是内存地址 V、旧值 A 和新值 B。CAS 用于冲突检测，内存地址 V 中的值如果不等于旧值 A，则表示冲突，否则才将内存地址 V 的值更新为新值 B。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/otherae4136a8-83b2-4d02-bc14-dc6d4afc4a96.gif)

Java 中的 AtomicInteger 使用 CAS 来实现，以下使用 AtomicInteger 声明了一个共享变量 cnt，该共享变量在多线程环境下不会出现线程安全问题。

```text
public class AtomicExample {

    private AtomicInteger cnt = new AtomicInteger();

    public void add() {
        cnt.incrementAndGet();
    }

    public int get() {
        return cnt.get();
    }
}
```

AtomicInteger 的核心代码是 getAndAddInt\(\)，其中 var1 是内存地址 V，getIntVolatile\(var1, var2\) 得到旧值 A，而 var5 + var4 是新值 B。可以看到在检测到冲突时，AtomicInteger 采用不断重试的方式，直到不再发生冲突为止。

```text
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));
    return var5;
}
```

如果一个变量初次读取的时候是 A 值，它的值被改成了 B，后来又被改回为 A，那 CAS 操作就会误认为它从来没有被改变过，这就是 ABA 问题。J.U.C 包提供了一个带有标记的原子引用类 AtomicStampedReference 来解决这个问题，它可以通过控制变量值的版本来保证 CAS 的正确性。

#### 使用版本号实现

在数据库管理系统中，可以为表增加一个 version 列，那么每个记录就可以维护一个版本号，每次修改时版本号加 1。在对记录进行修改之前先读出 version，并在修改后判断 version 是否发生改变。如果没有发生改变就表示没有发生冲突，执行提交，否则回滚。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427171209.png)

```text
start transaction;
select version from t_goods where id=#{id};
update t_goods set status=2,version=version+1 where id=#{id} and version=version;
commit;
```

## 死锁

### 1. 死锁为什么存在

为了保证多进程（或多线程）的安全性，一个进程在使用临界资源（必须互斥访问的资源）时需要对临界资源加锁，从而防止和其它进程同时使用该临界资源。如果有其它进程已经在使用该临界资源，那么就无法完成加锁操作，会处于一直等待的状态。在访问临界资源之后，需要对临界资源进行解锁，从而把临界资源让出来给其它进程使用。

但是当满足某些条件时，导致无法对资源完成解锁操作，那么其它想要访问这些资源的进程就无法完成加锁操作，而处于一直等待的状态。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427152711.png)

### 2. 死锁产生条件

* 互斥：每个资源要么已经分配给了一个进程，要么就是可用的，也就是说资源必须是临界资源。
* 占有和等待：已经得到了某个资源的进程可以再请求新的资源。
* 不可抢占：已经分配给一个进程的资源不能强制性地被抢占，它只能被占有它的进程显式地释放。
* 环路等待：有两个或者两个以上的进程组成一条环路，该环路中的每个进程都在等待下一个进程所占有的资源。

在下图中，有两个进程 P1 和 P2，以及两个资源 R1 和 R2。进程指向资源表示进程请求使用资源，资源指向进程表示资源已经分配给进程。下图出现了环路等待，所有进程都不能继续执行下去，而是处于一直等待状态。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427152838.png)

### 3. 死锁示例代码

以下示例代码模拟银行转账操作。Transfer 是转账类，实现了 Runnable 接口，可以被线程执行。在 run\(\) 方法中，为了保证 from 账户的扣钱操作和 to 账户的加钱操作具有原子性，需要使用 synchronized 同时锁住 from 和 to 对象。

```java
public class Transfer implements Runnable {

    private final Account from;
    private final Account to;
    private final int amount;

    public Transfer(Account from, Account to, int amount) {
        this.from = from;
        this.to = to;
        this.amount = amount;
    }

    @Override
    public void run() {
        synchronized (from) {
            synchronized (to) {
                from.amount = from.amount - amount;
                to.amount = to.amount + amount;
                System.out.println("success");
            }
        }

    }
}

public class Account {
    int amount;
}
```

启动了两个线程 t1 和 t2，并且新建两个账户 a1 和 a2，两个线程的转账对象不同。如果 t1 锁住 a1、t2 锁住了 a2， t1 想要锁住 a2、t2 想要锁住 a1，那么就出现环路等待。

```java
public class DeadLock {
    public static void main(String[] args) {
        Account a1 = new Account();
        Account a2 = new Account();
        Thread t1 = new Thread(new Transfer(a1, a2, 10));
        Thread t2 = new Thread(new Transfer(a2, a1, 10));
        t1.start();
        t2.start();
    }
}
```

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427153149.png)

为了更容易看到死锁效果，可以两个加锁操作之间为线程加一个停顿时间。

```java
@Override
public void run() {
    synchronized (from) {
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        synchronized (to) {
            from.amount = from.amount - amount;
            to.amount = to.amount + amount;
            System.out.println("success");
        }
    }
}
```

### 4. 死锁处理

#### 4.1 死锁预防

死锁预防是在程序运行之前通过某些手段从而保证不发生死锁。

#### 4.1.1 破坏互斥条件 <a id="section411"></a>

临界资源需要互斥访问，所以基本不能破坏互斥条件。但也可以使用一些手段来使得多个进程可以用共享的方式去使用临界资源。例如假脱机打印机技术允许若干个进程同时输出，唯一真正请求物理打印机的进程是打印机守护进程。

#### 4.1.2 破坏占有和等待条件 <a id="section412"></a>

规定所有进程在开始执行前一次性获取全部资源。

但这种方式会增加开销，并使得并发程度变低。

对于上面的死锁示例代码，可以将锁的粒度变为 Account 类，那么获取该锁就相当于获取了所有 Account 对象锁。

```java
@Override
public void run() {
    synchronized (Account.class) {
        from.amount = from.amount - amount;
        to.amount = to.amount + amount;
        System.out.println("success");
    }
}
```

#### 4.1.3 破坏不可抢占条件 <a id="section413"></a>

规定在进程在请求资源失败时，释放它获得的所有资源。

#### 4.1.4 破坏环路等待 <a id="section414"></a>

给资源统一编号，进程只能按编号顺序来请求资源。

但有些资源本身并不具有编号属性，如果加上编号的话，那么会让程序逻辑变得复杂。

#### 4.2 死锁检测与恢复

不试图阻止死锁，而是当检测到死锁发生时，采取措施进行恢复。

主要有两种死锁检测方法：超时机制和检测是否存在环路。

在检测到发生死锁之后，可以使用进程回退或者事务回滚等机制，释放获取的资源，之后再重新执行。

InnoDB 存储引擎使用检测是否存在环路的方式，并且选择将回滚操作代价小的事务进行回滚。

### 5. 活锁与饥饿

活锁是指进程互相谦让，都释放资源给别的进程，导致资源在进程之间跳动但是进程却一直不执行。

饥饿是指将低优先级的进程长时间请求不到所需要的资源。但是饥饿中进程最后可以请求到资源，只要不再有高优先级的进程使用资源，这和死锁有所不同。

## DELETE、TRUNCATE 和 DROP

### 1. 作用

DELETE 删除表中 WHERE 语句指定的数据。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427172519.png)

TRUNCATE 清空表，相当于删除表中的所有数据。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427172344.png)

DROP 删除表结构。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427172441.png)

### 2. 事务

* DELETE 会被放到日志中以便进行回滚；
* TRUNCATE 和 DROP 立即生效，不会放到日志中，也就不支持回滚。

### 3. 删除空间

* DELETE 不会减少表和索引占用的空间；
* TRUNCATE 会将表和索引占用的空间恢复到初始值；
* DROP 会将表和索引占用的空间释放。

### 4. 耗时

通常来说，DELETE &lt; TRUNCATE &lt; DROP。

