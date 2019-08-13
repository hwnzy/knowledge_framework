# Python基础面试题

* **基础语法是否熟悉？介绍下。**

> \[if-else/if-elif-else\]\[while/break/continue\]\[for-in/for-else\]\[def/return\]

* **有哪些关键字，并且解释其作用？**

&gt;&gt; help\('keywords'\)

```text
Here is a list of the Python keywords.  Enter any keyword to get more help.

and                 elif                import              return
as                  else                in                  try
assert              except              is                  while
break               finally             lambda              with
class               for                 not                 yield
continue            from                or
def                 global              pass
del                 if                  raise
```

* **有哪些内置方法，并且解释其作用？**

```python
abs() # 返回数字的绝对值
all() # 接收一个可迭代对象，如果对象里的所有元素的bool运算值都是True，那么返回True，否则返回False
any() # 接收一个可迭代对象，如果对象里有一个元素的bool运算值是True，那么返回True，否则返回False
ascii() # 调用对象的__repr__()方法（返回对象的string格式），获得该方法的返回值
bin() oct() hex() # 三个函数是将十进制分别转换为2/8/16进制
bool() # 测试一个对象或者表达式的执行结果是True还是False
bytearray() # 返回一个新字节数组。数组里的元素是可变，每个元素的值范围：0<=x<256
bytes() # 将对象转换为字节类型，可指定编码方式
str() # 将对象转换为字符串类型，可指定编码方式
callable() # 判断对象是否可以被调用，如果某个对象具有__call__方法就可以被调用
chr() # 返回某个十进制数对应的ASCII字符
ord() # 返回某个ASCII字符对应的十进制数
classmethod() staticmethod() property() # 类机制中，用于生成类方法、静态方法和属性的函数
compile() # 将字符串编译成python能识别和执行的代码
complex() # 通过数字和字符串生成复数类型对象
delattr() setattr() getattr() hasattr() # 类机制中，分别用来删除、设置、获取和判断属性
dir() # 显示对象所有的属性和方法。打印当前程序的所有变量。最好用的辅助函数之一
int() float() list() dict() set() tuple() # 他们都是实例化对应数据类型的类
divmod() # 除法，同时返回商和余数的元组
enumerate() # 将一个可遍历的数据对象组合为一个索引序列，同时列出数据和数据下标
eval() # 执行一个字符串表达式，并返回表达式的值。只能处理单行代码，有返回值
exec() # 执行储存在字符串或文件中的Python语句。能处理多行代码，没有返回值
format() # 调用对象所属类的__format__方法，类似print功能
frozenset() # 返回一个冰冻的集合，该集合不能再添加或删除任何元素
globals() # 列出当前环境下所有的全局变量
hash() # 为不可对象生成哈希值的函数
help() # 返回对象的帮助文档
id() # 返回对象的内存地址，查看变量引用的变化，对象是否相等
input() # 接收用户输入，返回一个输入的字符串
isinstance() # 判断一个对象是否是某个类的实例
issubclass() # (a, b) 判断a是否是b的子类
iter() # 制造一个迭代器，使其具备next()能力
len() # 返回对象的长度
locals() # 以字典类型返回当前位置的全部局部变量
max() min() # 返回给定集合里最大/最小的元素，可以指定排序方法如：key=len
memoryview(obj) # 返回obj内存视图对象，obj只能是bytes或bytesarray类型
next() # 调用迭代器的__next__()方法，获取下一个元素
object() # 不接受任何参数，object是Python所有类的基类
open() # 打开文件的方法
pow() # 幂函数
print() # 控制台输出
range() # 创建一个整数列表，一般用在for循环中
repr() # 将对象转换为供解释器读取的形式，返回对象的string格式
reversed() # 反转逆序对象
round() # 返回浮点数X的四舍五入值
slice() # 返回一个切片类型的对象， list(slice(start, end, sep))
sum # 求和
super() # 调用父类
sorted() # 排序方法，有key和reverse两个重要参数
type() # 显示对象所示数据类型
vars() # 与dir()类似，dir()返回key, vars()返回key-value
map() # 根据提供的函数对指定序列做映射
filter() # 根据提供的函数对指定序列做映射
zip() # 将可迭代对象对应的元素打包成一个个元组，返回元组组成的列表
```

* **解释下什么是动态语言？动态强类型是指什么？**

