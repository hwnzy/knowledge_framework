# Python基础面试题

## 常用的 Python 标准库都有哪些？

| os  | time | threading | pymysql | multiprocessing | queue | random |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 操作系统 | 时间 | 线程 | 连接数据库 | 连接数据库 | 队列 | 随机 |

## 赋值、浅拷贝和深拷贝的区别？

* **赋值**

在 Python 中，对象的赋值就是简单的对象引用： 

```python
a = [1,2,"hello",['python', 'C++']] 
b = a
```

在上述a 和 b 指向同一片内存，b是a的引用，可使用`b is a` 去判断，返回 `True`，表明他们地址相同，内容相同，也可以使用 id\(\)函数看两个列表的地址是否相同。

* **浅拷贝\(shallow copy\)**

浅拷贝会创建新对象，其内容非原对象本身的引用，而是原对象内第一层对象的引用。 

浅拷贝有三种形式:切片操作、工厂函数、 copy 模块中的 copy 函数。 

**切片操作**：`b = a[:]` 或者 `b = [x for x in a]`

**工厂函数**：`b = list(a)`

**copy 函数**：`b = copy.copy(a)`

列表 b 列表 a 使用 is 判断可以发现他们不是同一个对象，使用 id 查看也不指向同一片内存空间。**但当我们使用 `id(x) for x in a` 和 `id(x) for x in b` 查看 a 和 b 中元素地址，可以看到二者包含的元素的地址是相同的。** 

**但是要注意的是**，浅拷贝只拷贝了一层，列表 a 中嵌套的 list，修改了它情况就不一样。 比如：`a[3].append('java')`。查看列表 b会发现列表 b 也发生了变化。

* **深拷贝\(deep copy\)**

深拷贝只有使用copy 模块中的`deepcopy()`函数。 深拷贝拷贝了对象所有元素，因此时间和空间开销高。 

如果使用 b = copy.deepcopy\(a\)，再修改列表 b 将不会影响到列表 a，因为深拷贝拷贝出来的对象根本就是一个全新的对象， 不再与原来的对象有任何的关联。

* **拷贝的注意点**

对于非容器类型，如数字、字符，以及其他的“原子” 类型，没有拷贝一说，产生的都是原对象的引用。 

如果元组变量值包含原子类型对象，即使采用了深拷贝，也只能得到浅拷贝。

## \_\_init\_\_ 和\_\_new\_\_的区别？

`__init__` 在对象创建后，对对象进行初始化。 

`__new__` 是在对象创建之前创建一个对象，并将该对象返回给 `__init__`。

## Python 里面如何生成随机数？

在 Python 中用于生成随机数的模块是 random

`random.random()`：生成一个 0-1 之间的随机浮点数

`random.randint(a, b)`：生成\[a, b\]之间的整数

`random.choice(sequence)`：从特定序列中随机取一个元素，序列可以是字符串，列表， 元组等。

## 输入某年某月某日，判断这一天是这一年的第几天？

```python
import datetime
def dayofyear():
    year = input("请输入年份：")
    month = input("请输入月份：")
    day = input("请输入天：")
    date1 = datetime.date(year=int(year), month=int(month), day=int(day))
    date2 = datetime.date(year=int(year), month=1, day=1)
    return (date1 - date2 + 1).day
```

## 打乱一个排好序的 list 对象 alist？

```python
import random
random.shuffle(alist)
```

## 说明一下 os.path 和 sys.path 分别代表什么？

`os.path` 主要是用于对系统路径文件的操作。 

`sys.path` 主要是对 Python 解释器的系统环境参数的操作（动态的改变 Python 解释器搜索路径）。

## Python 中的 os 模块常见方法？

`os.remove()`删除文件 

`os.rename()`重命名文件 

`os.path.join()`将分离的各部分组合成一个路径名

`os.path.exists()`是否存在

## Python 的 sys 模块常用方法？

`sys.argv` 命令行参数 List，第一个元素是程序本身路径

## 模块和包是什么？

* 模块是搭建程序的一种方式。每一个Python代码文件都是一个模块，并可以引用其他的模块，比如对象和属性。 
* 包是包含许多模块和子文件夹的文件夹。

## unittest 是什么？

unittest 是Python中的**单元测试框架**。它拥有支持`共享搭建`、`自动测试`、`在测试中暂停代码`、`将不同测试迭代成一组`等功能。

## Python 中有日志吗?怎么使用?

Python 自带`logging`模块，调用 `logging.basicConfig()`方法配置需要的日志等级和相应的参数， Python 解释器会按照配置的参数生成相应的日志。

## 在 except 中 return 后还会不会执行 finally 中的代码？怎么抛出自定义异常？

finally 中的代码会继续处理 

raise 可以抛出自定义异常

## 介绍一下 except 的作用和用法



## 4G 内存怎么读取一个 5G 的数据？

方法一： 可以通过生成器，分多次读取，每次读取数量相对少的数据（比如 500MB）进行处理，处理结束后 在读取后面的 500MB 的数据。 

方法二： 可以通过 linux 命令 split 切割成小文件，然后再对数据进行处理，此方法效率比较高。可以按照行 数切割，可以按照文件大小切割。

