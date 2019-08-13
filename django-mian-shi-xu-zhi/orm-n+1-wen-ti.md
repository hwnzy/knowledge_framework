# ORM N+1问题



#### 基本场景

```python
class Category(models.Model):
    name = models.CharField(max_length=30)

class Article(models.Model):
    title = models.CharField(max_length=30)
    body = models.TextField()
    category = models.ForeignKey(Category)
    time    = models.DateTimeField()

#----列表页模板
{% for a in Article.objects.all %}
    {{ a.title }}
    {{ a.category.name }}
{% endfor %}
```

在生成列表页面时,首先执行一次

`select * from article limited 0,N`

然后逐条获取category.name,又需要执行N次

`select name from category where id = category_id`

所以N+1问题其实应该叫做1+N 问题,这**只是一个数据库设计模式的问题**.但是会对数据库带来很大的压力,一个简单的列表页可能会有几百次数据库查询

N+1问题并不是ORM独有,只是使用orm的时候,数据库表中的行变成一个对象,于是很自然的就容易使用上面的方法来进行查询 不使用orm进行编程的情况,一般直接用子查询或者inner join

`select a.*,c.name from article a,category b where a.category_id = b.id`

子查询或者inner join对数据库来说,也是很费资源的操作,因为需要锁表,高并发的情况下很容易锁死

要解决1+N问题一般有3种方法

1. **数据库反范式设计**,说直白点,就是把表合并,设计成冗余表,这可能会带来两个问题

   1. 表中存在大量的重复数据项
   2. 表中出现大量的空项,整个表格变成一个稀疏矩阵\(sparse matrix\)

   所以,这种方案显然存储效率不高,但是如果针对这两种情况进行优化,也算是是一种不错的解决办法, [MongoDB](http://www.mongodb.org/)就是这样干的

2. **加缓存** 把整个列表页加上缓存. 这样 无论是继续执行1+N次查询,还是用inner join 1次查询搞定,都可以.

   这种方法的缺点是

   1. 更新缓存 需要成本,增加了代码复杂度
   2. 某些场景要求数据实时性,无法使用缓存

3. **把N+1次查询变成2次查询**

   简单说 先执行 `select *,category_id from article limited 0,N`

   然后遍历结果列表,取出所有的category\_id,去掉重复项

   再执行一次 `select name from category where id in (category id list)`

#### 性能优化

把子查询/join查询 分成两次,是 高并发网站数据库调优中非常有效的常见做法,虽然会花费更多的cpu时间,但是避免了系统的死锁,提高了并发响应能力

数据库本身处理不了高并发,因为我们只能保证单个数据项的操作是原子的,而数据库的查询是以 列表为基本单元,这是个天然矛盾,无解

数据库设计范式不在web framework能力范围内,所以django的ORM 只支持后面两种做法

* `Article.ojbects.select_related()` 这就是inner join
* `Article.objects.prefetch_related('category')` 这是2次查询

