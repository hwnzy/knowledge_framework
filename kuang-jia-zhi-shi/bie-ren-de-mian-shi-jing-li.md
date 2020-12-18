# 别人的面试经历

## 1. 三面挂了

  
提前一天收到通知，要求9月26日前往[滴滴](/jump/super-jump/word?word=%E6%BB%B4%E6%BB%B4)参加面试，等待一个多小时之后，活动执行负责人跟我们宣布当天无法面试这么多人，于是改到了9月28日。在9月28日一早，我前往了北京西二旗的[滴滴](/jump/super-jump/word?word=%E6%BB%B4%E6%BB%B4)参加面试，在经过漫长的等待和蹭了一顿免费盒饭之后，终于通知我准备面试了。

###  一面

  
 一面的面试官看起来很有意思，不拘小节的风格，看到我主要使用语言是Python表示比较有兴趣，看得出来面试官也是比较善用Python的，随口问了一句with语法你知道吗？开始没听懂，后来反应过来说要实现 enter和 exit就可以使用with语法，面试官表示满意。后来又问了一些Python的问题，比如async是什么，如何判断一个instance是否为某一个类，这些问题我回答基本正确，顺利通过。

 接下来重头戏手撸代码的[算法题](/jump/super-jump/word?word=%E7%AE%97%E6%B3%95%E9%A2%98)来了。

 第一题是二叉搜索树如何原地变成双向有序[链表](/jump/super-jump/word?word=%E9%93%BE%E8%A1%A8)。我一开始理解错误，以为可以新建一个[链表](/jump/super-jump/word?word=%E9%93%BE%E8%A1%A8)，急急忙忙写完之后面试官说这个题明显不能新建[链表](/jump/super-jump/word?word=%E9%93%BE%E8%A1%A8)，要不然一个中序不就结束了，我也是觉得尴尬至极，后来在面试官的点拨之下，完成代码。

 第二题是只能使用递归不可开辟额外空间完成栈内元素的翻转。这个题我了解大概的思路，可是在他的一再提示下还是没有思路，只能作罢，面试官很不满意。

 第三题是两个递增数组合并成一个递减数组。面试官在考察完前两个题目之后，明显是想再给我一次机会，否则就之前的表现肯定就fail了，这个题难度比较低，属于放水，在此感谢面试官。 【总结】一共面试了70分钟，期间面试官人很好，会跟我讨论问题的思路，我也会时不时的提出自己的问题，如果不是特别重大的问题，他也会耐心的跟我一起讨论和解释。  


###  二面

  
 二面几乎没有写代码，只是在最后几分钟问了一个代码题，难度也不大，多个有序数组如何找到他们的交集，并给出时间复杂度。

 二面主要进行了以下的这些问题和讨论：

1.  介绍了我自己的[项目](/jump/super-jump/word?word=%E9%A1%B9%E7%9B%AE)，提出疑问：为什么不使用开源框架、系统的耦合度问题等。
2.  我的[项目](/jump/super-jump/word?word=%E9%A1%B9%E7%9B%AE)中用到的是NoSQL的mongodb，面试官问mongodb是如何做持久化的、它是否保证是数据一致性、是否会发生数据丢失现象，以及mongodb的索引是如何实现的，它与MySQL之类的SQL数据库有什么区别。最后一个问题是我给自己挖的坑~
3.  介绍第二个[项目](/jump/super-jump/word?word=%E9%A1%B9%E7%9B%AE)，爬虫的基本介绍
4.  面试官让我考虑一个场景的解决方案：已知一个乘客的地位位置，如果在O\(1\)的平均时间下找到距离最近的司机的列表，在讨论中他提到了区域划分粒度的问题，我就就势提出可以采用分级hash的策略。
5.  在浏览器中输入网址回车到页面显示完全，这中间都发生了什么。DNS，HTTP报文-&gt;TCP报文，TCP报文-&gt;IP报文，[keep](/jump/super-jump/word?word=keep)-alive，ARP，TCP的拥塞控制，丢包之后的重传与恢复。问的还算是比较细致。

 【总结】二面一共70分钟，面试官是一个比较随和安静的人，整个面试过程比较流畅，虽然一些问题我不知道，最后他表示对我有兴趣，希望我能过三面，然后我就被他一口毒奶~

