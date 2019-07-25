# Django的基本概念

## Django的认识

> Django是走大而全的方向，其最出名的是其全自动化的管理后台，只需要使用ORM做简单的对象定义就可以自动生成数据库结构以及全功能的管理后台。
>
> Django内置的ORM跟框架内的其他模块耦合程度高。
>
> 应用程序必须使用Django内置的ORM，否则就不能享受到框架内提供的种种基于的ORM的便利。
>
> Django的卖点是超高的开发效率，其性能扩展有限，正常并发量不过10000，采用Django的项目，在流量达到一定规模后，都需要对其进行重构，才能满足性能要求。比如把笨重的框架给拆掉自己写socket实现http通信，底层用纯C，C++写提升效率，自己编写封装与数据库交互的框架，因为ORM使用外键来联系表与表之间的查询。
>
> Django适用的是中小型的网站，或者是作为大型网站快速实现产品雏形的工具。
>
> Django模板设计哲学是彻底将代码、样式分离，杜绝在模板中编码和处理数据的可能。

## Django命令

创建项目和应用的命令？

> django-admin startproject 项目名
>
> python manage.py startapp 应用名

生成迁移文件和执行迁移文件的命令是什么？

> python manage.py makemigrations
>
> python manage.py migrate
>
> python manage.py makemigrations app\_name
>
> python manage.py migrate app\_name
>
> python manage.py migrate app\_name 0004  \# 回退到迁移文件0004\_xxx.py
>
> python manage.py showmigrations  \# 查看迁移文件的执行状态

## 



