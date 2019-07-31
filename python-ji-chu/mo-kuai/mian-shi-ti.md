# 面试题

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

浅拷贝会创建新对象，其内容非原对象本身的引用，而是原对象内第一层对象的引用。 浅拷贝有三种形式:切片操作、工厂函数、 copy 模块中的 copy 函数。 

切片操作：`b = a[:]` 或者 `b = [x for x in a]`

工厂函数：`b = list(a)`

copy 函数：`b = copy.copy(a)`

列表 b 列表 a 使用 is 判断可以发现他们不是同一个对象，使用 id 查看也不指向同一片内存空间。但当我们使用 `id(x) for x in a` 和 `id(x) for x in b` 查看 a 和 b 中元素地址，可以看到二者包含的元素的地址是相同的。 

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
    date1 = datetime.date(year=int(year)，month=int(month)，day=int(day))
    date2 = datetime.date(year=int(year)，month=1，day=1)
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

os.remove\(\)删除文件 

os.rename\(\)重命名文件 

os.path.join\(\)将分离的各部分组合成一个路径名

os.path.exists\(\)是否存在

## Python 的 sys 模块常用方法？

`sys.argv` 命令行参数 List，第一个元素是程序本身路径

## 模块和包是什么？

* 模块是搭建程序的一种方式。每一个Python代码文件都是一个模块，并可以引用其他的模块，比如对象和属性。 
* 包是包含许多模块和子文件夹的文件夹。

## unittest 是什么？

unittest 是Python中的单元测试框架。它拥有支持`共享搭建`、`自动测试`、`在测试中暂停代码`、`将不同测试迭代成一组`等功能。

## Python 中有日志吗?怎么使用?

Python 自带`logging`模块，调用 `logging.basicConfig()`方法配置需要的日志等级和相应的参数， Python 解释器会按照配置的参数生成相应的日志。

