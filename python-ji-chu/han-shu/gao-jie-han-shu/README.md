# 高阶函数

### map 用法 <a id="1-map-&#x7528;&#x6CD5;"></a>

**map\(\)** 会根据提供的函数对指定序列做映射

第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表

**计算每一个元素的平方值:**

```python
my_list = [1, 2, 3, 4, 5]

def f(x):
    return x ** 2

result = map(f, my_list)
print(type(result), result, list(result))
```

```python
<class 'map'> <map object at 0x000000C9729591D0> [1, 4, 9, 16, 25]
```

**首字母大写:**

```python
my_list = ['smith', 'edward', 'john', 'obama', 'tom']

def f(x):
    return x.capitalize()

result = map(f, my_list)
print(list(result))
```

```python
['Smith', 'Edward', 'John', 'Obama', 'Tom']
```

### reduce 用法 <a id="2-reduce-&#x7528;&#x6CD5;"></a>

**reduce\(\)** 函数会对参数序列中元素进行累计

函数将一个数据集合中的所有数据进行下列操作：

1. 用传给 reduce 中的函数 function（有两个参数）先对集合中的第 1、2 个元素进行操作
2. 得到的结果再与第三个数据用 function 函数运算, 最后得到一个结果

**计算列表中的累加和:**

```python
import functools

my_list = [1, 2, 3, 4, 5]

def f(x1, x2):
    return x1 + x2

result = functools.reduce(f, my_list)
print(result)
```

输出结果:

```python
15
```

### filter 用法 <a id="3-filter-&#x7528;&#x6CD5;"></a>

**filter\(\)** 函数用于过滤序列, 过滤掉不符合条件的元素, 返回一个 filter 对象, 如果要转换为列表, 可以使用 list\(\) 来转换.

该接收两个参数, 第一个为函数, 第二个为序列, 序列的每个元素作为参数传递给函数进行判, 然后返回 True 或 False, 最后将返回 True 的元素放到新列表中.

**过滤列表中的偶数:**

```python
my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

def f(x):
    return x % 2 == 0

result = filter(f, my_list)
print(list(result))
```

输出结果:

```python
[2, 4, 6, 8, 10]
```

**过滤列表中首字母为大写的单词:**

```python
my_list = ['edward', 'Smith', 'Obama', 'john', 'tom']

def f(x):
    return x[0].isupper()

result = filter(f, my_list)
print(list(result))
```

输出结果:

```python
['Smith', 'Obama']
```



