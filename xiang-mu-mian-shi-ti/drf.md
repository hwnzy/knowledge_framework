# DRF

## 后台项目需求说明

* 创建管理员：python manage.py createsuperuser
* 用户名：admin123
* 密码：python123

## 管理员登录

#### 浏览器的同源策略

* 在浏览器中，用js发起ajax请求，请求另外一个服务器时，浏览器认为访问不同的源，则禁止访问
* 浏览器向不同源的服务器发起一个options请求，根据响应报文来确定是否继续发起真实的请求

#### 跨域CORS

* 跨域：a域名下的js访问b域名下的接口
* 未配置cors时,options请求的响应报文头

```text
HTTP/1.0 404 Not Found
Date: Tue, 04 Jun 2019 07:11:39 GMT
Server: WSGIServer/0.2 CPython/3.5.2
Content-Length: 8332
Content-Type: text/html
X-Frame-Options: SAMEORIGIN
```

* 配置cors时,options请求的响应报文头

```text
HTTP/1.0 200 OK
Date: Tue, 04 Jun 2019 07:53:24 GMT
Server: WSGIServer/0.2 CPython/3.5.2
Content-Type: text/html; charset=utf-8
Vary: Origin

Access-Control-Allow-Origin: http://127.0.0.1:8080
Access-Control-Allow-Methods: DELETE, GET, OPTIONS, PATCH, POST, PUT
Access-Control-Allow-Headers: accept, accept-encoding, authorization, content-type, dnt, origin, user-agent, x-csrftoken, x-requested-with
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 86400
```

* 浏览器检测到跨域请求时，会发起options请求，浏览器接收到响应后，会根据响应报文头中的信息进行判断，决定是否继续发起后续请求，判断标准为Access-Control-Allow-...
* 浏览器的同源策略是什么意思？
  * 当浏览器检测到js的ajax请求跨域时，会阻止这个请求
* 浏览器在同源策略的机制下做了什么？
  * 发起options请求
  * 根据响应报文头进行验证
  * 如果允许则发起后续的请求
  * 如果不允许则停止请求
* 服务器应该如何响应跨域请求？
  * 响应options请求，在响应报文头中加入信息
  * Access-Control-Allow-_\*\*_

#### JWT

