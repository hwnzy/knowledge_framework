# 面试题

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

