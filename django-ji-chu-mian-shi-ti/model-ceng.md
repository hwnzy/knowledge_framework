# Model层

* **如何理解Django Migrations的作用？**

将模型类同步到数据库中。

```text
python manage.py makemigrations # 生成迁移文件，django 会在相应的 app 的migration文件夹下面生成 一个python脚本文件
python manage.py migrate # 同步到数据库中。根据 migration下面的脚本文件来生成数据表，django有一张django-migrations表，表中记录了已经执行的脚本，那么表中没有的就是还没执行的脚本，则执行migrate的时候就只执行表中没有记录的那些脚本
```

* 是**否有过手动编辑Migrations文件的经历，原因是什么，有哪些需要注意的？**

\*\*\*\*

* **介绍下ORM的概念。**

ORM 使用对象，封装了数据库操作，因此可以不碰 SQL 语言。开发者只使用面向对象编程，与数据对象直接交互，不用关心底层数据库。  
可以方便实现： 增加\(Create\)、读取查询\(Read\)、更新\(Update\)和删除\(Delete\)

* **如何理解ORM在Django框架中的作用？**

Django ORM用到三个类：Manager、QuerySet、Model。Manager定义表级方法（表级方法就是影响一条或多条记录的方法），我们可以以models.Manager为父类，定义自己的manager，增加表级方法；QuerySet：Manager类的一些方法会返回QuerySet实例，QuerySet是一个可遍历结构，包含一个或多个元素，每个元素都是一个Model 实例。

django中内嵌了ORM框架。实现了数据模型与数据库的解耦, 屏蔽了不同数据库操作上的差异.不在关注用的是mysql，oracle...等.通过简单的配置就可以轻松更换数据库, 而不需要修改代码。

1. 在settings.py中配置了数据库的连接配置信息
2. 指定调用MySQL的驱动程序PyMySQL
3. 在MySQL中创建数据库
4. 创建模型
5. 迁移

* **介绍下ORM下的N+1问题，发生的原因，以及解决方案。**

一条查询请求返回N条数据。外键查询，每一条记录的请求都需要对应查询一次数据库来获取关联外键的数据。

1. select\_related：解决外键产生的N+1问题，将关联的数据一次性查询出来
2. prefetch\_related：解决多对多关系的数据

* **介绍下Django中Model的作用。**

Model \(模型\) 简而言之即数据模型。模型不是数据本身（比如数据库里的数据），而是抽象的描述数据的构成和逻辑关系。每个Django model实际上是个类，继承了models.Model。每个Model应该包括属性，关系（比如单对单，单对多和多对多）和方法。当你定义好Model模型后，Django的接口会自动帮你在数据库生成相应的数据表\(table\)。这其实是MVC思想中的M的model层。

* **Model的Meta属性类有哪些可配置项，作用是什么，日常用到的。**

 给你的 Model 定义元数据，Model 元数据就是 ”不是一个字段的任何数据“

```text
db_table # 用于指定自定义数据库表名的
ordering # 这个字段是告诉Django模型对象返回的记录结果集是按照哪个字段排序的
verbose_name # 给你的模型类起一个更可读的名字
verbose_name_plural # 模型的复数形式是什么
```

* **介绍下QuerySet的作用，以及你常用的QuerySet优化措施？**

每个Model都有一个默认的manager实例，名为objects，QuerySet有两种来源：通过manager的方法得到、通过QuerySet的方法得到。mananger的方法和QuerySet的方法大部分同名，同意思，如filter\(\),update\(\)等，但也有些不同，如manager有create\(\)、get\_or\_create\(\)，而QuerySet有delete\(\)等，看源码就可以很容易的清楚Manager类与Queryset类的关系，Manager类的绝大部分方法是基于Queryset的。一个QuerySet包含一个或多个model instance。QuerySet类似于Python中的list，list的一些方法QuerySet也有，比如切片，遍历。

QuerySet是延迟获取的，只有当用到这个QuerySet时，才会查询数据库求值。另外，查询到的QuerySet又是缓存的，当再次使用同一个QuerySet时，并不会再查询数据库，而是直接从缓存获取（不过，有一些特殊情况）。一般而言，当对一个没有求值的QuerySet进行的运算，返回的是QuerySet、ValuesQuerySet、ValuesListQuerySet、Model实例时，一般不会立即查询数据库；反之，当返回的不是这些类型时，会查询数据库。下面介绍几种（并非全部）对QuerySet求值的场景。

* 可切片
* 可迭代
* 惰性查询
* 缓存机制
* exists\(\)与iterator\(\)方法



* **介绍下Pagination的使用。**

Django自身提供了一些类来实现管理分页，数据被分在不同的页面中，并带有“上一页/下一页”标签。这个类叫做Pagination，其定义位于 django/core/paginator.py 中

* **介绍下Model中Field的作用。**

在model中添加字段的格式一般为： field\_name = field\_type\(\*\*field\_options\)

* **如何定制Manager？什么场景下需要定制？**

Manager是django模型进行数据库查询操作的接口。Django 应用的每个模型都拥有至少一个管理器。

* 改变查询的结果集

```python
class BookInfoManager(models.Manager):
	"""图书模型管理器类"""
	# 改变查询的结果集
	def all(self):
		# 1.调用父类的all, 获取所有数据
		books = super().all()
		# 2. 返回的books是QuerySet集合，还可以继续使用所有查询
		books = books.filter(isDelete=False)
		# 返回books
		return books


class BookInfo(model.Model):
	"""图书模型类"""
	.... # 省略图书模型类属性
	objects = BookInfoManager() # 自定义一个BookInfoManage管理器类对象
```

* 添加额外的方法

管理器类中可以自定义一个方法帮我们操作模型类对应的数据表

案例： 在BookInfoManage类中添加create\_book\(参数\)方法，该方法内部封装实例化一个属性，给属性赋值，然后实例外对象.save\(\), 保存数据到数据库。封装好以后，我们就可以使用BookInfo.objects.create\_book\(参数\)来添加数据。

* **Raw SQL的效率跟ORM的效率是否进行过对比，结果如何？如何理解这种差异？**

\*\*\*\*

* **Django内置提供的权限逻辑是什么，以及其粒度？**

Django用permission对象存储权限项，每个model默认都有三个permission，即add model, change model和delete model。

