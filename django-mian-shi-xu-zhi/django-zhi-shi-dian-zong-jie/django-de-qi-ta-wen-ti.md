# Django的其他问题

## Django本身提供了runserver，为什么不能用来部署？

> runserver方法主要在测试和开发中调试Django，其使用Django自带的WSGI Server运行，runserver开启方式为单线
>
> uWSGI是一个Web服务器，它实现了WSGI协议、uwsgi、http等协议。uwsgi是一种通信协议，而uWSGI是实现uwsgi协议和WSGI协议的Web服务器，uWSGI具有超快的性能、低内存占用和多app管理等优点，并且搭配着Nginx就是一个生产环境了，能够将用户访问请求与应用app隔离开，实现真正的部署。相比来讲，支持的并发量更高，方便管理多进程，发挥多核的优势，提升性能。

## Django对http请求的执行流程

1. 在接收一个HTTP请求之前的准备

   > 启动一个支持WSGI网关协议的服务器监听端口等待外界的HTTP请求，比如Django自带的WSGI server 或者 uWSGI服务器
   >
   > 服务器根据WSGI协议指定相应的Handler来处理Http请求，并且初始化该Handler，在Django框架中由框架自身负责实现这一个Handler

2. 此时服务器已经处于监听状态，可以接收外界的HTTP请求
3. 当一个HTTP请求到达服务器的时候

   > 服务器根据WSGI协议从HTTP请求中提取必要的参数组成一个字典（environ）并传入Handler中进行处理。
   >
   > 在Handler中对已经符合WSGI协议标准规定的HTTP请求进行分析，比如加载Django提供的中间件，路由分配，调用路由匹配的视图等。
   >
   > 返回一个可以被浏览器解析的符合HTTP协议的HttpResponse

![Django&#x5BF9;http&#x8BF7;&#x6C42;&#x7684;&#x6267;&#x884C;&#x6D41;&#x7A0B;](../../.gitbook/assets/image%20%2854%29.png)

## Django下的（内建）缓存机制

> Django根据设置的缓存方式，浏览器第一次请求时，cache会缓存单个变量或整个网页等内容到硬盘或内存中，同时设置response头部，当浏览器再次发起请求时，附带f-Modified-Since请求时间到Django，Django发现f-Modified-Since会先去参数之后，会与缓存中的过期时间相比较，如果缓存时间比较新，则会重新请求数据，并缓存起来然后返回response给客户端，如果缓存没有过期，则直接从缓存中提取数据，返回给response给客户端。

## Django中如何加载初始化数据

> Django在创建对象时调用save\(\)方法后，ORM框架会把对象的属性转换为写入数据库中，实现对数据库的初始化。通过操作对象，查询数据库，将查询集返回给视图函数，通过模板语言展现在前端页面。

## Django中哪里用到了线程，哪里用到了协程，哪里用到了进程

> 1. django中耗时任务使用多线程发送邮件.send\_mail\(\)、异步任务.celery消息队列
> 2. django原生为单线程程序，当第一个请求没有完成，第二个请求就会被阻塞
> 3. django自带的development server为多线程模式

