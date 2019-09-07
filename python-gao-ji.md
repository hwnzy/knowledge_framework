# Python高级

### 内存管理与垃圾回收机制

#### Python 的内存管理机制及调优手段？

内存管理机制：引用计数、垃圾回收、内存池。

**引用计数**：

> 引用计数是一种非常高效的内存管理手段，当一个Python对象被引用时其引用计数增加1，当其不再被一个变量引用时则计数减1。当引用计数等于 0 时对象被删除。

**垃圾回收**：

1. 引用计数

   > 引用计数也是一种垃圾收集机制，而且也是一种最直观，最简单的垃圾收集技术。当 Python 的某 个对象的引用计数降为 0 时，说明没有任何引用指向该对象，该对象就成为要被回收的垃圾了。比如 某个新建对象，它被分配给某个引用，对象的引用计数变为 1。如果引用被删除，对象的引用计数为 0， 那么该对象就可以被垃圾回收。不过如果出现循环引用的话，引用计数机制就不再起有效的作用了

2. 标记清除

   > 如果两个对象的引用计数都为1，但是仅仅存在他们之间的循环引用，那么这两个对象都是需要被回收的，也就是说它们的引用计数虽然表现为非0，但实际上有效的引用计数为0。所以先将循环引用摘掉，就会得出这两个对象的有效计数。

3. 分代回收

   > 从前面“标记-清除”这样的垃圾收集机制来看，这种垃圾收集机制所带来的额外操作实际上与系统 中总的内存块的数量是相关的，当需要回收的内存块越多时，垃圾检测带来的额外操作就越多，而垃圾回收带来的额外操作就越少；反之当需回收的内存块越少时，垃圾检测就将比垃圾回收带来更少的额外操作。
   >
   > 举个例子：
   >
   > 当某些内存块 M 经过了 3 次垃圾收集的清洗之后还存活时，我们就将内存块 M 划到一个集合 A 中去，而新分配的内存都划分到集合 B 中去。当垃圾收集开始工作时，大多数情况都只对集合 B 进 行垃圾回收，而对集合 A 进行垃圾回收要隔相当长一段时间后才进行，这就使得垃圾收集机制需要处理的内存少了，效率自然就提高了。在这个过程中，集合 B 中的某些内存块由于存活时间长而会被转 移到集合 A 中，当然，集合 A 中实际上也存在一些垃圾，这些垃圾的回收会因为这种分代的机制而被延迟。

**内存池**：

1. Python 的内存机制呈现金字塔形状，-1，-2 层主要有操作系统进行操作
2. 第 0 层是 C 中的 malloc，free 等内存分配和释放函数进行操作
3. 第 1 层和第 2 层是内存池，有 Python 的接口函数 PyMem\_Malloc 函数实现，当对象小于256K 时有该层直接分配内存
4. 第 3 层是最上层，也就是我们对 Python 对象的直接操作，Python 在运行期间会大量地执行 malloc 和 free 的操作，频繁地在用户态和核心态之间进行切换，这将严重影响 Python 的执行效率。为了加速Python的执行效率，Python 引入了一个内存池机制，用于管理对小块内存的申请和释放。

   Python 内部默认的小块内存与大块内存的分界点定在 256 个字节，当申请的内存小于 256 字节时，PyObject\_Malloc 会在内存池中申请内存；当申请的内存大于 256 字节时，PyObject\_Malloc 的行为将蜕化为 malloc 的行为。当然，通过修改 Python 源代码，我们可以改变这个默认值，从而改变 Python 的默认内存管理行为。

**调优手段**：

1. 手动垃圾回收
2. 调高垃圾回收阈值 
3. 避免循环引用（手动解循环引用和使用弱引用）

## 内存泄露是什么？如何避免？

> 指由于疏忽或错误造成程序未能释放已经不再使用的内存的情况。内存泄漏并非指内存在物理上的消失，而是应用程序分配某段内存后，由于设计错误，失去了对该段内存的控制，因而造成了内存的浪费。导致程序运行速度减慢甚至系统崩溃等严重后果。有 del\(\) 函数的对象间的循环引用是导致内存泄漏的主凶。

> 不使用一个对象时使用:del object 来删除一个对象的引用计数就可以有效防止内存泄漏问题。通过 Python 扩展模块 gc 来查看不能回收的对象的详细信息。 可以通过 sys.getrefcount\(obj\) 来获取对象的引用计数，并根据返回值是否为 0 来判断是否内存泄漏。

### 单例

####  请手写一个单例

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

####  单例模式的应用场景有哪些

