# with语句和上下文管理器

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