## 现在考虑有一个 jsonline 格式的文件 file.txt 大小约为 10K，之前处理文件的 代码如下所示

```python
def get_lines():
    l = []
    with open(‘file.txt’，‘rb’ ) as f:
        for eachline in f:
            l.append(eachline)
    return l
if __name__ == ‘__main__’ :
    for e in get_lines():
        process(e) #处理每一行数据
```

现在要处理一个大小为 10G 的文件，但是内存只有 4G，如果在只修改 get\_lines 函数而其他代 码保持不变的情况下，应该如何实现？需要考虑的问题都有哪些？

```python
def get_lines():
    l = []
    with open(‘file.txt’，’ rb’ ) as f:
        data = f.readlines(60000)
    l.append(data)
    yield l
```

要考虑到的问题有： 内存只有 4G 无法一次性读入 10G 的文件，需要分批读入。分批读入数据要记录每次读入数据的位 置。分批每次读入数据的大小，太小就会在读取操作上花费过多时间。

## read、 readline 和 readlines 的区别?

read:读取整个文件

readline：读取下一行，使用生成器方法

readlines：读取整个文件到一个迭代器以供我们遍历

## 补充缺失的代码？

```python
def print_directory_contents(sPath):
    """
    这个函数接收文件夹的名称作为输入参数
    返回该文件夹中文件的路径
    以及其包含文件夹中文件的路径
    """
# 补充代码
```

```python
import os
for sChild in os.listdir(sPath):
    sChildPath = os.path.join(sPath, sChild)
    if os.path.isdir(sChildPath):
        print_directory_contents(sChildPath)
    else:
        print(sChildPath)
```

##  Python中可变对象和不可变对象？

不可变对象，该对象所指向的内存中的值不能被改变。当改变某个变量时候，由于其所指的值不能被改变，相当于把原来的值复制一份后再改变，这会开辟一个新的地址，变量再指向这个新的地址。 

可变对象，该对象所指向的内存中的值可以被改变。变量（准确的说是引用）改变后，实际上是其所指的值直接发生改变，并没有发生复制行为，也没有开辟新的出地址，通俗点说就是原地改变。 

Python 中，数值类型（int 和 float）、字符串 str、元组 tuple 都是**不可变类型**。而列表 list、字典 dict、集合 set 是**可变类型**。

## Python 中 is 和==的区别？

is 判断的是 a 对象是否就是 b 对象，是通过 id 来判断的。 

==判断的是 a 对象的值是否和 b 对象的值相等，是通过 value 来判断的。

## Python 的魔法方法

魔法方法就是可以给你的类增加魔力的特殊方法，如果你的对象实现 （重载）了这些方法中的某一个，那么这个方法就会在特殊的情况下被 Python 所调用，你可以定义自己想要的行为，而这一切都是自动发生的。 

**\_\_init\_\_** 构造器，当一个实例被创建的时候初始化的方法。但是它并不是实例化调用的第一个方法。

**\_\_new\_\_**才是实例化对象调用的第一个方法，它只取下 cls 参数，并把 其他参数传给 **\_\_init\_\_**。 **\_\_new\_\_**很少使 用，但是也有它适合的场景，尤其 是当类继承自一个像元组或者字符串这样不经常改变的类型的时候。 

**\_\_call\_\_** 允许一个类的实例像函数一样被调用 。 

**\_\_getitem\_\_** 定义获取容器中指定元素的行为，相当于 self\[key\] 。 

**\_\_getattr\_\_** 定义当用户试图访问一个不存在属性的时候的行为 。 

**\_\_setattr\_\_** 定义当一个属性被设置的时候的行为 。 

**\_\_getattribute\_\_** 定义当一个属性被访问的时候的行为 。

## 面向对象中怎么实现只读属性?

```python
class MyCls(object):
    __weight = 50

    @property #以访问属性的方式来访问 weight 方法
    def weight(self):
        return self.__weight

if __name__ == '__main__':
    obj = MyCls()
    print(obj.weight)
    obj.weight = 12
```

## 谈谈你对面向对象的理解？

面向对象是相对于面向过程而言的。

面向过程语言是一种**基于功能分析的、以算法为中心**的程序设计方法。

面向对象是一种基于结构分析的、以数据为中心的程序设计思想。在面向对象语言中有一个有很重要东西，叫做类。 

面向对象有三大特性：**封装、继承、多态**。

## Python 中类方法、类实例方法、静态方法有何区别？

**类方法**：类对象的方法，在定义时需要在上方使用“@classmethod”进行装饰，形参为 cls， 表示类对象。类对象和实例对象都可调用。

**类实例方法**：类实例化对象的方法，只有实例对象可以调用，形参为 self，指代对象本身。

**静态方法**：一个任意函数，在其上方使用“@staticmethod”进行装饰，可以用对象直接调用， 静态方法实际上跟该类没有太大关系。

## Python 中如何动态获取和设置对象的属性？

```python
if hasattr(Parent, 'x'):
    print(getattr(Parent, 'x'))
    setattr(Parent, 'x', 3)
print(getattr(Parent, 'x'))
```

## Python 函数调用的时候参数的传递方式是值传递还是引用传递？

Python 的参数传递有：位置参数、默认参数、可变参数、关键字参数。

函数的传值到底是值传递还是引用传递，要分情况：

