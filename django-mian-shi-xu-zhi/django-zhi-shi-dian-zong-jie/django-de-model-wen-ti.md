# Django的model问题

## 关系型数据库的关系包含哪些类型?

* ForeignKey：一对多，将字段定义在多的一端
* ManyToManyField：多对多，将字段定义在两端中
* OneToOneField：一对一，将字段定义在任意一端中

## 查询集返回列表的过滤器有哪些

* all\(\)：返回模型类所有的数据
* filter\(\)：返回满足条件的数据
* exclude\(\)：返回满足条件之外的数据，相当于sql语句中where部分的not关键字
* order\_by\(\)：排序

## 判断查询集是否有数据

> exists\(\)：判断查询集中是否有数据，有返回True，没有返回False

## QuerySet的get和filter方法的区别

* 【输入参数】
  * get参数只能是model中定义的那些字段，只支持严格匹配
  * filter的参数可以是字段，也可以是扩展的where查询关键字，如in, like等
* 【返回值】
  * get返回值是一个定义的model对象
  * filter返回值是一个新的QuerySet对象，然后可以对QuerySet在进行查询返回新的QuerySet对象，支持链式操作QuerySet一个集合对象，可以使用迭代或者遍历，切片，但是不等同于list类型
* 【异常】
  * get只有一条记录返回的时候才正常，也就说明get的查询字段必须是主键或者唯一约束的字段，当返回多条记录或者是没有找到记录的时候会抛出异常，MultipleObjectsReturned和DoesNotExist。
  * filter有没有匹配记录都可以

## model的SlugField类型字段有什么用途

> SlugField字段是将输入内容中的空格都替换为'-'之后保存，Slug是一个新闻术语，通常是某些东西的短标签。一个slug只能包含字母、数字、下划线或者是连字符，通常作为短标签。通常用来放在URL里