Python 是`强类型`的`动态脚本语言。`

`强类型`：不允许不同类型相加。

 `动态`：不使用显示数据类型声明，确定一个变量类型是在第一次赋值时。 

`脚本语言`：解释型语言，运行代码只需要解释器，不需要编译。

* **是否有编码规范的概念？采用的是哪中编码规范？**

提高代码的可读性。PEP8规范。

* **解释下深拷贝和浅拷贝**

浅拷贝：不可变类型只拷贝引用。可变类型只对第一层对象开辟新内存空间。

深拷贝：不可变类型只拷贝引用。只要有一层有可变对象就对该对象每一层对象进行拷贝。

```python
import copy
copy.copy()
copy.deepcopy()
```

* **lambda的用法以及使用场景？**

`lambda 参数 : 返回值`

1.  lambda 函数比较轻便，即用即仍，很适合需要完成一项功能，但是此功能只在此一处使用， 连名字都很随意的情况下
2. 匿名函数，一般用来给 filter， map 这样的函数式编程服务
3. 作为回调函数，传递给某些应用，比如消息处理

* **解释下什么是闭包，以及它的作用？**

1. 函数嵌套
2. 内部函数使用外部函数的变量（包括外部函数参数）
3. 外部函数返回内部函数

闭包的作⽤就是让⼀个变量能够常驻内存. 供后⾯的程序使⽤

* **实现一个简单的装饰器，用来对某个函数的结果进行缓存？**

```python
def decorator(func):
  def inner(*args, **kwargs):
    result = func(*args, **kwargs)
    return result
  
  return inner
```

* **Python中几种容器类型的差别及使用场景？**

|  | 定义 | 特点 |
| :--- | :--- | :--- |
| 字符串 | 字符/不可变/序列 | 序列：有顺序、能索引、切片、获取元素更灵活 |
| 列表 | 变量/可变/序列 | 可变：适用于需要添加修改。序列：有顺序，能索引，切片，获取元素更灵活 |
| 元组 | 变量/不可变/序列 | 不可变：当元素个数固定且不需要改变，优先用元组，能够节省内存 |
| 字典 | 键值对/可变/散列 | 键值对：哈希原理，查找速度快，占用内存大，键名唯一，代码可读性好。散列：元素无法排序，无法直接索引，无法切片 |
| 集合 | 不重复变量/可变/散列 | 相对于只有键没有值的字典，主要用于数学运算 |

* **列表推导式的使用和场景？**

列表推导式（又称列表解析式）提供了一种简明扼要的方法来创建列表。

它的结构是在一个中括号里包含一个表达式，然后是一个for语句，然后是 0 个或多个 for 或者 if 语句。那个表达式可以是任意的，意思是你可以在列表中放入任意类型的对象。返回结果将是一个新的列表，在这个以 if 和 for 语句为上下文的表达式运行完成之后产生。

列表推导式的执行顺序：各语句之间是嵌套关系，左边第二个语句是最外层，依次往右进一层，左边第一条语句是最后一层。

```python
[x*y for x in range(1,5) if x > 2 for y in range(1,4) if y < 3]
```

他的执行顺序是:

```python
for x in range(1,5)
    if x > 2
        for y in range(1,4)
            if y < 3
                x*y
```

* **yield的使用。**

数据不是一次性全部生成处理，而是使用一个，再生成一个，可以**节约大量的内存**。

```python
def my_generater(n):
    for i in range(n):
        yield i  # 代码执行到yield会暂停返回结果，下次启动生成器会在暂停的位置继续往下执行
```

* **常用的内置库有哪些？举例他们的用法。**

```python
import os 
result = os.path.join(var1, var2)  # 将两个路径合并成一个
import socket 
serversocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # 创建socket对象
import sys
sys.argv  # 命令行参数List
import time
result = time.time() # 时间戳单位最适于做日期运算。但是1970年之前的日期就无法以此表示了。太遥远的日期也不行，UNIX和Windows只支持到2038年
import json
json_str = json.dumps(data) # Python字典类型转换为JSON对象
data = json.loads(json_str) # # 将JSON对象转换为Python字典
```

* **介绍下你了解的"dunder method"（魔法方法）有哪些，以及他们的作用？**



* **解释下面向对象的概念，以及在编程中的作用？**