* 不可变参数用值传递：

> 像整数和字符串这样的不可变对象，是通过拷贝进行传递的，因为你无论如何都不可能在原处改变不可变对象。

* 可变参数是引用传递的：

> 比如像列表，字典这样的对象是通过引用传递、和 C 语言里面的用指针传递数组很相似，可变对象能在函数内部改变。

## 对缺省参数的理解？

> 缺省参数指在调用函数的时候没有传入参数的情况下，调用默认的参数，在调用函数的同时赋值时，所传入的参数会替代默认参数。

**args 是不定长参数**，他可以表示输入参数是不确定的，可以是任意多个。 __

**\*kwargs 是关键字参数**，赋值的时候是以键 = 值的方式，参数是可以任意多对

在定义函数的时候不确定会有多少参数会传入时，就可以使用两个参数。

## 为什么函数名字可以当做参数用?

Python 中一切皆对象，函数名是函数在内存中的空间，也是一个对象。

## Python 中 pass 语句的作用是什么？

在编写代码时只写框架思路，具体实现还未编写就可以用 pass 进行占位，使程序不报错，不会进 行任何操作。

## 有这样一段代码，print c 会输出什么，为什么？

```python
a = 10
b = 20
c = [a]
a = 15
```

答：10 对于字符串、数字，传递是相应的值。

## 什么是 lambda 函数？ 有什么好处？

lambda 函数是一个可以接收任意多个参数\(包括可选参数\)并且返回单个表达式值的函数 

1.  lambda 函数比较轻便，即用即仍，很适合需要完成一项功能，但是此功能只在此一处使用， 连名字都很随意的情况下
2. 匿名函数，一般用来给 filter， map 这样的函数式编程服务
3. 作为回调函数，传递给某些应用，比如消息处理

## 下面这段代码的输出结果将是什么？请解释

```python
def multipliers():
    return [lambda x : i * x for i in range(4)]
print [m(2) for m in multipliers()]
```

上面代码输出的结果是\[6， 6， 6， 6\]，而不是我们想的\[0， 2， 4， 6\]

上述问题产生的原因是 **Python 闭包的延迟绑定**。这意味着内部函数被调用时，参数的值在闭包内进行查找。因此当任何由 multipliers\(\)返回的函数被调用时，i 的值将在附近的范围进行查找。**那时不管返回的函数是否被调用，for 循环已经完成**，i 被赋予了最终的值 3。因此，每次返回的函数乘以传递过来的值 3，因为上段代码传过来的值是 2，它们最终返回的都是 6。

## 你如何修改上面的 multipliers 的定义产生想要的结果？

一种解决方法就是用 Python 生成器

```python
def multipliers():
    for i in range(4): 
        yield lambda x : i * x
```

另外一个解决方案就是创造一个闭包，利用默认函数立即绑定

```python
def multipliers():
    return [lambda x, i=i : i * x for i in range(4)]
```

## map 函数和 reduce 函数？

* **参数方面**

map\(\)包含两个参数，第一个参数是一个函数，第二个是序列（列表 或元组）

函数可以接收一个或多个参数

educe\(\)第一个参数是函数，第二个是序列（列表或元组）

函数必须接收两个参数

* **传入数值**

map\(\)将函数依次作用到序列的每个元素，每个元素都是独自被函数“作用”一次 。 

reduce\(\)将函数作用在序列的第一个元素得到结果后，把这个结果继续与下一个元素作用 （累积计算）

## 递归函数停止的条件？

1. 判断递归的次数是否达到某一限定值
2. 判断运算的结果是否达到某个范围等，根据设计的目的来选择

## 回调函数，如何通信的?

回调函数是把函数的地址作为参数传递给另一个函数，将整个函数当作一个对象赋值给调用的函数。

## Python 主要的内置数据类型都有哪些？ 

内建类型：布尔类型、数字、字符串、列表、元组、字典、集合

## `print(dir('a'))` 的输出？

输出字符串'a'的内建方法

## print\(list\(map\(lambda x: x \* x, \[y for y in range\(3\)\]\)\)\)的输出？

`[0， 1， 4]`

## hasattr\(\) getattr\(\) setattr\(\) 函数使用详解？

* hasattr\(object, name\)函数

> 判断一个对象里面是否有name属性或者name方法，返回bool值，有name属性\(方法\)返回True， 否则返回 False。

* getattr\(object, name\[,default\]\) 函数

> 获取对象 object 的属性或者方法，如果存在则打印出来，如果不存在，打印默认值，默认值可选。 注意：如果返回的是对象的方法，则打印结果是方法的内存地址，如果需要运行这个方法，可以在后面添加括号\(\)。

* setattr\(object,name,values\)函数

> 给对象的属性赋值，若属性不存在，先创建再赋值。

```python
class function_demo(object):
    name = 'demo'
    def run(self):
        return "hello function"
functiondemo = function_demo()
res = hasattr(functiondemo, 'addr') # 先判断是否存在 
if res:
    addr = getattr(functiondemo, 'addr')
    print(addr)
else:
    addr = getattr(functiondemo, 'addr', setattr(functiondemo, 'addr', '北京首都'))
    #addr = getattr(functiondemo, 'addr', '美国纽约')
    print(addr)
```

