# Django



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

QuerySet

#### 可切片

使用Python 的切片语法来限制查询集记录的数目 。它等同于SQL 的LIMIT 和OFFSET 子句。

| 1 | `>>> Entry.objects.all()[:5]      # (LIMIT 5)` |
| :--- | :--- |


| 1 | `Entry.objects.all()[5:10]    # (OFFSET 5 LIMIT 5)` |
| :--- | :--- |


不支持负的索引（例如Entry.objects.all\(\)\[-1\]）。通常，查询集 的切片返回一个新的查询集 —— 它不会执行查询。　　　

#### 可迭代

| 1234 | `articleList=models.Article.objects.all()` `for` `article in` `articleList:    print(article.title)`　　 |
| :--- | :--- |


#### 惰性查询

查询集 是惰性执行的 —— 创建查询集不会带来任何数据库的访问。你可以将过滤器保持一整天，直到查询集 需要求值时，Django 才会真正运行这个查询。

| 123456 | `queryResult=models.Article.objects.all() # not hits database`  `print(queryResult) # hits database`  `for` `article in` `queryResult:    print(article.title)    # hits database` |
| :--- | :--- |


 一般来说，只有在“请求”查询集 的结果时才会到数据库中去获取它们。当你确实需要结果时，查询集 通过访问数据库来_求值_。 关于求值发生的准确时间，参见[_何时计算查询集_](http://python.usyiyi.cn/documents/django_182/ref/models/querysets.html#when-querysets-are-evaluated)。　　

#### 缓存机制

每个查询集都包含一个缓存来最小化对数据库的访问。理解它是如何工作的将让你编写最高效的代码。

在一个新创建的查询集中，缓存为空。首次对查询集进行求值 —— 同时发生数据库查询 ——Django 将保存查询的结果到查询集的缓存中并返回明确请求的结果（例如，如果正在迭代查询集，则返回下一个结果）。接下来对该查询集 的求值将重用缓存的结果。

请牢记这个缓存行为，因为对查询集使用不当的话，它会坑你的。例如，下面的语句创建两个查询集，对它们求值，然后扔掉它们：

| 12 | `print([a.title for` `a in` `models.Article.objects.all()])print([a.create_time for` `a in` `models.Article.objects.all()])` |
| :--- | :--- |


这意味着相同的数据库查询将执行两次，显然倍增了你的数据库负载。同时，还有可能两个结果列表并不包含相同的数据库记录，因为在两次请求期间有可能有Article被添加进来或删除掉。为了避免这个问题，只需保存查询集并重新使用它：　

| 123 | `queryResult=models.Article.objects.all()print([a.title for` `a in` `queryResult])print([a.create_time for` `a in` `queryResult])` |
| :--- | :--- |


**何时查询集不会被缓存?**

查询集不会永远缓存它们的结果。当只对查询集的部分进行求值时会检查缓存， 如果这个部分不在缓存中，那么接下来查询返回的记录都将不会被缓存。所以，这意味着使用切片或索引来限制查询集将不会填充缓存。

例如，重复获取查询集对象中一个特定的索引将每次都查询数据库：

| 123 | `>>> queryset =` `Entry.objects.all()>>> print` `queryset[5] # Queries the database>>> print` `queryset[5] # Queries the database again` |
| :--- | :--- |


然而，如果已经对全部查询集求值过，则将检查缓存：　　

| 1234 | `>>> queryset =` `Entry.objects.all()>>> [entry for` `entry in` `queryset] # Queries the database>>> print` `queryset[5] # Uses cache>>> print` `queryset[5] # Uses cache` |
| :--- | :--- |


下面是一些其它例子，它们会使得全部的查询集被求值并填充到缓存中：

| 1234 | `>>> [entry for` `entry in` `queryset]>>> bool(queryset)>>> entry in` `queryset>>> list(queryset)` |
| :--- | :--- |


注：简单地打印查询集不会填充缓存。　　

| 123 | `queryResult=models.Article.objects.all()print(queryResult) #  hits databaseprint(queryResult) #  hits database`　 |
| :--- | :--- |


#### exists\(\)与iterator\(\)方法

**exists：**

简单的使用if语句进行判断也会完全执行整个queryset并且把数据放入cache，虽然你并不需要这些 数据！为了避免这个，可以用exists\(\)方法来检查是否有数据：

| 123 | `if` `queryResult.exists():    #SELECT (1) AS "a" FROM "blog_article" LIMIT 1; args=()        print("exists...")` |
| :--- | :--- |


**iterator:**

当queryset非常巨大时，cache会成为问题。

处理成千上万的记录时，将它们一次装入内存是很浪费的。更糟糕的是，巨大的queryset可能会锁住系统 进程，让你的程序濒临崩溃。要避免在遍历数据的同时产生queryset cache，可以使用iterator\(\)方法 来获取数据，处理完数据就将其丢弃。

| 1234567 | `objs =` `Book.objects.all().iterator()# iterator()可以一次只从数据库获取少量数据，这样可以节省内存for` `obj in` `objs:    print(obj.title)#BUT,再次遍历没有打印,因为迭代器已经在上一次遍历(next)到最后一次了,没得遍历了for` `obj in` `objs:    print(obj.title)` |
| :--- | :--- |


当然，使用iterator\(\)方法来防止生成cache，意味着遍历同一个queryset时会重复执行查询。所以使 \#用iterator\(\)的时候要当心，确保你的代码在操作一个大的queryset时没有重复执行查询。

总结:

queryset的cache是用于减少程序对数据库的查询，在通常的使用下会保证只有在需要的时候才会查询数据库。 使用exists\(\)和iterator\(\)方法可以优化程序对内存的使用。不过，由于它们并不会生成queryset cache，可能 会造成额外的数据库查询。　 

Django ORM用到三个类：Manager、QuerySet、Model。Manager定义表级方法（表级方法就是影响一条或多条记录的方法），我们可以以models.Manager为父类，定义自己的manager，增加表级方法；QuerySet：Manager类的一些方法会返回QuerySet实例，QuerySet是一个可遍历结构，包含一个或多个元素，每个元素都是一个Model 实例，它里面的方法也是表级方法，前面说了，Django给我们提供了增加表级方法的途径，那就是自定义manager类，而不是自定义QuerySet类，一般的我们没有自定义QuerySet类的必要；django.db.models模块中的Model类，我们定义表的model时，就是继承它，它的功能很强大，通过自定义model的instance可以获取外键实体等，它的方法都是记录级方法（都是实例方法，貌似无类方法），不要在里面定义类方法，比如计算记录的总数，查看所有记录，这些应该放在自定义的manager类中。以Django1.6为基础。

### 1. Queryset简介

每个Model都有一个默认的manager实例，名为objects，QuerySet有两种来源：通过manager的方法得到、通过QuerySet的方法得到。mananger的方法和QuerySet的方法大部分同名，同意思，如filter\(\),update\(\)等，但也有些不同，如manager有create\(\)、get\_or\_create\(\)，而QuerySet有delete\(\)等，看源码就可以很容易的清楚Manager类与Queryset类的关系，Manager类的绝大部分方法是基于Queryset的。一个QuerySet包含一个或多个model instance。QuerySet类似于Python中的list，list的一些方法QuerySet也有，比如切片，遍历。[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
>>> from userex.models import UserEx

>>> type(UserEx.objects)

<class ‘django.db.models.manager.Manager’>

>>> a = UserEx.objects.all()

>>> type(a)

<class ‘django.db.models.query.QuerySet’>
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

QuerySet是延迟获取的，只有当用到这个QuerySet时，才会查询数据库求值。另外，查询到的QuerySet又是缓存的，当再次使用同一个QuerySet时，并不会再查询数据库，而是直接从缓存获取（不过，有一些特殊情况）。一般而言，当对一个没有求值的QuerySet进行的运算，返回的是QuerySet、ValuesQuerySet、ValuesListQuerySet、Model实例时，一般不会立即查询数据库；反之，当返回的不是这些类型时，会查询数据库。下面介绍几种（并非全部）对QuerySet求值的场景。[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
class Blog(models.Model):

    name = models.CharField(max_length=100)

    tagline = models.TextField()

 

    def __unicode__(self):

        return self.name

 

class Author(models.Model):

    name = models.CharField(max_length=50)

    email = models.EmailField()

 

    def __unicode__(self):

        return self.name

 

class Entry(models.Model):

    blog = models.ForeignKey(Blog)

    headline = models.CharField(max_length=255)

    body_text = models.TextField()

    pub_date = models.DateField()

    mod_date = models.DateField()

    authors = models.ManyToManyField(Author)

    n_comments = models.IntegerField()

    n_pingbacks = models.IntegerField()

    rating = models.IntegerField()

 

    def __unicode__(self):

        return self.headline
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

我们以上面的models为例。

#### **1.1 遍历**

```text
a = Entry.objects.all()

for e in a:

    print (e.headline)
```

当遍历一开始时，先从数据库执行查询select \* from Entry得到a，然后再遍历a。注意：这里只是查询Entry表，返回的a的每条记录只包含Entry表的字段值，不管Entry的model中是否有onetoone、onetomany、manytomany字段，都不会关联查询。这遵循的是数据库最少读写原则。我们修改一下代码，如下，遍历一开始也是先执行查询得到a，但当执行print \(e.blog.name\)时，还需要再次查询数据库获取blog实体。[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
from django.db import connection

l = connection.queries  #l是一个列表，记录SQL语句

a = Entry.objects.all()

for e in a:

    print (e.blog.name)

    len(l)
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

遍历时，每次都要查询数据库，l长度每次增1，Django提供了方法可以在查询时返回关联表实体，如果是onetoone或onetomany，那用select\_related，不过对于onetomany，只能在主表（定义onetomany关系的那个表）的manager中使用select\_related方法，即通过select\_related获取的关联对象是model instance，而不能是QuerySet，如下，e.blog就是model instance。对于onetomany的反向和manytomany，要用prefetch\_related，它返回的是多条关联记录，是QuerySet。[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
a = Entry.objects.select_related('blog')

for e in a:

        print (e.blog.name)

        len(l)
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

可以看到从开始到结束，l的长度只增加1。另外，通过查询connection.queries\[-1\]可以看到Sql语句用了join。

#### **1.2 切片**

切片不会立即执行，除非显示指定了步长，如a= Entry.objects.all\(\)\[0:10:2\]，步长为2。

#### **1.3 序列化，即Pickling**

序列化QuerySet很少用。

#### **1.4  repr\(\)**

和str\(\)功能相似，将对象转为字符串，很少用。

#### **1.5 len\(\)**

计算QuerySet元素的数量，并不推荐使用len\(\)，除非QuerySet是求过值的（即evaluated），否则，用QuerySet.count\(\)获取元素数量，这个效率要高。

#### **1.6 list\(\)**

将QuerySet转为list。

#### **1.7 bool\(\)，判断是否为空**

```text
if Entry.objects.filter(headline="Test"):

     print("There is at least one Entry with the headline Test")
```

 同样不建议这种方法判断是否为空，而应该使用QuerySet.exists\(\)，查询效率高。   


### 2. QuerySet的方法

数据库的常用操作就四种：增、删、改、查，QuerySet的方法涉及删、改、查。后面还会讲model对象的方法，model方法主要是增、删、改、还有调用model实例的字段。

#### 2.1 删delete\(\)

原型：delete\(\)

返回：None

相当于delete-from-where, delete-from-join-where。先filter，然后对得到的QuerySet执行delete\(\)方法就行了，它会同时删除关联它的那些记录，比如我删除记录表1中的A记录，表2中的B记录中有A的外键，那同时也会删除B记录，那ManyToMany关系呢？对于ManyToMany，删除其中一方的记录时，会同时删除中间表的记录，即删除双方的关联关系。由于有些数据库，如Sqlite不支持delete与limit连用，所以在这些数据库对QuerySet的切片执行delete\(\)会出错。如

```text
>>> a = UserEx.objects.filter(is_active=False)

>>> b = a[:3]

>>> b.delete() #执行时会报错
```

解决：UserEx.objects.filter\(pk\_\_in=b\).delete\(\) 

in后面可以是一个QuerySet，见 https://docs.djangoproject.com/en/1.6/ref/models/querysets/\#in

#### 2.2 改 update\(\)

批量修改，返回修改的记录数。不过update\(\)中的键值对的键只能是主表中的字段，不能是关联表字段，如下

```text
Entry.objects.update(blog__name='foo')  #错误，无法修改关联表字段，只能修改Entry表的字段

Entry.objects.filter(blog__name='foo').update(comments_on=False)  #正确
```

最好的方法是先filter，查询出QuerySet，然后再执行QuerySet.update\(\)。

由于有些数据库，不支持update与limit连用，所以在这些数据库对QuerySet的切片执行update\(\)会出错。

#### 2.3 查询 filter\(\*\*kwargs\)、exclude\(\*\*kwargs\)、get\(\*\*kwargs\)

相当于select-from-where，select-from-join-where，很多网站读数据库操作最多。可以看到，filter\(\)的参数是变个数的键值对，而不会出现&gt;,&lt;,!=等符号，这些符号分别用\_\_gt,\_\_lt,~Q或exclude\(\)，不过对于!=，建议使用Q查询，更不容易出错。可以使用双下划线对OneToOne、OneToMany、ManyToMany进行关联查询和反向关联查询，而且方法都是一样的，如：[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
>>> Entry.objects.filter(blog__name='Beatles Blog') #限定外键表的字段

#下面是反向连接，不过要注意，这里不是entry_set，entry_set是Blog instance的一个属性，代表某个Blog object

#的关联的所有entry，而QuerySet的方法中反向连接是直接用model的小写，不要把两者搞混。It works backwards,

#too. To refer to a “reverse” relationship, just use the lowercase name of the model.

>>> Blog.objects.filter(entry__headline__contains='Lennon')

>>> Blog.objects.filter(entry__authors__name='Lennon')   #ManyToMany关系，反向连接

>>> myblog = Blog.objects.get(id=1)

>>> Entry.objects.filter(blog=myblog)  #正向连接。与下面一句等价，既可以用实体，也可以用

#实体的主键，其实即使用实体，也是只用实体的主键而已。这两种方式对OneToOne、

#OneToMany、ManyToMany的正向、反向连接都适用。

>>> Entry.objects.filter(blog=1)    #我个人不建议这样用，对于create()，不支持这种用法

>>> myentry = Entry.objects.get(id=1)

>>> Blog.objects.filter(entry=myentry) #ManyToMany反向连接。与下面两种方法等价

>>> Blog.objects.filter(entry=1)   

>>> Blog.objects.filter(entry_id=1)  #适用于OneToOne和OneToMany的正向连接
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

OneToOne的关系也是这样关联查询，可以看到，Django对OneToOne、OneToMany、ManyToMany关联查询及其反向关联查询提供了相同的方式，真是牛逼啊。对于OneToOne、OneToMany的主表，也可以使用下面的方式

Entry.objects.filter\(blog\_id=1\)，因为blog\_id是数据库表Entry的一个字段， 这条语句与Entry.objects.filter\(blog=blog1\)生成的SQL是完全相同的。

与filter类似的还有exclude\(\*\*kwargs\)方法，这个方法是剔除，相当于select-from-where not。可以使用双下划线对OneToOne、OneToMany、ManyToMany进行关联查询和反向关联查询，方法与filter\(\)中的使用方法相同。

```text
>>> Entry.objects.exclude(pub_date__gt=datetime.date(2005, 1, 3), headline='Hello')
```

转为SQL为

SELECT \*

FROM Entry

WHERE NOT \(pub\_date &gt; '2005-1-3' AND headline = 'Hello'\)

#### 2.4 SQL其它关键字在django中的实现

在SQL中，很多关键词在删、改、查时都是可以用的，如order by、 like、in、join、union、and、or、not等等，我们以查询为例，说一下django如何映射SQL的这些关键字的（查、删、改中这些关键字的使用方法基本相同）。

**2.4.1  F类（无对应SQL关键字）**

前面提到的filter/exclude中的查询参数值都是常量，如果我们想比较model的两个字段怎么办呢？Django也提供了方法，F类，F类实例化时，参数也可以用双下划线，也可以逻辑运算，如下[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
>>> from django.db.models import F

>>> Entry.objects.filter(n_comments__gt=F('n_pingbacks'))

>>> from datetime import timedelta

>>> Entry.objects.filter(mod_date__gt=F('pub_date') + timedelta(days=3))

>>> Entry.objects.filter(authors__name=F('blog__name'))
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

**2.4.2 Q类（对应and/or/not）**

如果有or等逻辑关系呢，那就用Q类，filter中的条件可以是Q对象与非Q查询混和使用，但不建议这样做，因为混和查询时Q对象要放前面，这样就有难免忘记顺序而出错，所以如果使用Q对象，那就全部用Q对象。Q对象也很简单，就是把原来filter中的各个条件分别放在一个Q\(\)即可，不过我们还可以使用或与非，分别对应符号为”\|”和”&”和”~”，而且这些逻辑操作返回的还是一个Q对象，另外，逗号是各组条件的基本连接符，也是与的关系，其实可以用&代替（在python manage.py shell测试过，&代替逗号，执行的SQL是一样的），不过那样的话可读性会很差，这与我们直接写SQL时，各组条件and时用换行一样，逻辑清晰。[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
from django.db.models import Q

>>> Poll.objects.get( Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)),

question__startswith='Who')   #正确，但不要这样混用

>>> Poll.objects.get( Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)),

Q(question__startswith='Who'))  #推荐，全部是Q对象

>>> Poll.objects.get( (Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)))&

Q(question__startswith='Who'))  #与上面语句同意，&代替”,”，可读性差

# Q类中时应该可以用F类，待测试。
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

**2.4.3  annotate（无对应SQL关键字）**

函数原型annotate\(_\*args_, _\*\*kwargs_\)

返回QuerySet

往每个QuerySet的model instance中加入一个或多个字段，字段值只能是聚合函数，因为使用annotate时，会用group by，所以只能用聚合函数。聚合函数可以像filter那样关联表，即在聚合函数中，Django对OneToOne、OneToMany、ManyToMany关联查询及其反向关联提供了相同的方式，见下面例子。[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
>>> from django.contrib.auth.models import User

>>> from django.db.models import Count

#计算每个用户的userjob数量，字段命名为ut_num，返回的QuerySet中的每个object都有

#这个字段。在UserJob中定义User为外键，在Job中定义与User是ManyToMany

>>> a = User.objects.filter(is_active=True, userjob__is_active=True). annotate(n=Count(‘userjob’)) #一对多反向连接

>>> b = User.objects.filter(is_active=True, job__is_active=True).annotate(n=Count(‘job__name’))  #多对多反向连接，User与Job是多对多

>>> len(a)  #这里才会对a求值

>>> len(b)  #这里才会对b求值
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

a对应的SQL语句为\(SQL中没有为表起别名,u、ut是我加的\)：[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
select auth.user.*,Count(ut.id) as ut_num

from auth_user as u

left outer join job_userjob as ut on u.id = ut.user_id

where u.is_active=True and ut.is_active=True

group by u.*
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

b对应的SQL语句为\(SQL中没有为表起别名,u、t、r是我加的\)：[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
select u.*,Count(t.name) as n

from auth_user as u

left outer join job_job_users as r on u.id=r.user_id

left outer join job_job as t on r.job_id=t.id

where t.is_active=True and u.is_active=True

group by u.*
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

**2.4.4 order\_by——对应order by**

函数原型 order\_by\(_\*_fields\)

返回QuerySet

正向的反向关联表跟filter的方式一样。如果直接用字段名，那就是升序asc排列；如果字段名前加-，就是降序desc

**2.4.5  distinct——对应distinct**

原型 distinct\(\)

一般与values\(\)、values\_list\(\)连用，这时它返回ValuesQuerySet、ValuesListQuerySet

这个类跟列表很相似，它的每个元素是一个字典。它没有参数（其实是有参数的，不过，参数只在PostgreSQL上起作用）。使用方法为

```text
>>> a=Author.objects.values_list(name).distinct()

>>> b=Author.objects.values_list(name,email).distinct()
```

对应的SQL分别为

```text
select distinct name from Author
```

和

```text
select distinct name,email from Author
```

**2.4.6 values\(\)和values\_list\(\)——对应‘select 某几个字段’**

函数原型values\(\*field\), values\_list\(\*field\)

返回ValuesQuerySet, ValuesListQuerySet

Author.objects.filter\(\*\*kwargs\)对应的SQL只返回主表（即Author表）的所有字段值，即使在查询时关联了其它表，关联表的字段也不会返回，只有当我们通过Author instance用关联表时，Django才会再次查询数据库获取值。当我们不用Author instance的方法，且只想返回几个字段时，就要用values\(\)，它返回的是一个ValuesQuerySet对象，它类似于一个列表，不过，它的每个元素是字典。而values\_list\(\)跟values\(\)相似，它返回的是一个ValuesListQuerySet，也类型于一个列表，不过它的元素不是字典，而是元组。一般的，当我们不需要model instance的方法且返回多个字段时，用values\(\*field\)，而返回单个字段时用values\_list\(‘field’,flat=True\)，这里flat=True是要求每个元素不是元组，而是单个值，见下面例子。而且我们可以返回关联表的字段，用法跟filter中关联表的方式完全相同。[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
>>> a = User.objects.values(‘id’,’username’,’userex__age’)

>>> type(a)

<class ‘django.db.models.query.ValuesQuerySet’>

>>> a

[{‘id’:0,’username’:u’test0’,’ userex__age’: 20},{‘id’:1,’username’:u’test1’,’userex__age’: 25},

 {‘id’:2,’username’:u’test2’, ’ userex__age’: 28}]

>>> b= User.objects.values_list(’username’,flat=True)

>>> b

[u’test0’, u’test1’ ,u’test2’]
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

**2.4.7  select\_related\(\)——对应返回关联记录实体**

原型select\_related\(\*filed\)

返回QuerySet

它可以指定返回哪些关联表model instance，这里的field跟filter\(\)中的键一样，可以用双下划线，但也有不同，You can refer to any ForeignKey or OneToOneField relation in the list of fields passed to select\_related\(\)，QuerySet中的元素中的OneToOne关联及外键对应的是都是关联表的一条记录，如my\_entry=Entry.objects.get\(id=1\)，my\_entry.blog就是关联表的一条记录的对象。select\_related\(\)不能用于OneToMany的反向连接，和ManyToMany，这些都是model的一条记录对应关联表中的多条记录。前面提到了对于a = Author.objects.filter\(\*\*kwargs\)这类语句，对应的SQL只返回主表，即Author的所有字段，并不会返回关联表字段值，只有当我们使用关联表时才会再查数据库返回，但有些时候这样做并不好。看下面两段代码，这两段代码在1.1中提到过。在代码1中，在遍历a前，先执行a对应的SQL，拿到数据后，然后再遍历a，而遍历过程中，每次都还要查询数据库获取关联表。代码2中，当遍历开始前，先拿到Entry的QuerySet，并且也拿到这个QuerySet的每个object中的blog对象，这样遍历过程中，就不用再查询数据库了，这样就减少了数据库读次数。

 [![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
# 代码1

a = Entry.objects.all()

for e in a:

    print (e.blog.name)
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

 [![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
# 代码2

a = Entry.objects.select_related('blog')

for e in a:

        print (e.blog.name)
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

**2.4.8  prefetch\_related\(\*field\) ——对应返回关联记录实体的集合**

函数原型**prefetch\_related**\(\*field\)

返回的是QuerySet

这里的field跟filter\(\)中的键一样，可以用双下划线。用于OneToMany的反向连接，及ManyToMany。其实，prefetch\_related\(\)也能做select\_related\(\)的事情，但由于策略不同，可能相比select\_related\(\)要低效一些，所以建议还是各管各擅长的。select\_related是用select ……join来返回关联的表字段，而prefetch\_related是用多条SQL语句的形式查询，一般，后一条语句用IN来调用上一句话返回的结果。[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
class Restaurant(models.Model):

    pizzas = models.ManyToMany(Pizza, related_name='restaurants')

    best_pizza = models.ForeignKey(Pizza, related_name='championed_by')

 

>>> Restaurant.objects.prefetch_related('pizzas__toppings')

>>> Restaurant.objects.select_related('best_pizza').prefetch_related('best_pizza__toppings')
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

先用select\_related查到best\_pizza对象，再用prefetch\_related 从best\_pizza查出toppings

**2.4.9  extra\(\)——实现复杂的where子句**

函数原型：extra\(select=None, where=None, params=None, tables=None, order\_by=None, select\_params=None\)

基本上，查询时用django提供的方法就够用了，不过有时where子句中包含复杂的逻辑，这种情况下django提供的方法可能不容易做到，还好，django有extra\(\)， extra\(\)中直接写一些SQL语句。不过，不同的数据库用的SQL有些差异，所以尽可能不要用extra\(\)。需要时再看使用方法吧。

**2.4.10 aggregate\(\*args, \*\*kwargs\)——对应聚合函数**

参数为聚合函数，最好用\*\*kwargs的形式，每个参数起一个名字。

该函数与annotate\(\)有何区别呢？annotate相当于aggregate\(\)和group by的结合，对每个group执行aggregate\(\)函数。而单独的aggregate\(\)并没有group by。[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
>>> from django.db.models import Count

>>> q = Blog.objects.aggregate(Count('entry'))  #这是用*args的形式，最好不要这样用

>>> q = Blog.objects.aggregate(number_of_entries=Count('entry'))  #这是用**kwargs的形式

{'number_of_entries': 16}
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

    至此，我们总结了QuerySet方法返回的数据形式，主要有五种。第一种：返回QuerySet，每个object只包含主表字段；第二种：返回QuerySet，每个object除了包含主表所有字段，还包含某些关联表的object，这种情况要用select\_related\(\)和prefetch\_related\(\)，可以是任意深度（即任意多个双下划线）的关联，通常一层关联和二层关联用的比较多；第三种：返回ValuesQuerySet, ValuesListQuerySet，它们的每个元素包含若干主表和关联表的字段，不包含任何实体和关联实例，这种情况要用values\(\)和values\_list\(\)；第四种：返回model instance；第五种:单个值，如aggregate\(\)方法。

**2.4.11  exists\(\)、count\(\)、len\(\)**

如果只是想知道一个QuerySet是否为空，而不想获取QuerySet中的每个元素，那就用exists\(\)，它要比len\(\)、count\(\)、和直接进行if判断效率高。如果只想知道一个QuerySet有多大，而不想获取QuerySet中的每个元素，那就用count\(\)；如果已经从数据库获取到了QuerySet，那就用len\(\)

**2.4.12  contains/startswith/endswith——对应like**

    字段名加双下划线，除了它，还有icontains，即Case-insensitive contains，这个是大小写不敏感的，这需要相应数据库的支持。有些数据库需要设置

才能支持大小写敏感。

**2.4.13  in——对应in**

字段名加双下划线

**2.4.14exclude\(field\_\_in=iterable\)——对应not in**

iterable是可迭代对象

**2.4.15  gt/gte/lt/lte——对应于&gt;,&gt;=,&lt;,&lt;=**

字段名加双下划线

**2.4.16 range——对应于between and**

字段名加双下划线，range后面值是列表

**2.4.17  isnull——对应于is null**

Entry.objects.filter\(pub\_date\_\_isnull=True\)对应的SQL为SELECT ... WHERE pub\_date IS NULL;

**2.4.18  QuerySet切片——对应于limit**

    QuerySet的索引只能是非负整数，不支持负整数，所以QuerySet\[-1\]错误

a=Entry.objects.all\(\)\[5:10\]

b=len\(a\) 

执行Entry.objects.all\(\)\[5:8\]，对于不同的数据库，SQL语句不同，Sqlite 的SQL语句为select \* from tablename limit 3 offset 5; MySQL的SQL语句为select \* from tablename limit 5,3

### Django-Pagination的使用

 Django自身提供了一些类来实现管理分页，数据被分在不同的页面中，并带有“上一页/下一页”标签。这个类叫做Pagination，其定义位于 django/core/paginator.py 中。

一. Paginator类的解释[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
class Paginator(object):

    def __init__(self, object_list, per_page, orphans=0,
                 allow_empty_first_page=True):
        self.object_list = object_list
        self.per_page = int(per_page)
        self.orphans = int(orphans)
        self.allow_empty_first_page = allow_empty_first_page
        self._num_pages = self._count = None
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

1.根据其定义做出以下解释，上述代码没有将其类属性和方法贴出。

* object\_list:可以是列表，元组，查询集或其他含有 count\(\) 或 \_\_len\_\_\(\)方法的可切片对象。对于连续的分页，查询集应该有序，例如有order\_by\(\)项或默认ordering参数。
* per\_page:每一页中包含条目数目的最大值，不包括独立成页的那页。（见下面 orphans参数解释）。
* orphans=0:当你使用此参数时说明你不希望最后一页只有很少的条目。如果最后一页的条目数少于等于orphans的值，则这些条目会被归并到上一页中（此时的上一页变为最后一页）。例如有23项条目， per\_page=10,orphans=0,则有3页，分别为10，10，3.如果orphans&gt;=3,则为2页，分别为10，13。
* allow\_empty\_first\_page=True： 默认允许第一页为空。

2.类方法：

*     Paginator.page\(number\)：根据参数number返回一个Page对象。（number为1的倍数）

3.类属型：

* Paginator.count：所有页面对象总数，即统计object\_list中item数目。当计算object\_list所含对象的数量时， Paginator会首先尝试调用object\_list.count\(\)。如果object\_list没有 count\(\) 方法，Paginator 接着会回退使用len\(object\_list\)。
* Pagnator.num\_pages:页面总数。
* pagiator.page\_range：页面范围，从1开始，例如\[1,2,3,4\]。

二. Page类的解释

 **通常不用手动创建Page对象，可以从Paginator.page\(\)来获得他们。**

```text
class Page(collections.Sequence):

    def __init__(self, object_list, number, paginator):
        self.object_list = object_list
        self.number = number
        self.paginator = paginator
```

1.类方法

* Page.has\_next\(\)  如果有下一页，则返回True。
* Page.has\_previous\(\) 如果有上一页，返回 True。
* Page.has\_other\_pages\(\) 如果有上一页或下一页，返回True。
* Page.next\_page\_number\(\) 返回下一页的页码。如果下一页不存在，抛出[I](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.InvalidPage)nvlidPage异常。
* Page.previous\_page\_number\(\) 返回上一页的页码。如果上一页不存在，抛出InvalidPage异常。
* Page.start\_index\(\) 返回当前页上的第一个对象，相对于分页列表的所有对象的序号，从1开始。比如，将五个对象的列表分为每页两个对象，第二页的[s](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.Page.start_index)tart\_index\(\)会返回3。
* Page.end\_index\(\) 返回当前页上的最后一个对象，相对于分页列表的所有对象的序号，从1开始。 比如，将五个对象的列表分为每页两个对象，第二页的[e](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.Page.end_index)nd\_index\(\) 会返回 4。

2.类属型

* Page.object\_list 当前页上所有对象的列表。
* Page.number 当前页的序号，从1开始。
* Page.paginator 相关的[P](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.Paginator)aginator对象。

三.非法页面处理

* InvalidPage\(Exception\): 异常的基类，当paginator传入一个无效的页码时抛出。

  [P](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.Paginator.page)aginator.page\(\)放回在所请求的页面无效（比如不是一个整数）时，或者不包含任何对象时抛出异常。通常，捕获InvalidPage异常就够了，但是如果你想更加精细一些，可以捕获以下两个异常之一：

* exception PageNotAnInteger，当向page\(\)提供一个不是整数的值时抛出。
* exception EmptyPage，当向page\(\)提供一个有效值，但是那个页面上没有任何对象时抛出。

       这两个异常都是[I](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.InvalidPage)nalidPage的子类，所以可以通过简单的except InvalidPage来处理它们。

四.使用Paginator

官方示例：

views.py:[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
from django.core.paginator import Paginator, EmptyPage, PageNotAnInteger
from django.shortcuts import render

def listing(request):
    contact_list = Contacts.objects.all()  # 获取所有contacts,假设在models.py中已定义了Contacts模型
    paginator = Paginator(contact_list, 25) # 每页25条

    page = request.GET.get('page')
    try:
        contacts = paginator.page(page) # contacts为Page对象！
    except PageNotAnInteger:
        # If page is not an integer, deliver first page.
        contacts = paginator.page(1)
    except EmptyPage:
        # If page is out of range (e.g. 9999), deliver last page of results.
        contacts = paginator.page(paginator.num_pages)

    return render(request, 'list.html', {'contacts': contacts})
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

list.html:[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
{% for contact in contacts %}
    {# Each "contact" is a Contact model object. #}
    {{ contact.full_name|upper }}<br />
    ...
{% endfor %}

<div class="pagination">
    <span class="step-links">
        {% if contacts.has_previous %}
            <a href="?page={{ contacts.previous_page_number }}">previous</a>
        {% endif %}

        <span class="current">
            Page {{ contacts.number }} of {{ contacts.paginator.num_pages }}.
        </span>

        {% if contacts.has_next %}
            <a href="?page={{ contacts.next_page_number }}">next</a>
        {% endif %}
    </span>
</div>
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

final:  根据官方示例代码结果如下，页面共有3页，首页时只有next选项，中间页时可选择previous 或 next，尾页时只有previous选项。