> 单例模式应用的场景一般发现在以下条件下： 
>
> （1）资源共享的情况下，避免由于资源操作时导致的性能或损耗等。如日志文件，应用配置。 
>
> （2）控制资源的情况下，方便资源之间的互相通信。如线程池等。 1.网站的计数器 2.应用配置 3.多线程池 4. 数据库配置，数据库连接池 5.应用程序的日志应用

### 深拷贝和浅拷贝

### 浅拷贝

* 不可变类型进行浅拷贝不会给拷贝的对象开辟新的内存空间，而只是拷贝引用
* 可变类型进行浅拷贝只对拷贝的第一层对象开辟新的内存空间，子对象不进行拷贝。

```python
import copy  # 使用浅拷贝需要导入copy模块

# 不可变类型有: 数字、字符串、元组
a1 = 123
b1 = copy.copy(a1)  # 使用copy模块里的copy()函数就是浅拷贝了
print(id(a1))  # 140459558944048
print(id(b1))  # 140459558944048
a2 = "abc"
b2 = copy.copy(a2)
print(id(a2))  # 140459558648776
print(id(b2))  # 140459558648776
a3 = (1, 2, ["hello", "world"])
b3 = copy.copy(a3)
# 查看内存地址
print(id(a3))  # 140459558073328
print(id(b3))  # 140459558073328

# 可变类型有: 列表、字典、集合
a1 = [1, 2]
b1 = copy.copy(a1) 
print(id(a1))  # 139882899585608
print(id(b1))  # 139882899585800
a2 = {"name": "hwnzy", "age": 24}
b2 = copy.copy(a2)
print(id(a2))  # 139882919626432
print(id(b2))  # 139882919626504
a3 = {1, 2, "hwnzy"}
b3 = copy.copy(a3)
print(id(a3))  # 139882919321672
print(id(b3))  # 139882899616264
a4 = [1, 2, [4, 5]]
b4 = copy.copy(a4)
print(id(a4))  # 139882899587016
print(id(b4))  # 139882899586952
print(id(a4[2]))  # 139882899693640
print(id(b4[2]))  # 139882899693640

a4[2][0] = 6

print(a4)  # [1, 2, [6, 5]]
print(b4)  # [1, 2, [6, 5]]
```

### 深拷贝

```python
import copy
copy.deepcopy(value)
```

* 不可变类型进行深拷贝如果子对象没有可变类型则不会进行拷贝，而只是拷贝引用
* 只要有一层是可变对象就会对该对象每一层对象进行拷贝，每一层拷贝开辟新的内存空间



### with语句和上下文管理器

### with 语句

with 语句执行完成后即使出现异常也自动调用关闭文件操作

```python
with open("1.txt", "w") as f:
    f.write("hello world")
```

### 上下文管理器

一个类只要实现`__enter__()和__exit__()`两个方法，该类创建的对象就称为上下文管理器。上下文管理器可以使用 with 语句，**with语句之所以这么强大，背后是由上下文管理器做支撑的**，也就是说刚才使用 open 函数创建的文件对象就是就是一个上下文管理器对象。

```python
class File(object):  # 自定义上下文管理器类,模拟文件操作:

    # 初始化方法
    def __init__(self, file_name, file_model):
        # 定义变量保存文件名和打开模式
        self.file_name = file_name
        self.file_model = file_model
    # 上文方法
    def __enter__(self):
        print("进入上文方法")
        # 返回文件资源
        self.file = open(self.file_name,self.file_model)
        return self.file
    # 下文方法
    def __exit__(self, exc_type, exc_val, exc_tb):
        print("进入下文方法")
        self.file.close()

if __name__ == '__main__':

    # 使用with管理文件
    with File("1.txt", "r") as file:
        file_data = file.read()
        print(file_data)
# 进入上文方法
# hello world
# 进入下文方法
```

上下文管理器的另外一种实现方式

```python
# 导入装饰器
from contextlib import contextmanager
# 装饰器装饰函数，让其称为一个上下文管理器对象
@contextmanager
def my_open(path, mode):
    try:
        # 打开文件
        file = open(file_name, file_mode)
        # yield之前的代码好比是上文方法
        yield file
    except Exception as e:
        print(e)
    finally:
        print("over")
        # yield下面的代码好比是下文方法
        file.close()

# 使用with语句
with my_open('out.txt', 'w') as f:
    f.write("hello , the simplest context manager")
```



### 进程线程协程

一个程序运行至少有一个进程，一个进程默认有一个线程，进程里可创建多个线程，线程依附进程。

**进程是操作系统进行资源分配的基本单位**。

### Process进程类的说明

#### Process\(\[group \[, target \[, name \[, args \[, kwargs\]\]\]\]\]\)：

* group：指定进程组，目前只能使用None
* target：执行目标任务名
* name：进程名字，默认为Process-N，N为从1开始递增的整数
* args：以元组方式给执行任务传参
* kwargs：以字典方式给执行任务传参