###  三面

  
 三面的面试官是一个比较严肃的Boss，主要是针对简历中的[项目](/jump/super-jump/word?word=%E9%A1%B9%E7%9B%AE)发问，比如问道mongodb跟sql数据库比有什么区别，有什么优势；如果防止网络爬虫被网站识别并封号，然后抓住我的简历中一个用词失误使劲问，最后我只好说我的意思跟这个词儿好像不是特别搭，是我用词不当了。整个面试过程整整30分钟，结束的时候对我说“挺好的，不错”，然后一分钟之后，活动执行人员告诉我“你的面试结束了”，然后我一脸懵逼的踏上了去地铁站的征程。。。

###  总结

1.  自己的[项目](/jump/super-jump/word?word=%E9%A1%B9%E7%9B%AE)一定要特别清楚，所用到的技术的机制和与其他技术的对比一定要清楚，“我只是用过这个技术”可能会导致面试fail
2.  面试官总体来说还是很友好的，一起讨论问题的氛围很重要，三面就一直是面试官不停发问，我疲于应对，没有形成良好互动。
3.  【对自己说的】加油，offer一定会有的！

## 2. 三面的经历

### 二面

首先简单自我介绍，我面试的是大数据开发相关的职位，

主要考察的问题是算法，其中，快排，以及优化，搜索，优化，动态规划等题目。

还有hive，Hbase，优化等问题。

flink中报文处理问题。

这里说一个比较好玩的例子。

查找一个单链表是否存在环？

解决方案

设置两个指针，一个每次走一步，另一个每次走两部。

如果存在环的话，这两个指针总会相遇。

### 三面

算法 + 项目

算法是洗牌算法

解决方案如下：

```text
void MySwap(int &x, int &y)
{
    int temp = x;
    x = y;
    y = temp;
}

void Shuffle(int n)
{
    for(int i=n-1; i>=1; i--)
    {
        MySwap(num[i], num[rand()%(i+1)]);
    }
}
```

## 3. 一个遇到要写SQL语句的面试 

**学生表:tb\_student\(name:学生姓名，id：学号，class：班级，in\_time：入学时间，age:年龄，sex：性别，major：专业\)**

**1\)学生成绩表：tb\_score\(id：学号，course：课程，score：分数\)**

1. 筛选出2017年入学的“计算机”专业年龄最小的10位同学名单（姓名、学号、班级、年龄）

```text
select a.name,a.id,a.class,a.age
from tb_student a
where year(in_time)=2017 and major="计算机"
order by a.age asc
limit 10
select a.name, a.id, a.class, a.age from student a where year order by age asc
```

2.统计每个班同学各科成绩平均分大于80分的人数和人数占比

```text
SELECT a.class,count(if(b.score>80,true,null)) as numover80,count(if(b.score>80,true,null))/count(score) as total
FROM tb_student a
inner join tb_score b
on a.id=b.id
GROUP BY class
having avg(b.score)>80
```

**2\)用户教育经历表：tb\_user\_edu\(uid：用户id，star\_date：入学时间，end\_date：毕业时间，degree：学历，school：学校，major：专业\)**

```text
select uid
from tb_user_edu
group by uid,end_date
order by end_date
limit 1
```

**3\)table1（id：自增id，money：费用）问题：按id顺序累加money，取出累计值与1000相差最小差值的id。**

```text
select d.id,min(d.差值)
from
(
select c.id,ABS(1000-c.cum) 差值
from
(
SELECT a.id,a.money,SUM(lt.money) as cum
FROM table1 a JOIN table1 lt 
ON a.id >= lt.id
GROUP BY a.id
ORDER BY a.id
)c
)d
group by d.id
```

这道题犹豫了一下，用多表连接进行累加，然后对做差值进行绝对值化，然后找出绝对值的最小值就是和1000相差最近的了。

4） Employee 表包含所有员工信息，每个员工有其对应的 Id, salary 和 DepartmentId。

+----+-------+--------+--------------+

\| Id \| Name \| Salary \| DepartmentId \|

+----+-------+--------+--------------+

\| 1 \| Joe \| 70000 \| 1 \|

\| 2 \| Henry \| 80000 \| 2 \|

\| 3 \| Sam \| 60000 \| 2 \|

\| 4 \| Max \| 90000 \| 1 \|

Department 表包含公司所有部门的信息。

+----+----------+

\| Id \| Name \|

+----+----------+

\| 1 \| IT \|

\| 2 \| Sales \|

+----+----------+

编写一个 SQL 查询，找出每个部门工资第二高的员工。

```text
select a.name as Department,b.name as Employee,b.salary as Salary
from department a
inner join
(
select *,rank() over(partition by deparmentid order by salary desc) as ranking from employee
)b
on a.id=b.departmentid
where ranking=2
```

**因为没有真实表验证，所以有错误请大家指出，感谢！**