## 一句话解决阶乘函数？

`reduce(lambda x,y: x*y, range(1,n+1))  # python2中使用，python3中取消了该函数`

## 使用 Socket 套接字需要传入哪些参数 ？

Address Family 和 Type，分别表示套接字应用场景和类型。 family 的值可以是 AF\_UNIX\(Unix 域，用于同一台机器上的进程间通讯\)，也可以是 AF\_INET （对于 IPV4 协议的 TCP 和 UDP），至于 type 参数，SOCK\_STREAM（流套接字）或者 SOCK\_DGRAM（数据报文套接字）,SOCK\_RAW（raw 套接字）

## 说说 HTTP 和 HTTPS 区别？

HTTP 协议传输的数据都是未加密的，也就是明文的，因此使用 HTTP 协议传输隐私信息非常不安 全，为了保证这些隐私数据能加密传输，于是网景公司设计了 SSL（Secure Sockets Layer）协议用于 对 HTTP 协议传输的数据进行加密，从而就诞生了 HTTPS。简单来说，HTTPS 协议是由 SSL+HTTP 协 议构建的可进行加密传输、身份认证的网络协议，要比 http 协议安全。

HTTPS 和 HTTP 的区别主要如下： 

1、 https 协议需要到 ca 申请证书，一般免费证书较少，因而需要一定费用。 

2、 http 是超文本传输协议，信息是明文传输，https 则是具有安全性的 ssl 加密传输协议。 

3、 http 和 https 使用的是完全不同的连接方式，用的端口也不一样，前者是 80，后者是 443。

4、 http 的连接很简单，是无状态的；HTTPS 协议是由 SSL+HTTP 协议构建的可进行加密传输、 身份认证的网络协议，比 http 协议安全。

## 谈一下 HTTP 协议以及协议头部中表示数据类型的字段？

HTTP 协议是 Hyper Text Transfer Protocol（超文本传输协议）的缩写，是用于从万维网 （WWW:World Wide Web）服务器传输超文本到本地浏览器的传送协议。

 HTTP 是一个基于 TCP/IP 通信协议来传递数据（HTML 文件， 图片文件， 查询结果等）。 

HTTP 是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体 信息系统。它于 1990 年提出，经过几年的使用与发展，得到不断地完善和扩展。目前在 WWW 中 使用的是 HTTP/1.0 的第六版，HTTP/1.1 的规范化工作正在进行之中，而且 HTTP-NG\(Next Generation of HTTP\)的建议已经提出。 

HTTP 协议工作于客户端-服务端架构为上。浏览器作为 HTTP 客户端通过 URL 向 HTTP 服务端即 WEB 服务器发送所有请求。 Web 服务器根据接收到的请求后，向客户端发送响应信息。 表示数据类型字段： Content-Type

## HTTP 请求方法都有什么？

根据 HTTP 标准，HTTP 请求可以使用多种请求方法。 

HTTP1.0 定义了三种请求方法： GET， POST 和 HEAD 方法。 

HTTP1.1 新增了五种请求方法：OPTIONS， PUT， DELETE， TRACE 和 CONNECT 方法。 

1、 GET 请求指定的页面信息，并返回实体主体。 

2、 HEAD 类似于 get 请求，只不过返回的响应中没有具体的内容，用于获取报头

3、 POST 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在 请求体中。 POST 请求可能会导致新的资源的建立和/或已有资源的修改。 

4、 PUT 从客户端向服务器传送的数据取代指定的文档的内容。 

5、 DELETE 请求服务器删除指定的页面。 

6、 CONNECT HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。 

7、 OPTIONS 允许客户端查看服务器的性能。 

8、 TRACE 回显服务器收到的请求，主要用于测试或诊断。

## HTTP 常见请求头？

1. Host \(主机和端口号\)
2. Connection \(链接类型\)
3. Upgrade-Insecure-Requests \(升级为 HTTPS 请求\)
4. User-Agent \(浏览器名称\)
5. Accept \(传输文件类型\)
6. Referer \(页面跳转处\)
7. Accept-Encoding（文件编解码格式）
8. Cookie （Cookie）
9. x-requested-with :XMLHttpRequest \(是 Ajax 异步请求\)



## 简述浏览器通过 WSGI 请求动态资源的过程?

1.发送 http 请求动态资源给 web 服务器 

2.web 服务器收到请求后通过 WSGI 调用一个属性给应用程序框架 

3.应用程序框架通过引用 WSGI 调用 web 服务器的方法，设置返回的状态和头信息。 

4.调用后返回，此时 web 服务器保存了刚刚设置的信息 

5.应用程序框架查询数据库，生成动态页面的 body 的信息 

6.把生成的 body 信息返回给 web 服务器 

7.web 服务器把数据返回给浏览器

## 描述用浏览器访问 www.baidu.com 的过程

先要解析出 baidu.com 对应的 ip 地址 

x 要先使用 arp 获取默认网关的 mac 地址 

x 组织数据发送给默认网关\(ip 还是 dns 服务器的 ip，但是 mac 地址是默认网关的 mac 地址\) 

x 默认网关拥有转发数据的能力，把数据转发给路由器 

x 路由器根据自己的路由协议，来选择一个合适的较快的路径转发数据给目的网关 