#### Process创建的实例对象的常用方法：

* start\(\)：启动子进程实例（创建子进程）
* join\(\)：等待子进程执行结束
* terminate\(\)：不管任务是否完成，立即终止子进程

```python
import multiprocessing
import time
import os

def dance():
    print("dance:", os.getpid())  # 获取当前进程的编号
    print("dance:", multiprocessing.current_process())  # 获取当前进程
    print("dance的父进程编号:", os.getppid())  # 获取父进程的编号
    for i in range(5):
        print("跳舞中...")
        time.sleep(0.2)
        os.kill(os.getpid(), 9)  # 扩展:根据进程编号杀死指定进程

def sing():
    print("sing:", os.getpid())
    print("sing:", multiprocessing.current_process())
    print("sing的父进程编号:", os.getppid())
    for i in range(5):
        print("唱歌中...")
        time.sleep(0.2)


if __name__ == '__main__':
    dance_process = multiprocessing.Process(target=dance)
    sing_process = multiprocessing.Process(target=sing)

    dance_process.start()
    sing_process.start()
```

#### 进程执行带有参数的任务

```python
import multiprocessing
import time

def task(count):
    for i in range(count):
        print("running")
        time.sleep(0.2)
    else:
        print("over")


if __name__ == '__main__':
    # 创建子进程
    # args: 以元组的方式给任务传入参数
    sub_process1 = multiprocessing.Process(target=task, args=(5,))
    sub_process2 = multiprocessing.Process(target=task, kwargs={"count": 3})
    sub_process1.start()
    sub_process2.start()
```

#### 进程之间不共享全局变量 <a id="3-&#x8FDB;&#x7A0B;&#x4E4B;&#x95F4;&#x4E0D;&#x5171;&#x4EAB;&#x5168;&#x5C40;&#x53D8;&#x91CF;&#x7684;&#x5C0F;&#x7ED3;"></a>

创建子进程会对主进程资源进行拷贝，子进程是主进程的一个副本，多个进程操作的不是同一个进程里面的全局变量。

#### 主进程会等待所有的子进程执行结束再结束 <a id="4-&#x4E3B;&#x8FDB;&#x7A0B;&#x4F1A;&#x7B49;&#x5F85;&#x6240;&#x6709;&#x7684;&#x5B50;&#x8FDB;&#x7A0B;&#x6267;&#x884C;&#x7ED3;&#x675F;&#x518D;&#x7ED3;&#x675F;"></a>

* multiprocessing.Process\(daemon=True\)
* sub\_process.terminate\(\)

线程是进程中执行代码的分支，每个执行分支要执行代码需cpu调度 ，线程是**cpu调度的基本单位**，每个进程至少都有一个线程，而这个线程就是我们通常说的主线程。

### 线程类Thread参数说明

#### Thread\(\[group \[, target \[, name \[, args \[, kwargs\]\]\]\]\]\)：

* group：线程组，目前只能使用None
* target：执行的目标任务名
* args：以元组的方式给执行任务传参
* kwargs：以字典方式给执行任务传参
* name：线程名，一般不用设置

#### Thread创建的实例对象的常用方法： <a id="3-&#x542F;&#x52A8;&#x7EBF;&#x7A0B;"></a>

* start\(\)：启动子线程实例（创建子线程）

```python
import threading
import time

def sing():
    print("sing当前执行的线程为：", threading.current_thread())
    for i in range(3):
        print("正在唱歌...%d" % i)
        time.sleep(1)

def dance():
    for i in range(3):
        print("正在跳舞...%d" % i)
        time.sleep(1)


if __name__ == '__main__':
    sing_thread = threading.Thread(target=sing)
    dance_thread = threading.Thread(target=dance)
    
    sing_thread.start()
    dance_thread.start()
```

#### 线程执行带有参数的任务

```python
import threading
import time

def task(count):
    for i in range(count):
        print("任务执行中..")
        time.sleep(0.2)
    else:
        print("任务执行完成")


if __name__ == '__main__':
    sub_thread1 = threading.Thread(target=task, args=(5,))
    sub_thread2 = threading.Thread(target=task, kwargs={"count": 3})
    sub_thread1.start()
    sub_thread2.start()
```

#### 线程之间共享全局变量 <a id="3-&#x7EBF;&#x7A0B;&#x4E4B;&#x95F4;&#x5171;&#x4EAB;&#x5168;&#x5C40;&#x53D8;&#x91CF;"></a>

1. 线程等待\(join\)
2. 互斥锁

#### 线程之间执行是无序的 <a id="2-&#x7EBF;&#x7A0B;&#x4E4B;&#x95F4;&#x6267;&#x884C;&#x662F;&#x65E0;&#x5E8F;&#x7684;"></a>