抽象 / 封装 / 继承 / 多态

* **如何实现单例模式？**

```python
class A(object):
    __instance = None
    def __new__(cls， *args， **kwargs):
        if cls.__instance is None:
            cls.__instance = object.__new__(cls)
        return cls.__instance
            else:
        return cls.__instance
```

* **如何对Python对象进行序列化？**

json模块主要有四个比较重要的函数，分别是：

`dump` - 将Python对象按照JSON格式序列化到文件中

`dumps` - 将Python对象处理成JSON格式的字符串

`load` - 将文件中的JSON数据反序列化成对象

`loads` - 将字符串的内容反序列化成Python对象

* **是否能够熟练编写多线程和多进程程序？**

\*\*\*\*

* **使用socket编写一个简单的HTTP Server，成功返回success即可。**

```python
import socket

if __name__ == '__main__':
    # 创建tcp服务端套接字
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 设置端口号复用，让程序退出端口号立即释放
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True) 
    # 给程序绑定端口号
    tcp_server_socket.bind(("", 8080))
    # 设置监听
    # 128:最大等待建立连接的个数， 提示： 目前是单任务的服务端，同一时刻只能服务与一个客户端，后续使用多任务能够让服务端同时服务与多个客户端，
    # 不需要让客户端进行等待建立连接
    # listen后的这个套接字只负责接收客户端连接请求，不能收发消息，收发消息使用返回的这个新套接字来完成
    tcp_server_socket.listen(128)
    # 等待客户端建立连接的请求, 只有客户端和服务端建立连接成功代码才会解阻塞，代码才能继续往下执行
    # 1. 专门和客户端通信的套接字： service_client_socket
    # 2. 客户端的ip地址和端口号： ip_port
    service_client_socket, ip_port = tcp_server_socket.accept()
    # 代码执行到此说明连接建立成功
    print("客户端的ip地址和端口号:", ip_port)  # ('172.16.47.209', 52472)
    # 接收客户端发送的数据, 这次接收数据的最大字节数是1024
    recv_data = service_client_socket.recv(1024)
    # 获取数据的长度
    recv_data_length = len(recv_data)
    print("接收数据的长度为:", recv_data_length)
    # 对二进制数据进行解码
    recv_content = recv_data.decode("gbk")
    print("接收客户端的数据为:", recv_content)
    # 准备发送的数据
    send_data = "ok, 问题正在处理中...".encode("gbk")
    # 发送数据给客户端
    service_client_socket.send(send_data)
    # 关闭服务与客户端的套接字， 终止和客户端通信的服务
    service_client_socket.close()
    # 关闭服务端的套接字, 终止和客户端提供建立连接请求的服务
    tcp_server_socket.close()
```

* **如何理解Python中的GIL？对我们日常开发有什么影响？**

Python的解释器有一个“全局解释器锁”（GIL）的东西，任何线程执行前必须先获得GIL锁，然后每执行100条字节码，解释器就自动释放GIL锁，让别的线程有机会执行。

* **解释下协程、线程、进程之间的差别。**

1. 一个执行中程序的实例。系统中的每个程序都运行在某个进程的上下文中。进程是系统资源分配的最小单位
2. 线程就是运行在进程上下文中的逻辑流。线程是操作系统能够进行运算调度的最小单位
3. 协程是一个大型程序中的某部分代码，由一个或多个语句块组成。它负责完成某项特定任务，而且相较于其他代码，具备相对的独立性。

* 一个独立的逻辑控制流：它提供一个假象，好像我们的程序独占地使用处理器。
* 一个私有的地址空间，它提供一个假象，好像我们的程序独占地使用内存系统。

1. 进程出现的目的，是为了更好的利用CPU资源
2. 线程共享进程的大部分资源，并参与CPU的调度
3. 协程通过在线程中实现调度，避免了陷入内核级别的上下文切换造成的性能损失，进而突破了线程在IO上的性能瓶颈 

> 核心只有一个，**线程是操作系统调度，协程是用户态调度。** 协程不必须是语言集成，例如C语言可以用setjmp/longjmp实现，也可以自己通过改变esp指针换栈实现协程。 协程本身跟高吞吐没任何关系，基于io多路复用+回调就可以实现高并发和高吞吐。**引入协程是为了将回调逻辑变成线性同步逻辑。**



