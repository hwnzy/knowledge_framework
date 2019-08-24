# 整体结构

## **整体结构**

* **如何理解设计模式中的MVC模式，你日常中怎么使用这种模式？**

MVC 设计模型是一种使用 Model View Controller（ 模型-视图-控制器）设计创建 Web 应用程序的模式。

![](../.gitbook/assets/image%20%2857%29.png)

1. M全拼为Model，主要封装对数据库层的访问，对数据库中的数据进行增、删、改、查操作。
2. V全拼为View，用于封装结果，生成页面展示的html内容。
3. C全拼为Controller，用于接收请求，处理业务逻辑，与Model和View交互，返回结果

* **如何理解Django中的MTV模型？**

![](../.gitbook/assets/image%20%283%29.png)

1. M全拼为Model，与MVC中的M功能相同，负责和数据库交互，进行数据处理。
2. V全拼为View，与MVC中的C功能相同，接收请求，进行业务处理，返回应答。
3. T全拼为Template，与MVC中的V功能相同，负责封装构造要返回的html。

* **介绍下Django中你熟悉的模块有哪些，分别作用是什么？**

```python
单词理解
urls 链接
view 视图
shortcuts 捷径
contrib 构建
decorators 装饰
core 核心
uploadedfile 上传文件

from Django.conf import settings
1. urls相关操作
from django.urls import path, re_path(使用正则时使用), include
from django.urls import reverse  // 注意reverse 和另一个reversed区别。前者要明确导入通过名称解析出地址，后者是built-in内置不用导入；两者功能也不一。
2. HttpResponse生成
from django.shortcuts import render, HttpResponse, redirect
from django.http import JsonResponse // 响应一个content-type：text/json 返回一个json响应报文,相应的浏览器端也不用在对json反解
3. 组件auth
from django.contrib import auth  //contrib 意味：构件
from django.contrib.auth.models import User 
from django.contrib.auth.decorators import login_required
4. 组件forms
from django import forms
from django.forms import widgets
from django.core.exceptions import ValidationError  // django的异常定义都在django.core.exceptions模块中，该异常用于自定义钩子。
from django.forms import ModelForm  // 如果一个form的字段数据是被用映射到一个django models.那么一个ModelForm可以帮助你节约很多开发时间。因为它将构建一个form实例，连同构建适当的field和field attributes，利用这些构建信息，都来自一个Model class. 
from django.core.files.uploadedfile import SimpleUploadedFile
```

* **如何看待Django自带的Admin，以及说说你的使用经验。**

Django自带的Admin当我们有了数据表和Model相当于有了一个管理后台，还包括权限控。其可以在各个app的admin.py文件中进行控制。

```python
from django.contrib import admin
from blog.models import Blog
# 1. 装饰器方式应用注册
@admin.register(Blog)
class BlogAdmin(admin.ModelAdmin):
    # 2. 记录列表是我们打开后台管理进入某个应用看到的界面
    # listdisplay设置要显示在列表中的字段
    list_display=('id', 'caption', 'author', 'publish_time')
    # list_per_page设置每页显示多少条记录，默认是100条
    list_per_page = 50
    # ordering设置默认排序字段，负号表示降序排序
    ordering = ('-publish_time',)
    # list_editable 设置默认可编辑字段
    list_editable = ['machine_room_id', 'temperature']
    # 设置哪些字段可以点击进入编辑界面
    list_display_links = ('id', 'caption')
    # fk_fields 设置显示外键字段
    fk_fields = ('machine_room_id',)
    # 3. 筛选器
    list_filter =('trouble', 'go_time', 'act_man__user_name', 'machine_room_id__machine_room_name') # 过滤器 
    search_fields =('server', 'net', 'mark') # 搜索字段
    date_hierarchy = 'go_time'    # 详细时间分层筛选
    # 4. 编辑页面设置
    # 怎么用fields和exclude。用得比较多的是fieldsets。该设置可以对字段分块，看起来比较整洁
    fieldsets = (
        ("base info", {'fields': ['caption', 'author', 'tags']}),
        ("Content", {'fields':['content', 'recommend']})
    )
    　    
# admin界面汉化，在settings.py中
LANGUAGE_CODE = 'zh-hans'
TIME_ZONE = 'Asia/Shanghai'
```

* **如何理解WSGI的作用？**

**WSGI**：全称是Web Server Gateway Interface，WSGI不是服务器，python模块，框架，API或者任何软件，只是一种规范，描述web server如何与web application通信的规范。

**uwsgi**：与WSGI一样是一种协议，是uWSGI服务器的独占协议，用于定义传输信息的类型\(type of information\)，每一个uwsgi packet前4byte为传输信息类型的描述，与WSGI协议是两种东西，据说该协议是fcgi协议的10倍快。

**uWSGI**：是一个web服务器，实现了WSGI协议、uwsgi协议、http协议等。

* **如何自己实现WSGI协议？**

\*\*\*\*

* **为什么正式部署时不要开启`DEBUG = True`配置？**

这样的话当访问接口存在错误会直接展示项目的所有配置信息，那么如果我们部署在正式环境，这样过于危险

