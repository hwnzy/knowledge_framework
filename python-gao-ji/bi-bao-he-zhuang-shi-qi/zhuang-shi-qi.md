# 装饰器

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
   return a + b
```

### 使用多个装饰器

```python
def make_div(func):
    def inner(*args, **kwargs):
        return "<div>" + func() + "</div>"
    return inner

def make_p(func):
    def inner(*args, **kwargs):
        return "<p>" + func() + "</p>"
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
                print("--正在努力加法计算--")
            elif flag == "-":
                print("--正在努力减法计算--")
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