x 目的网关\(dns 服务器所在的网关\)，把数据转发给 dns 服务器 

x dns 服务器查询解析出 baidu.com 对应的 ip 地址，并原路返回请求这个域名的 client 

得到了 baidu.com 对应的 ip 地址之后，会发送 tcp 的 3 次握手，进行连接

使用 http 协议发送请求数据给 web 服务器 

x web 服务器收到数据请求之后，通过查询自己的服务器得到相应的结果，原路返回给浏览器。 

x 浏览器接收到数据之后通过浏览器自己的渲染功能来显示这个网页。 

x 浏览器关闭 tcp 连接，即 4 次挥手结束，完成整个访问过程

## Post 和 Get 请求的区别?

GET 请求，请求的数据会附加在 URL 之后，以?分割 URL 和传输数据，多个参数用&连接。 URL 的 编码格式采用的是 ASCII 编码，而不是 uniclde，即是说所有的非 ASCII 字符都要编码之后再传输。 

POST 请求：

POST 请求会把请求的数据放置在 HTTP 请求包的包体中。上面的 item=bandsaw 就 是实际的传输数据。 因此，GET 请求的数据会暴露在地址栏中，而 POST 请求则不会。 

传输数据的大小： 

x 在 HTTP 规范中，没有对 URL 的长度和传输的数据大小进行限制。但是在实际开发过程中，对 于 GET，特定的浏览器和服务器对 URL 的长度有限制。因此，在使用 GET 请求时，传输数据会 受到 URL 长度的限制。 

x 对于 POST，由于不是 URL 传值，理论上是不会受限制的，但是实际上各个服务器会规定对 POST 提交数据大小进行限制，Apache、 IIS 都有各自的配置。 

安全性： 

x POST 的安全性比 GET 的高。这里的安全是指真正的安全，而不同于上面 GET 提到的安全方法 中的安全，上面提到的安全仅仅是不修改服务器的数据。比如，在进行登录操作，通过 GET 请求， 用户名和密码都会暴露再 URL 上，因为登录页面有可能被浏览器缓存以及其他人查看浏览器的历史记录的原因，此时的用户名和密码就很容易被他人拿到了。除此之外，GET 请求提交的数据 还可能会造成 Cross-site request frogery 攻击

效率：GET 比 POST 效率高。 

POST 请求的过程： 

1.浏览器请求 tcp 连接（第一次握手） 

2.服务器答应进行 tcp 连接（第二次握手） 

3.浏览器确认，并发送 post 请求头（第三次握手，这个报文比较小，所以 http 会在此时进行 第一次数据发送） 

4.服务器返回 100 continue 响应 

5.浏览器开始发送数据 

6.服务器返回 200 ok 响应 

GET 请求的过程： 

1.浏览器请求 tcp 连接（第一次握手） 

2.服务器答应进行 tcp 连接（第二次握手） 

3.浏览器确认，并发送 get 请求头和数据（第三次握手，这个报文比较小，所以 http 会在此时 进行第一次数据发送） 

4.服务器返回 200 OK 响应



## 现有字典 d={‘a’ :24，‘g’ :52，‘i’ :12，‘k’ :33}请按字典中的 value 值进行排序？

```python
sorted(d.items(), key=lambda x:x[1])
```

## 说一下字典和json的区别？

字典是一种数据结构

json是一种数据的表现形式

字典的key值只要是能hash的就行，json 的 必须是字符串。

## 什么是可变、不可变类型？

可变类型指的是对象所在的内存块中的值可以改变，有列表、字典

不可变类型指的是对象所在内存块中的值不可以改变，有数值、字符串、元组

## 存入字典里的数据有没有先后排序？

存入的数据不会自动排序，可以使用sorted函数对字典进行排序。但是python3.6之后可以根据插入顺序排列

## 字典推导式？

```python
d = {key: value for (key, value) in iterable}
```

## 将字符串"k:1\|k1:2\|k2:3\|k3:4"，处理成 Python 字典：{k:1, k1:2, ... } \# 字典里的 K 作为字符串处理

```python
str1 = "k:1|k1:2|k2:3|k3:4"
def str2dict(str1):
    dict1 = {}
    for iterms in str1.split('|'):
        key, value = iterms.split(':')
        dict1[key] = value
    return dict1

# 快速解法
dicts = {key: value for (key, value) in (items.split(':') for items in str1.split('|'))}
```

## 请按 alist 中元素的 age 由大到小排序

```python
alist [{'name':'a'，'age':20}，{'name':'b'，'age':30}，{'name':'c'，'age':25}]
def sort_by_age(list1):
    return sorted(alist，key=lambda x:x['age']，reverse=True)
```

## 下面代码的输出结果将是什么？

```python
list = ['a'， 'b'， 'c'， 'd'， 'e']
print list[10:]
```

下面的代码将输出\[\]，不会产生IndexError错误，list\[10\]会导致IndexError

## 写一个列表生成式，产生一个公差为 11 的等差数列

```python
print([x*11 for x in range(10)])
```

## 给定两个列表，怎么找出他们相同的元素和不同的元素?

```python
list1 = [1，2，3]
list2 = [3，4，5]
set1 = set(list1)
set2 = set(list2)
print(set1 & set2)
print(set1 ^ set2)
```

