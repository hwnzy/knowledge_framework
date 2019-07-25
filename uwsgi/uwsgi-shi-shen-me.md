# UWSGI是什么

## uWSGI是什么

> uWSGI是一个web服务器，它实现了WSGI协议、uwsgi、http等协议。
>
> uwsgi协议是一个uWSGI服务器自有的协议，它用于定义传输信息的类型，每个uwsgi packet前4byte为传输信息类型描述。
>
> Nginx中HttpUswgiModule的作用是与uWSGI服务器进行交换。

## 为什么有了uWSGI还需要nginx?

> 因为Nginx具有优秀的静态内容处理能力，将动态内容转发给uWSGI服务器，这样可以达到很好的客户端响应



