# uWSGI

### [wsgi协议/uwsgi协议/wsgi协议实现过程](https://www.cnblogs.com/ray-mr-huang/p/9077678.html)

WSGI：全称是`Web Server Gateway Interface`，`WSGI`不是服务器，`python`模块，框架，`API`或者任何软件，只是一种规范，描述`web server`如何与`web application`通信的规范。`server`和`application`的规范在[PEP 3333](https://link.jianshu.com/?t=https://www.python.org/dev/peps/pep-3333/)中有具体描述。要实现WSGI协议，必须同时实现web server和web application，当前运行在`WSGI`协议之上的`web`框架有`Bottle`, `Flask`, `Django`。 

uwsgi：与`WSGI`一样是一种通信协议，是`uWSGI`服务器的独占协议，用于定义传输信息的类型\(`type of information`\)，每一个`uwsgi packet`前`4byte`为传输信息类型的描述，与WSGI协议是两种东西，据说该协议是`fcgi`协议的10倍快。 

uWSGI：是一个`web`服务器，实现了`WSGI`协议、`uwsgi`协议、`http`协议等。

`WSGI`协议主要包括`server`和`application`两部分：

* `WSGI server`负责从客户端接收请求，将`request`转发给`application`，将`application`返回的`response`返回给客户端；
* `WSGI application`接收由`server`转发的`request`，处理请求，并将处理结果返回给`server`。`application`中可以包括多个栈式的中间件\(`middlewares`\)，这些中间件需要同时实现server与application，因此可以在WSGI服务器与WSGI应用之间起调节作用：对服务器来说，中间件扮演应用程序，对应用程序来说，中间件扮演服务器。

`WSGI`协议其实是定义了一种`server`与`application`解耦的规范，即可以有多个实现`WSGI server`的服务器，也可以有多个实现`WSGI application`的框架，那么就可以选择任意的`server`和`application`组合实现自己的`web`应用。例如`uWSGI`和`Gunicorn`都是实现了`WSGI server`协议的服务器，`Django`，`Flask`是实现了`WSGI application`协议的`web`框架，可以根据项目实际情况搭配使用。

![](../.gitbook/assets/image%20%288%29.png)

像`Django`，`Flask`框架都有自己实现的简单的`WSGI server`，一般用于服务器调试，生产环境下建议用其他`WSGI server`。

### WSGI协议的实现

以`Django`为例，分析一下`WSGI`协议的具体实现过程。

#### django WSGI application

`WSGI application`应该实现为一个可调用对象，例如函数、方法、类\(包含\`call\`方法\)。需要接收两个参数：

* 一个字典，该字典可以包含了客户端请求的信息以及其他信息，可以认为是请求上下文，一般叫做`environment`（编码中多简写为`environ`、`env`）
* 一个用于发送HTTP响应状态（`HTTP status` ）、响应头（`HTTP headers`）的回调函数

通过回调函数将响应状态和响应头返回给`server`，同时返回响应正文\(`response body`\)，响应正文是可迭代的、并包含了多个字符串。下面是`Django`中`application`的具体实现部分：

```python
class WSGIHandler(base.BaseHandler):
    initLock = Lock()
    request_class = WSGIRequest

    def __call__(self, environ, start_response):
        # 加载中间件
        if self._request_middleware is None:
            with self.initLock:
                try:
                    # Check that middleware is still uninitialized.
                    if self._request_middleware is None:
                        self.load_middleware()
                except:
                    # Unload whatever middleware we got
                    self._request_middleware = None
                    raise

        set_script_prefix(get_script_name(environ))
        # 请求处理之前发送信号
        signals.request_started.send(sender=self.__class__, environ=environ)
        try:
            request = self.request_class(environ)
        except UnicodeDecodeError:
            logger.warning('Bad Request (UnicodeDecodeError)',
                exc_info=sys.exc_info(),
                extra={'status_code': 400,})
            response = http.HttpResponseBadRequest()
        else:
            response = self.get_response(request)

        response._handler_class = self.__class__

        status = '%s %s' % (response.status_code, response.reason_phrase)
        response_headers = [(str(k), str(v)) for k, v in response.items()]
        for c in response.cookies.values():
            response_headers.append((str('Set-Cookie'), str(c.output(header=''))))
        # server提供的回调方法，将响应的header和status返回给server
        start_response(force_str(status), response_headers)
        if getattr(response, 'file_to_stream', None) is not None and environ.get('wsgi.file_wrapper'):
            response = environ['wsgi.file_wrapper'](response.file_to_stream)
        return response
```

可以看出`application`的流程包括:

* 加载所有中间件，以及执行框架相关的操作，设置当前线程脚本前缀，发送请求开始信号；
* 处理请求，调用`get_response()`方法处理当前请求，该方法的的主要逻辑是通过`urlconf`找到对应的`view`和`callback`，按顺序执行各种`middleware`和`callback`。
* 调用由`server`传入的`start_response()`方法将响应`header`与`status`返回给`server`。
* 返回响应正文

#### django WSGI Server

负责获取`http`请求，将请求传递给`WSGI application`，由`application`处理请求后返回`response`。以`Django`内建`server`为例看一下具体实现。  
通过`runserver`运行`django`项目，在启动时都会调用下面的`run`方法，创建一个`WSGIServer`的实例，之后再调用其`serve_forever()`方法启动服务。

```python
def run(addr, port, wsgi_handler, ipv6=False, threading=False):
    server_address = (addr, port)
    if threading:
        httpd_cls = type(str('WSGIServer'), (socketserver.ThreadingMixIn, WSGIServer), {})
    else:
        httpd_cls = WSGIServer
    # 这里的wsgi_handler就是WSGIApplication
    httpd = httpd_cls(server_address, WSGIRequestHandler, ipv6=ipv6)
    if threading:
        httpd.daemon_threads = True
    httpd.set_app(wsgi_handler)
    httpd.serve_forever()
```

下面表示`WSGI server`服务器处理流程中关键的类和方法。

![](../.gitbook/assets/image%20%2883%29.png)

* WSGIServer `run()`方法会创建`WSGIServer`实例，主要作用是接收客户端请求，将请求传递给`application`，然后将`application`返回的`response`返回给客户端。
  * 创建实例时会指定`HTTP`请求的`handler`：`WSGIRequestHandler`类
  * 通过`set_app`和`get_app`方法设置和获取`WSGIApplication`实例`wsgi_handler`
  * 处理http请求时，调用`handler_request`方法，会创建`WSGIRequestHandler`实例处理http请求。
  * `WSGIServer`中`get_request`方法通过`socket`接受请求数据
* WSGIRequestHandler
  * 由`WSGIServer`在调用`handle_request`时创建实例，传入`request`、`cient_address`、`WSGIServer`三个参数，`__init__`方法在实例化同时还会调用自身的`handle`方法
  * `handle`方法会创建`ServerHandler`实例，然后调用其`run`方法处理请求
* ServerHandler
  * `WSGIRequestHandler`在其`handle`方法中调用`run`方法，传入`self.server.get_app()`参数，获取`WSGIApplication`，然后调用实例\(`__call__`\)，获取`response`，其中会传入`start_response`回调，用来处理返回的`header`和`status`。
  * 通过`application`获取`response`以后，通过`finish_response`返回`response`
* WSGIHandler
  * `WSGI`协议中的`application`，接收两个参数，`environ`字典包含了客户端请求的信息以及其他信息，可以认为是请求上下文，`start_response`用于发送返回`status`和`header`的回调函数

虽然上面一个`WSGI server`涉及到多个类实现以及相互引用，但其实原理还是调用`WSGIHandler`，传入请求参数以及回调方法`start_response()`，并将响应返回给客户端。

#### django simple\_server

`django`的`simple_server.py`模块实现了一个简单的`HTTP`服务器，并给出了一个简单的`demo`，可以直接运行，运行结果会将请求中涉及到的环境变量在浏览器中展示出来。  
其中包括上述描述的整个`http`请求的所有组件:  
`ServerHandler`, `WSGIServer`, `WSGIRequestHandler`，以及`demo_app`表示的简易版的`WSGIApplication`。  
可以看一下整个流程：

```python
if __name__ == '__main__':
    # 通过make_server方法创建WSGIServer实例
    # 传入建议application，demo_app
    httpd = make_server('', 8000, demo_app)
    sa = httpd.socket.getsockname()
    print("Serving HTTP on", sa[0], "port", sa[1], "...")
    import webbrowser
    webbrowser.open('http://localhost:8000/xyz?abc')
    # 调用WSGIServer的handle_request方法处理http请求
    httpd.handle_request()  # serve one request, then exit
    httpd.server_close()
    
def make_server(
    host, port, app, server_class=WSGIServer, handler_class=WSGIRequestHandler
):
    """Create a new WSGI server listening on `host` and `port` for `app`"""
    server = server_class((host, port), handler_class)
    server.set_app(app)
    return server

# demo_app可调用对象，接受请求输出结果
def demo_app(environ,start_response):
    from io import StringIO
    stdout = StringIO()
    print("Hello world!", file=stdout)
    print(file=stdout)
    h = sorted(environ.items())
    for k,v in h:
        print(k,'=',repr(v), file=stdout)
    start_response("200 OK", [('Content-Type','text/plain; charset=utf-8')])
    return [stdout.getvalue().encode("utf-8")]
```

`demo_app()`表示一个简单的WSGI application实现，通过`make_server()`方法创建一个`WSGIServer`实例，调用其`handle_request()`方法，该方法会调用`demo_app()`处理请求，并最终返回响应。

### uWSGI

`uWSGI`旨在为部署分布式集群的网络应用开发一套完整的解决方案。主要面向`web`及其标准服务。由于其可扩展性，能够被无限制的扩展用来支持更多平台和语言。`uWSGI`是一个`web`服务器，实现了`WSGI`协议，`uwsgi`协议，`http`协议等。  
`uWSGI`的主要特点是：

* 超快的性能
* 低内存占用
* 多`app`管理
* 详尽的日志功能（可以用来分析`app`的性能和瓶颈）
* 高度可定制（内存大小限制，服务一定次数后重启等）

`uWSGI`服务器自己实现了基于`uwsgi`协议的`server`部分，我们只需要在`uwsgi`的配置文件中指定`application`的地址，`uWSGI`就能直接和应用框架中的`WSGI application`通信。