## 请写出一段 Python 代码实现删除一个 list 里面的重复元素?

* 内置的set

```python
l1 = ['b'，'c'，'d'，'b'，'c'，'a'，'a']
l2 = list(set(l1))
```

* 如果想要保持他们原来的排序

```python
l1 = ['b', 'c', 'd', 'b', 'c', 'a', 'a']
l2 = list(set(l1))
l2.sort(key=l1.index)
```

```python
l1 = ['b', 'c', 'd', 'b', 'c', 'a', 'a']
l2 = sorted(set(l1), key=l1.index)
```

* 使用遍历

```python
l1 = ['b', 'c', 'd', 'b', 'c', 'a', 'a']
l2 = []
for i in l1:
    if not i in l2:
        l2.append(i)
```

## 下面这段代码的输出结果是什么？请解释？

```python
def extendlist(val, list=[]):
    list.append(val)
    return list

list1 = extendlist(10)
list2 = extendlist(123, [])
list3 = extendlist('a')

print("list1 = %s" %list1)
print("list2 = %s" %list2)
print("list3 = %s" %list3)
```

输出结果：

```python
list1 = [10, 'a']
list2 = [123]
list3 = [10, 'a']
```

新的默认列表只在函数被定义的那一刻创建一次。

当 extendList 被没有指定特定参数 list 调用时，这组 list 的值随后将被使用。

带有默认参数的表达式在函数被定义的时候被计算，不是在调用的时候被计算。

## 将以下 3 个函数按照执行效率高低排序

```python
def f1(lIn):
    l1 = sorted(lIn)
    l2 = [i for i in l1 if i<0.5]
    return [i*i for i in l2]

def f2(lIn):
    l1 = [i for i in l1 if i<0.5]
    l2 = sorted(l1)
    return [i*i for i in l2]

def f3(lIn):
    l1 = [i*i for i in lIn]
    l2 = sorted(l1)
    return [i for i in l1 if i<(0.5*0.5)]
```

按执行效率从高到低排列：f2、f1、f3。

要证明这个答案是正确的，你应该知道如何分析自己代码的性能。 

```python
import random
import cProfile
lIn = [random.random() for i in range(100000)]
cProfile.run('f1(lIn)')
cProfile.run('f2(lIn)')
cProfile.run('f3(lIn)')
```

## 如何理解 Python 中字符串中的\字符？

1. 转义字符 
2. 路径名中用来连接路径名 
3. 编写太长代码手动软换行

## 请反转字符串“aStr” ?

```python
print('aStr'[::-1])
```

## 阅读下面的代码，写出 A0，A1 至 An 的最终值

```python
A0 = dict(zip(('a', 'b', 'c', 'd', 'e'), (1, 2, 3, 4, 5)))
A1 = range(10)
A2 = [i for i in A1 if i in A0]
A3 = [A0[s] for s in A0]
A4 = [i for i in A1 if i in A3]
A5 = {i:i*i for i in A1}
A6 = [[i，i*i] for i in A1]
```

```python
A0 = {'a': 1， 'c': 3， 'b': 2， 'e': 5， 'd': 4}
A1 = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
A2 = []
A3 = [1, 3, 2, 5, 4]
A4 = [1, 2, 3, 4, 5]
A5 = {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81}
A6 = [[0, 0], [1, 1], [2, 4], [3, 9], [4, 16], [5, 25], [6, 36], [7, 49], [8, 64], [9, 81]]
```

##  Python2 中 range 和 xrange 的区别？

> 两者用法相同，不同的是range 返回的结果是一个列表，直接开辟一块内存空间来保存。列表xrange 的结果是一个生成器，是边循环边使用，只有使用时才会开辟内存空间。所以当列表很长时，使用 xrange 性能要比 range 好。

## 考虑以下 Python 代码，如果运行结束，命令行中的运行结果是什么？

```python
l = []
for i in xrange(10):
    l.append({'num' :i})
print l
```

```python
[{‘num’ :0}, {‘num’ :1}, {‘num’ :2}, {‘num’ :3}, {‘num’ :4}, {‘num’ :5}, {‘num’ :6}, {‘num’ :7}, {‘num’ :8}, {‘num’ :9}]
```

```python
l = []
a = {'num' :0}
for i in xrange(10):
    a['num'] = i
    l.append(a)
print l
```

```python
[{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}]
```

> 字典是可变对象，在下方的l.append\(a\)的操作中把字典a的引用传到列表l中，当后续操作修改a\['num'\]的值时候，l中的值也会跟着改变，相当于浅拷贝

## Python 是强语言类型还是弱语言类型？

Python 是`强类型`的`动态脚本语言`。

`强类型`：不允许不同类型相加。

 `动态`：不使用显示数据类型声明，确定一个变量类型是在第一次赋值时。 

`脚本语言`：解释型语言，运行代码只需要解释器，不需要编译。

## 关于 Python 程序的运行方面，有什么手段能提升性能？

1. 使用多进程，充分利用机器的多核性能 
2. 对于性能影响较大的部分代码，可以使用 C 或 C++编写 
3. 对于 IO 阻塞造成的性能影响，可以使用 IO 多路复用来解决
4. 尽量使用 Python 的内建函数
5. 尽量使用局部变量