* 官方网站：[https://jwt.io/](https://jwt.io/)
* user\_id=1,密钥=123

```text
HaXuqZKNsHZwO9YeAUJA8WizoGaxsAVPp05YiU6lerg
```

* user\_id=2,

```text
密钥=abc===》F0QOpCT8DoE78KGB9CtSLbSVx1dYTHvpvRkp7fr8gKg
密钥=123===》vvN0kOZLLa6HZerGVfa2dtoDxDDSq3ns2o6lKIAjcV4
```

* jwt如何保证数据安全有效：密钥
* 验证：服务器接收到jwt后，会取出header、payload，与密钥一起进行计算，得到新的签名，与接收到的签名进行对比，如果相同，则认为身份有效，如果不同，则认为用户未登录
* jwt是什么？
  * json web token
  * 构成：header、payload、signature
  * header===&gt;加密算法、类型，固定不变
  * payload===&gt;用户身份信息
  * signature===&gt;加密算法\(header.payload,密钥\)
* 如何保证数据安全有效？
  * 接收请求报文中的jwt字符串，分别取出header、payload、signature
  * 以header、payload、密钥计算，得到新的signature2
  * signature==signature2则认为身份有效，解密payload部分，获得用户信息
  * 如果不相等，则认为身份无效，处于未登录

#### drf-jwt

* 安装包
* 配置
* 注册路由
* 问题1：响应体中只有token，没有用户名
* 问题2：jwt的载荷中不包含email
* 问题3：jwt的视图如何进行验证，需要is\_staff=True
* 思路：读官方文档，读源码

## 总结

#### 重要知识点

* 跨域问题及解决方案
* jwt与视图实现

#### 作业

* 完成今天代码
* 总结新知识点

#### 大纲要求

* 能够知道如何解决cors跨域问题

```text
响应options请求，在请求报文头中加入信息：Access-Control-Allow-****
```

* 知道JWT的三部分构成

```text
header=======》类型，加密算法
payload======》用户身份信息
signature====》用于验证jwt的有效性
```

* 能够实现登录的接口设计

```text
请求方式：post
地址：自己决定，前端调用
参数：username，password
响应：user_id,username,token
```



## 复习

* 浏览器同源策略与跨域解决
  * 浏览器的同源策略：浏览器中的js发起的ajax请求，在访问其它域名时，会被阻止
  * 浏览器发起options方式的请求，根据响应报文进行判断，如果允许则继续请求，如果不允许则停止请求
  * 判断标准：在响应报文头中包含Access-Control-Allow-_\*\*_
  * 解决：在视图中响应options请求，在响应报文头中添加信息
  * 实现：cors，安装，配置白名单，中间件
* 状态保持新方案：token====&gt;jwt
  * 实现原理：
    * 1.服务器生成token，返回给客户端
    * 2.将token包含在请求报文头中
    * 3.从请求报文头中接收token，进行验证
  * 最常用的表示方式：jwt，json web token
  * 构成：header、payload、signature
* drf-jwt实现登录
  * 安装
  * 配置
  * 登录视图

## 今天知识点

* 自定义登录
* 数据统计

## 自定义登录

* 问题1：载荷中想加入手机号，如何实现
  * 阅读官方文档，参考源码，JWT\_PAYLOAD\_HANDLER
* 问题2：在响应体中加入用户编号、用户名
  * JWT\_RESPONSE\_PAYLOAD\_HANDLER
* 问题3：在登录验证时，加入is\_staff=1的条件
  * 1.阅读jwt登录视图源码，找到authenticate\(\)方法
  * 2.调用自定义认证后端进行验证
  * 3.改写自定义认证后端，判断是否后台调用

## 数据统计

* 图书-英雄为一对多，在英雄中定义外键hbook=models.FK\(图书,related\_name='heros'\)

```text
如果已知英雄编号为1，查找对应的图书：
方案一：hero=HeroInfo.objects.get(pk=1)
    hero.hbook===>图书对象
    hero.hbook_id===>图书对象的主键
方案二：BookInfo.objects.filter(heros__id__gt=10)

如果已知图书名称包含“西”，查询对应的英雄：
方案一：book=BookInfo.objects.get(bitlte__contains='西')
    book.heros.all()
方案二：HeroInfo.objects.filter(hbook__btitle__contains='西')
```

* echarts

## 总结

#### 重要知识点

* 自定义登录：
  * 阅读drf-jwt官方文档
  * 阅读源码
  * 1.自定义载荷
  * 2.自定义响应体
  * 3.自定义验证方法
* 统计
  * 关系属性查询

#### 作业

* 完成今天代码
* 阅读drf-jwt文档

#### 大纲要求

* 能够实现JWT返回数据方法的改写

```text
JWT_RESPONSE_PAYLOAD_HANDLER===》方法
接收user、token、request作为参数
返回字典，指定token、id、username
```

* 能够知道JWT实现用户验证的形式

```text
drf-jwt包中，登录视图并没有直接进行验证，而是调用django的认证方法，django的认证方法会从配置文件中读取认证后端，调用认证后端的authenticate()方法进行验证
```

* 能够理解用户总数统计的代码逻辑

```text
User.objects.filter(***).count()
```

## 复习

* 自定义登录
  * 使用drf-jwt提供的登录视图
  * 自定义载荷
  * 自定义响应体
  * 自定义验证：读源码
* 数据统计
  * 模型类.objects.filter\(....\).count\(\)
  * 在查询语句中使用关系属性：今天下过订单的用户总数

```text
结果：用户User.objects.filter(...).count()
条件：用户今天下过订单
    orders__create_time__gte=today
```

## 今天知识点

* 用户管理
* 规格管理

## 用户管理

* 在类视图的实例方法中，通过self.request可以获得请求对象
* 分页：
  * 1.定义分页类型，继承自drf的分页类型
  * 2.重写属性，指定页大小、页大小参数键、最大页大小
* 阅读drf分页类型的源码，知道：使用django的Paginator、Page实现分页功能
* 自定义分页的响应字典
* 创建：
  * 视图类继承自CreateAPIView
  * 在序列化器类中：验证方法,create\(\)方法

## 规格管理

* 查询多个
* 增加
* 查询:SPU的基本信息，用于添加规格时选择
* 视图集封装所有操作

## 总结

#### 重要知识点

* 用户的查询、分页、增加
* 商品规格的crud
* 分页：
  * 实现：继承自drf的分页类型，重写属性、方法
  * 原理：drf的分页是如何实现的？
  * 答：通过阅读源码，调用django的分页类型，Paginator，Page
* 用户的创建：没有继承自ModelSerializer
  * 原因：密码需要加密再保存，而不能直接保存

#### 作业

* 完成今天代码
* 适当阅读源码\(慢慢练习\)

#### 大纲要求

* 能够理解获取所有用户信息的代码逻辑

```text
1.定义视图类，继承自ListAPIView
2.定义序列化器类，继承自Serializer
3.定义分页类型，重写属性、方法
```

* 能够知道改写分页返回结果的方法

```text
在分页类型中，重写get_paginated_response()方法
返回Response对象，指定字典
```

* 在保存用户功能中，能够知道改写序列化器中create方法的原因

```text
创建用户对象，需要将密码加密再保存，而不是直接将密码值保存
```

## 复习

* 用户管理
  * 查询：ListAPIView,过滤,分页
  * 创建：CreateAPIView
  * 重写序列化器的create\(\)方法：创建用户对象需要将密码加密再保存
    * create\_user\(\)=====&gt;create\(\)
* 规格管理
  * 完成的查询、增加、修改、删除
  * ModelViewSet
  * ModelSerializer

## 反馈

* _\*_    阅读文档有什么技巧吗？提升英语水平？请老师分享下心得 O\(∩\_∩\)O
  * 基础：英语
  * 多读
  * 使用一个包时，官方文档可以告诉我们如何使用包中的类、属性、方法
  * 辅助读源码：知道实现原理，按照代码的执行顺序

## 今天知识点

* SKU
* 图片

## SKU

#### 查询多个

* 视图
* 过滤
* 分页

#### 增加

* 查询第三级分类
* 查询SPU
* 查询指定SPU的规格及对应的选项
  * self.request===&gt;请求对象
  * self.args====&gt;路由中提取的位置参数，列表
  * self.kwargs====&gt;路由中提取的关键字参数，字典
* 保存
  * 重写序列化器的create\(\)方法：只保存库存商品对象，未保存规格选项数据
  * 事务
  * celery生成静态页面
* 注意：在配置文件中修改模板语言为jinja2

## 总结

#### 重要知识点

* sku查询：
  * 过滤：filter\(Q\(\)\|Q\(\)\)
  * 分页
* sku创建
  * 查询第三级分类：subs\_\_isnull=True
  * 查询spu
  * 根据spu\_id查询规格及选项
  * 重写序列化器的create\(\)方法：创建sku，遍历创建规格选项对象
  * 事务
  * 为sku生成静态页面：celery任务

#### 作业

* 完成今天代码

#### 大纲要求

* 能够理解获取sku商品三级分类的代码逻辑

```text
条件：三级分类没有子级数据
****.objects.filter(subs__isnull=True)
```

* 能够理解获取sku表规格和规格选项数据的业务逻辑

```text
在库存商品规格选项表中定义了外键
在sku对象中有隐藏的关系属性specs===>访问相关的规格选项数据
```

* 能够知道改写sku序列化器create方法的原因

```text
1.只能创建sku对象，无法创建规格选项对象===》重写后可以创建sku对象、规格选项对象
2.创建成功后生成静态文件
```

## 复习

* SKU查询
  * 过滤：Q\(name**contains=...\)\|Q\(caption**contains=....\)
  * 分页
* SKU创建
  * 查询第三级分类:subs\_\_isnull=True
  * 查询SPU
  * 查询指定SPU的规格与选项
  * 重写序列化器的create\(\)方法：创建sku对象，创建规格选项对象，生成静态页面

## 反馈

* _\*_    自从用了序列化器，断点不会打了。一脸懵……
  * 想看哪里，点哪里
* _\*_    router = DefaultRouter\(\) router.register\('goods/specs', specs.SpecModelViewSet, base\_name='specs'\) router.register\('skus', skus.SkuModelViewSet, base\_name='skus'\) urlpatterns += router.urls 老师，DefaultRouter，类视图必须继承ModelViewsSet吗？
  * 不是，ViewSet
* 李少勇    老师可以再讲一下表结构中的关系属性吗？对于这块的查询理解不是很透彻，谢谢
  * BookInfo====&gt;heros
  * HeroInfo===&gt;hbook=FK\(related\_name=heros\)
* 许梦超    老师可以教一下debug模式下序列化器中怎么打断点
  * 同上

## 今天知识点

* SKU修改、删除
* 图片管理

## SKU管理

#### 修改

* 保存
  * 重写序列化器的update\(\)方法
  * 事务
  * celery生成静态页面

#### 删除

* 默认实现

```text
    #/skus/(?P<pk>\d+),***.as_view({'delete':'destroy'})/
    def destroy(self, request, *args, **kwargs):
        instace=self.get_object()
        instace.is_delete=True
        instace.save()
```

## 图片处理

* 复习fastdfs
* 安装包
  * pip install fdfs\_client-py-master.zip
* 上传
  * upload\_by\_filename\(\)===&gt;本地文件名
  * upload\_by\_buffer\(\)====》文件的二进制数据
* 修改
  * modify\_by\_filename\(\)===&gt;本地文件名为参数
  * modify\_by\_buffer\(\)====》文件的二进制数据为参数
  * 结论：提供的修改方法无效，抛异常
  * 实现思路：先删除，后上传
* 删除
  * delete\_file\(\)====&gt;storage中的文件名为参数

## 图片

#### 查询

* 查询所有图片

#### 增加

* 查询简单的SKU数据
* 重写视图的create\(\)方法
* 上传图片
* 创建

#### 修改

* 重写视图的update\(\)方法
* 修改文件
* 修改

#### 删除

* 重写视图的destroy\(\)方法
* 删除文件
* 删除

## 总结

#### 重要知识点

* sku
  * 修改：重写序列化的update\(\)方法，修改sku，删除旧的规格选项，创建新的规格选项，生成静态文件
  * 删除：设置外键的删除方式为级联
* 图片
  * 查询：分页
  * 增加：
    * 在视图集类中，定义方法，使用@action装饰器为方法生成路由规则
    * 重写视图的create\(\)方法：上传图片，创建对象
  * 修改：
    * 重写视图的update\(\)方法：删除图片，上传图片，修改对象
  * 删除：
    * 重写视图的destroy\(\)方法：删除图片，删除对象

#### 作业

* 完成今天代码
* 复习drf第1天的代码
* 总结drf与项目

#### 大纲要求

* 能够知道fastDFS客户端的使用

```text
client=Fdfs_client(配置文件)
上传：client.upload_by_buffer(文件的二进制数据)
删除：client.delete_file(storage中文件的名称)
修改：删除，上传
```

* 能够改写图片保存和更新的业务逻辑

```text
重写视图类的create()、update()方法
```

* 能够知道更新业务中改写update方法的原因

```text
sku：修改sku，删除原规格选项，创建新规格选项，生成静态文件
图片：删除旧图片，上传新图片，修改图片对象
```



## 复习

* SKU
  * 修改：重写序列化器的update方法，修改sku对象，保存规格选项数据，生成静态文件
  * 删除：默认实现
* 图片上传
  * client=Fdfs\_client\(配置文件\)
  * 上传client.upload\_by\_buffer\(文件的二进制数据\)
  * 删除client.delete\_file\(storage中文件的名称\)
* 图片crud
  * 重写视图集的create\(\)、update\(\)、destroy\(\)：除了数据操作外，还需要图片文件操作

## 反馈

* _\*_    get\_object\(\)获取对象，可不可以改为：先由self.arg或self.kwargs获取到路径参数pk,再通过参数查询得到模型类对象
  * 可以
  * 从关键字参数中获取pk，在查询集中通过pk查找一个对象

## 订单管理

#### 查询多条

* ListModelMixin

#### 查询一条

* RetrieveModelMixin
* ReadOnlyModelViewSet

#### 修改状态

* UploadModelMixin===&gt;修改所有属性
* 只修改status属性===》定义方法，添加action装饰器

## 系统管理

#### 权限管理方式

* permission
* group
* user
* ManyToMany多对多
* 方式一：权限===》组====》用户
* 方式二：权限===》用户

#### 权限

* 查询多个，分页
* 当定义模型类迁移时，会生成表，同时在权限表中添加3条数据，分别为：增加、修改、删除
* 视图类：ListAPIView
* 序列化类：ModelSerializer

#### 组

* 完成的crud
* 视图类：ModelViewSet
* 序列化类：ModelSerializer
* 添加：组表中创建数据，组-权限关系表中创建数据
  * 问题：为什么这里可以自动添加数据，而sku时需要重写create，手动添加规格数据
  * 回答：模型类序列化器的create\(\)、update\(\)默认实现是，创建指定模型类对象，为多对多的关系添加数据

#### 管理员

* 完成的crud
* 重写get\_serializer\_class，通过self.action判断，返回不同的序列化器类型
* self.action表示当前执行方法的名称
* 默认实现不完美：
  * 没有设置管理员is\_staff=True
  * 密码未加密

## 总结

#### 重要知识点

* 订单的查询多个、查询一个、修改status属性
* django自带权限管理
  * permission
  * group
  * user
* 查询组时，重写get\_serializer\_class方法，根据self.action返回不同序列化器类型
* 重写管理员序列化器的create\(\)、update\(\)方法，进行密码加密
* 说明：模型类序列化器的create\(\)、update\(\)方法：
  * 1.创建指定模型类对象
  * 2.为多对多关系属性赋值

#### 作业

* 完成今天代码
* 练习drf第1天的代码
* 总结drf框架与项目

#### 大纲要求

* 能够理解订单表详情数据获取的业务逻辑

```text
OrderInfo
    skus===>所有订单商品
        sku===>库存商品对象
```

* 能够理解修改订单状态的业务逻辑

```text
在视图集中定义方法status，添加装饰器action
为什么不使用默认的update()方法：只修改status属性，前端没有提供其它数据
```

* 能够理解管理员用户的增删改查业务逻辑

```text
ModelViewSet
ModelSerializer
重写序列化器的create()、update()方法
    1.指定管理员
    2.密码加密
```