* 线程执行由cpu调度决定
* 进程执行由操作系统决定

#### 主线程会等待所有的子线程执行结束再结束 <a id="3-&#x4E3B;&#x7EBF;&#x7A0B;&#x4F1A;&#x7B49;&#x5F85;&#x6240;&#x6709;&#x7684;&#x5B50;&#x7EBF;&#x7A0B;&#x6267;&#x884C;&#x7ED3;&#x675F;&#x518D;&#x7ED3;&#x675F;"></a>

1. threading.Thread\(daemon=True\)
2. sub\_thread.setDaemon\(True\)

#### 互斥锁

```python
mutex = threading.Lock()  # 创建锁
mutex.acquire()  # 上锁
...这里编写代码能保证同一时刻只能有一个线程去操作, 对共享数据进行锁定...
mutex.release()  # 释放锁
```



### 生成器

 数据不是一次性全部生成处理，而是使用一个，再生成一个，可以**节约大量的内存**。

```python
my_generator = (i * 2 for i in range(5))  # 生成器推导式使用小括号
print(my_generator) # <generator object <genexpr> at 0x101367048>

def my_generater(n):
    for i in range(n):
        yield i  # 代码执行到yield会暂停返回结果，下次启动生成器会在暂停的位置继续往下执行
```

### property属性

### 装饰器方式

```python
class Person(object):
    def __init__(self):
        self.__age = 0

    @property  # 读取age
    def age(self):
        return self.__age

    @age.setter  # 设置age
    def age(self, new_age):
        if new_age >= 120:
            print("too old")
        else:
            self.__age = new_age

p = Person()  # 0
p.age = 100  # 100
p.age = 1000  # too_old
```

### 类属性方式

```python
class Person(object):
    def __init__(self):
        self.__age = 0

    def get_age(self):
        return self.__age

    def set_age(self, new_age):
        if new_age >= 120:
            print("too old")
        else:
            self.__age = new_age

    age = property(get_age, set_age)  #(获取属性, 设置属性)

p = Person()
p.age = 100
p.age = 1000
```

### 闭包和装饰器

### 闭包

1. 函数嵌套
2. 内部函数使用外部函数的变量（包括外部函数参数）
3. 外部函数返回内部函数

```python
def func_out(num1):
    def func_inner(num2):
        result = num1 + num2
        print("result is ", result)
    return func_inner

f = func_out(1)
f(2)  # 3
f(3)  # 4
```

* **闭包保存外部函数的变量，不随外部函数调用完而销毁，所以会消耗内存**

### 修改闭包内使用的外部变量

```python
def func_out(num1):
    def func_inner(num2):
        nonlocal num1  # 告诉解释器此处使用外部变量num1
        num1 = 10
        result = num1 + num2
        print("result is :", result)
    return func_inner

f = func_out(1)
f(2)  # 2
```

### 装饰器

在不改变已有函数源代码及调用方式的前提下，对已有函数进行功能的扩展

```python
def wrapper(func):
  def inner(*args, **kwargs):
    result = func(*args, **kwargs)
    return result
  
  return inner
  
 @wrapper
 def func(*args, **kwargs):
   return args[0] + args[1]
```

### 使用多个装饰器

```python
def make_div(func):
    def inner(*args, **kwargs):
        return "<div>" + func(*args, **kwargs) + "</div>"
    return inner

def make_p(func):
    def inner(*args, **kwargs):
        return "<p>" + func(*args, **kwargs) + "</p>"
    return inner

@make_div
@make_p
def content():
    return "hello"

result = content()  # <div><p>人生苦短</p></div>
```

### 带有参数的装饰器

装饰器只能接收一个函数类型参数，装饰器外再包裹上一个函数，让最外函数接收参数返回装饰器，因为@符号后面必须是装饰器实例。

```python
def wapper(flag):
    def decorator(func):
        def inner(num1, num2):
            if flag == "+":
                print("--加法--")
            elif flag == "-":
                print("--减法--")
            result = func(num1, num2)
            return result
        return inner
    return decorator

@wapper("+")
def add(a, b):
    result = a + b
    return result


@wapper("-")
def sub(a, b):
    result = a - b
    return result

result = add(1, 2)  # 3
result = sub(1, 2)  # -1
```

### 类装饰器

通过定义一个类来装饰函数

```python
class Check(object):
    def __init__(self, fn):
        self.__fn = fn

    # 实现__call__方法，表示对象是一个可调用对象，可以像调用函数一样进行调用。
    def __call__(self, *args, **kwargs):
        # 添加装饰功能
        print("请先登陆...")
        self.__fn()

@Check
def comment():
    print("发表评论")

comment()
```