## Python 中的作用域？

变量的作用域总是由在代码中被赋值的地方所决定，当 Python 遇到变量时按照如下顺序进行搜索： 

本地作用域\(Local\)---&gt;当前作用域被嵌入的本地作用域\(Enclosing locals\)---&gt;全局/模块作用域 \(Global\)---&gt;内置作用域\(Built-in\)

## 代码中要修改不可变数据会出现什么问题? 抛出什么异常?

代码不会正常运行，抛出 TypeError 异常

## a=1,b=2,不用中间变量交换 a 和 b 的值？

*  方法一

```python
a = a + b
b = a - b
a = a - b
```

*  方法二

```python
a = a ^ b
b = b ^ a
a = a ^ b
```

* 方法三

```python
a, b = b, a
```

## print 调用 Python 中底层的什么方法?

print 方法默认调用 sys.stdout.write 方法，即往控制台打印字符串

## 下面这段代码的输出结果将是什么？请解释？

```python
class Parent(object):
    x = 1
class Child1(Parent):
    pass
class Child2(Parent):
    pass
print(Parent.x, Child1.x, Child2.x)
Child1.x = 2
print(Parent.x, Child1.x, Child2.x)
Parent.x = 3
print(Parent.x, Child1.x, Child2.x)
```

1. 继承自父类的类属性x，所以都一样，指向同一块内存地址
2. 更改Child1，Child1的x指向了新的内存地址
3. 更改Parent，Parent和Child2的x指向了新的内存地址

##  简述你对 input\(\)函数的理解？

在 Python3 中，input\(\)获取用户输入，不论用户输入的是什么，获取到的都是字符串类型的。

在 Python2 中有 raw\_input\(\)和 input\(\), raw\_input\(\)和 Python3 中的 input\(\)作用是一样的， input\(\)输入的是什么数据类型的，获取到的就是什么数据类型的。

## 生成器、迭代器的区别？

迭代器是抽象的概念，它的类有 next 方法和 iter 方法返回自己本身。

对于 string、 list、 dict、 tuple 等这类容器对象，使用 for 循环遍历对容器对象调用 iter\(\)函数，iter\(\)会返回一个定义了 next\(\)方法的迭代器对象在容器中逐个访问容器内元素，在没有后续元素时会抛出一个 StopIteration 异常。 

生成器（Generator）是创建迭代器的简单而强大的工具。像是正规的函数只需要返回数据时使用 yield 语句。每次 next\(\)被调用时，生成器会返回它脱离的位置（它记忆语句最后一次执行的位置 和所有的数据值） 

区别：

生成器能做到迭代器能做的所有事,而且因为自动创建了 iter\(\)和 next\(\)方法,

生成器显得特别简洁,而且 生成器也是高效的，使用生成器表达式取代列表解析可以同时节省内存。

## X 是什么类型？`X = (for i in range(10))`

X 是 generator 类型

## 请尝试用“一行代码”实现将 1-N 的整数列表以 3 为单位分组，比如 1-100 分组后为?

```python
print([[x for x in range(1, 100)][i:i+3] for i in range(0, 100, 3)])
```

## Python 中 yield 的用法？

yield 就是保存当前程序执行状态。用 for 循环时每取一个元素就计算一次。用 yield 的函数叫 generator，和 iterator 一样。

它的好处是不用一次计算所有元素，而是用一次算一次，节省很多空间。

 generator 每次计算需要上一次计算结果，所以用 yield，否则一 return，上次计算结果就没了。



## 谈谈你对多进程，多线程，以及协程的理解，项目是否用？

**进程**：一个运行的程序（代码）就是一个进程，没有运行的代码叫程序，进程是系统资源分配的最 小单位，进程拥有自己独立的内存空间，所以进程间数据不共享，开销大。 

**线程**： 调度执行的最小单位，也叫执行路径，不能独立存在，依赖进程存在一个进程至少有一个 线程，叫主线程，而多个线程共享内存\(数据共享，共享全局变量\)，从而极大地提高了程序的运行效率。

**协程**：是一种用户态的轻量级线程，协程的调度完全由用户控制。协程拥有自己的寄存器上下文和栈。 协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，恢复先前保存的寄存器上下文和栈，直接操作栈则基本没有内核切换的开销，可以不加锁的访问全局变量，所以上下文的切换非常快。

## 什么是多线程竞争？

线程是非独立的，同一个进程里线程是数据共享的，当各个线程访问数据资源时会出现竞争状态即： 数据几乎同步会被多个线程占用，造成数据混乱 ，即所谓的线程不安全 

那么怎么解决多线程竞争问题？-- 锁。 

锁的好处： 确保了某段关键代码\(共享数据资源\)只能由一个线程从头到尾完整地执行能解决多线程资源竞争下 的原子操作问题。 

锁的坏处： 阻止了多线程并发执行，包含锁的某段代码实际上只能以单线程模式执行，效率就大大地下降了 锁的致命问题：死锁。

## 解释一下什么是锁，有哪几种锁?

锁\(Lock\)是 Python 提供的对线程控制的对象。有互斥锁、可重入锁。

## 什么是死锁呢？

若干子线程在系统资源竞争时，都在等待对方对某部分资源解除占用状态，结果是谁也不愿先解锁， 互相干等着，程序无法执行下去，这就是死锁。

GIL锁 全局解释器锁（只在 cpython 里才有） 作用：**限制多线程同时执行，保证同一时间只有一个线程执行**，所以 **cpython 里的多线程其实是伪多线程**!

Python 里常常使用协程技术来代替多线程，协程是一种更轻量级的线程， 进程和线程的切换时由系统决定，而协程由我们程序员自己决定，而模块 gevent 下切换是遇到了 耗时操作才会切换。 三者的关系：进程里有线程，线程里有协程

## 什么是线程安全，什么是互斥锁？

每个对象都对应于一个可称为" 互斥锁" 的标记，这个标记用来保证在任一时刻，只能有一个线程 访问该对象。 同一个进程中的多线程之间是共享系统资源的，多个线程同时对一个对象进行操作，一个线程操作 尚未结束，另一个线程已经对其进行操作，导致最终结果出现错误，此时需要对被操作对象添加互斥锁， 保证每个线程对该对象的操作都得到正确的结果。

## 说说下面几个概念：同步，异步，阻塞，非阻塞?

注意同步和异步只是针对于I/O操作，阻塞非阻塞相对于代码执行

**同步**：值调用IO操作时，必须等待IO操作完成后才开始新的的调用方式

**异步**：指调用IO操作时，不必等待IO操作完成就开始新的的调用方式

**阻塞**：指调用函数的时候，当前线程被挂起 

**非阻塞**：指调用函数的时候，当前线程不会被挂起，而是立即返回

## 什么是僵尸进程和孤儿进程？怎么避免僵尸进程?

**孤儿进程**：父进程退出，子进程还在运行的这些子进程都是孤儿进程，孤儿进程将被 **init 进程**\(进程号为 1\)所收养，并由 init 进程对它们完成状态收集工作。 

**僵尸进程**：进程使用 fork 创建子进程，如果子进程退出，而父进程并没有调用 wait 或 waitpid 获取子进程的状态信息，那么子进程的进程描述符仍然保存在系统中的这些进程是僵尸进程。 

**避免僵尸进程的方法**： 

1.fork 两次用孙子进程去完成子进程的任务

2.用 wait\(\)函数使父进程阻塞

3.使用信号量，在 signal handler 中调用 waitpid，这样父进程不用阻塞

## Python 中的进程与线程的使用场景?

多进程适合在 **CPU 密集型操作**\(cpu 操作指令比较多，如位数多的浮点运算\)。

多线程适合在 **IO 密集型操作**\(读写数据操作较多的，比如爬虫\)。

## 线程是并发还是并行，进程是并发还是并行？

线程是并发，进程是并行

 进程之间相互独立，是系统分配资源的最小单位，同一个线程中的所有线程共享资源。

## 并行（parallel）和并发（concurrency）？

并行：同一时刻点多个任务在运行，适合IO密集型

并发：同一时间间隔多个任务在运行，交替执行，适合CPU密集型

实现并行的库有：multiprocessing 

实现并发的库有：threading 

## IO 密集型和 CPU 密集型区别？

IO 密集型：系统运作，大部分的状况是 CPU 在等 I/O \(硬盘/内存\)的读/写。 

CPU 密集型：大部份时间用来做计算、逻辑判断等 CPU 动作的程序称之 CPU 密集型。

## 对装饰器的理解，并写出一个计时器方法执行性能的装饰器？

装饰器本质上是一个 Python 函数，它可以让其他函数在不需要做任何代码变动的前提下增加额外功能，装饰器的返回值也是一个函数对象。

```python
import time

def timeit(func):
    def wrapper():
        start = time.clock()
        func()
        end = time.clock()
        print('used:', end-start)
    return wrapper

@timeit
def foo():
    print('in foo()', foo())
```

## 解释一下什么是闭包?

1. 函数嵌套
2. 内部函数使用外部函数的变量（包括外部函数参数）
3. 外部函数返回内部函数

这个函数以及用到的一些变量称之为闭包。

## 函数装饰器有什么作用？

装饰器本质上是一个 Python 函数，它可以在让其他函数在**不需要做任何代码的变动的前提下**增加**额外的功能**。 装饰器的返回值也是一个函数的对象，它经常用于有特殊需求的场景。 

插入日志、性能测试、事务处理、缓存、 权限的校验等场景 有了装饰器就可以抽离出大量的与函数功能本身无关的雷同代码并发并继续使用。

```python
# 插入日志
def trace_func(func):
    def tmp(*args, **kwargs):
        print('start %s(%s, %s)...' % (func.__name__, args, kwargs))
        return func(*args, **kwargs)
    return tmp
```

```python
# 性能测试
from cProfile import Profile
def profile_wrapper(func):
    def wrapper(*args, **kwargs):
        prof = Profile()
        prof.enable()
        func(*args, **kwargs)
        prof.create_stats()
        prof.print_stats()

    return wrapper
```

* **基础语法是否熟悉？介绍下。**

> \[if-else/if-elif-else\]\[while/break/continue\]\[for-in/for-else\]\[def/return\]

* **有哪些关键字，并且解释其作用？**

&gt;&gt; help\('keywords'\)

```python
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



