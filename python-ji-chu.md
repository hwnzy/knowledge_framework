# Python基础

## Python基础

注释

在python的语法规范中中文支持推荐使用的方式：

```text
# -*- coding:utf-8 -*-
```

变量

![&#x53D8;&#x91CF;&#x53CA;&#x5176;&#x7C7B;&#x578B;](.gitbook/assets/image%20%2897%29.png)

关键字

可以在Python Shell通过以下命令进行查看当前系统中python的关键字

```text
>>> import keyword
>>> keyword.kwlist
```

输出

```text
print("我的姓名是%s, 年龄是%d" % (name, age))
```

| 格式符号 | 转换 |
| :--- | :--- |
| %c | 字符 |
| %s | 字符串 |
| %d | 有符号十进制整数 |
| %u | 无符号十进制整数 |
| %o | 八进制整数 |
| %x | 十六进制整数（小写字母0x） |
| %X | 十六进制整数（大写字母0X） |
| %f | 浮点数 |
| %e | 科学计数法（小写'e'） |
| %E | 科学计数法（大写“E”） |
| %g | ％f和％e 的简写 |
| %G | ％f和％E的简写 |

输入

raw\_input\(python2\) == input\(python3\)

```python
password = raw_input("请输入密码:")  # 小括号中放入的是提示信息
print('您刚刚输入的密码是:%d' % password)  # 输入的任何值都作为字符串
```

input\(python2\)，其接受的输入必须是表达式

```python
>>> a = input()
abc
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<string>", line 1, in <module>
NameError: name 'abc' is not defined
>>> a = input()
"abc"+"def"
>>> a
'abcdef'
```

运算符

| 运算符       | 描述 | 实例 |
| :--- | :--- | :--- |
| + | 加 | 两个对象相加 a + b 输出结果 30 |
| - | 减 | 得到负数或是一个数减去另一个数 a - b 输出结果 -10 |
| \* | 乘 | 两个数相乘或是返回一个被重复若干次的字符串 a \* b 输出结果 200 |
| / | 除 | b / a 输出结果 2 |
| // | 取整除 | 返回商的整数部分 9//2 输出结果 4 , 9.0//2.0 输出结果 4.0 |
| % | 取余 | 返回除法的余数 b % a 输出结果 0 |
| \*\* | 指数 | a\*\*b 为10的20次方， 输出结果 100000000000000000000 |
| = | 赋值运算符 | 把 = 号右边的结果 赋给 左边的变量，如 num = 1 + 2 \* 3，结果num的值为7 |
| += | 加法赋值运算符 | c += a 等效于 c = c + a |
| -= | 减法赋值运算符 | c -= a 等效于 c = c - a |
| \*= | 乘法赋值运算符 | c \*= a 等效于 c = c \* a |
| /= | 除法赋值运算符 | c /= a 等效于 c = c / a |
| %= | 取模赋值运算符 | c %= a 等效于 c = c % a |
| \*\*= | 幂赋值运算符 | c \*\*= a 等效于 c = c \*\* a |
| //= | 取整除赋值运算符          | c //= a 等效于 c = c // a |

数据类型转换

| 类型转换操作 | 说明 |
| :--- | :--- |
| int\(x \[,base \]\) | 将x转换为一个整数 |
| float\(x \) | 将x转换为一个浮点数 |
| complex\(real \[,imag \]\) | 创建一个复数，real为实部，imag为虚部 |
| str\(x \) | 将对象 x 转换为字符串 |
| repr\(x \) | 将对象 x 转换为表达式字符串 |
| eval\(str \) | 将字符串转成原始数据类型 |
| tuple\(s \) | 将序列 s 转换为一个元组 |
| list\(s \) | 将序列 s 转换为一个列表 |
| chr\(x \) | 将一个整数转换为一个字符 |
| ord\(x \) | 将一个字符转换为它的ASCII整数值 |
| hex\(x \) | 将一个整数转换为一个十六进制字符串 |
| oct\(x \) | 将一个整数转换为一个八进制字符串 |
| bin\(x \) | 将一个整数转换为一个二进制字符串 |

### if

```text
    if 要判断的条件:
        条件成立时，要做的事情
```

### if-else

```text
    if 条件:
        满足条件时要做的事情
    else:
        不满足条件时要做的事情
```

### if...elif...else...

```text
    if xxx1:
        事情1
    elif xxx2:
        事情2
    else xxx3:
        事情3
```

### if 实现三目运算操作

```text
a if a > b else b  # 如果 a > b的条件成立,三目运算的结果是a,否则就是b
```

### while

```text
    while 条件:
        条件满足时，做的事情1
        条件满足时，做的事情2
        条件满足时，做的事情3
        ...(省略)...
```

### for

```text
for 临时变量 in 列表或者字符串等可迭代对象:
    循环满足条件时执行的代码
    
```

### break和continue

```text
    if 条件1:
        break  # 立刻结束break所在的循环
    if 条件2:
        continue  # 用来结束本次循环，紧接着执行下一次的循环
```

| 运算符      | 描述 | 示例 |
| :--- | :--- | :--- |
| == | 检查两个操作数的值是否相等，如果是则条件变为真。 | 如a=3,b=3，则（a == b\) 为 True |
| != | 检查两个操作数的值是否相等，如果值不相等，则条件变为真。 | 如a=1,b=3，则\(a != b\) 为 True |
| &gt; | 检查左操作数的值是否大于右操作数的值，如果是，则条件成立。 | 如a=7,b=3，则\(a &gt; b\) 为 True |
| &lt; | 检查左操作数的值是否小于右操作数的值，如果是，则条件成立。 | 如a=7,b=3，则\(a &lt; b\) 为 False |
| &gt;= | 检查左操作数的值是否大于或等于右操作数的值，如果是，则条件成立。 | 如a=3,b=3，则\(a &gt;= b\) 为 True |
| &lt;= | 检查左操作数的值是否小于或等于右操作数的值，如果是，则条件成立。 | 如a=3,b=3，则\(a &lt;= b\) 为 True |

| 运算符          | 逻辑表达式              | 描述 | 实例 |
| :--- | :--- | :--- | :--- |
| and | x and y | 布尔"与"：如果 x 为 False，x and y 返回 False，否则它返回 y 的值。 | True and False， 返回 False。 |
| or | x or y | 布尔"或"：如果 x 是 True，它返回 True，否则它返回 y 的值。 | False or True， 返回 True。 |
| not | not x | 布尔"非"：如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。 | not True 返回 False, not False 返回 True |



### 字符串

```python
# 单引号或者双引号或者三引号中的数据，就是字符串
a = "hello"
a = 'hello'
a = '''hello'''
a = """hello"""  # hello
b = "%s" % a  # hello
c = f'{a}, 1+1={1+1}, {{a}}'  # 'hello, 1+1=2, {a}'
```

### 字符串常见操作

```python
mystr = ' hello world '
# 如果包含str返回开始的索引值，否则返回-1
mystr.find(str, start=0, end=len(mystr))
# 跟find()一样，但str不在mystr会报异常
mystr.index(str, start=0, end=len(mystr))  
# 返回str在start和end之间在mystr里面出现的次数
mystr.count(str, start=0, end=len(mystr))
# 把mystr的str1替换成str2,如果count指定，则替换不超过count次
mystr.replace(str1, str2,  mystr.count(str1))
# 以str为分隔符切片mystr，如果maxsplit有指定值则仅分隔maxsplit+1个子字符串
mystr.split(str, maxsplit=2)
# 把字符串的第一个字符大写
mystr.capitalize()
# 把字符串的每个单词首字母大写
mystr.title()
# 检查字符串是否是以 hello 开头, 是则返回 True，否则返回 False
mystr.startswith('hello')
# 检查字符串是否以rld结束，如果是返回True,否则返回 False
mystr.endswith('rld')
# 转换 mystr 中所有大写字符为小写
mystr.lower() 
# 转换 mystr 中的小写字母为大写
mystr.upper()
# 删除 mystr 左边的空白字符
mystr.lstrip()
# 删除 mystr 字符串末尾的空白字符
mystr.rstrip()
# 删除mystr字符串两端的空白字符
mystr.strip()
# rfind()类似于find()函数，不过是从右边开始查找
mystr.rfind(str, start=0,end=len(mystr))
# rindex()类似于index()，不过是从右边开始
mystr.rindex(str, start=0,end=len(mystr))
# 把mystr以str分割成三部分，str前，str和str后
mystr.partition(str)
# rpartition()类似于 partition()函数,不过是从右边开始
mystr.rpartition(str)
# 按照行分隔，返回一个包含各行作为元素的列表
mystr.splitlines()
# 如果 mystr 所有字符都是字母 则返回 True,否则返回 False
mystr.isalpha()
# 如果 mystr 只包含数字则返回 True 否则返回 False
mystr.isdigit()
# 如果 mystr 所有字符都是字母或数字则返回 True,否则返回 False
mystr.isalnum()
# 如果 mystr 中只包含空格，则返回 True，否则返回 False
mystr.isspace()
# mystr中每个元素后面插入str,构造出一个新的字符串
mystr.join(str)
```

### 列表

```python
testList1 = [1, 'a']  # 元素可以是不同类型的
```

### 添加元素

```python
testList1.append('b')  # 添加元素 [1, 'a', 'b']
testList2 = [2, 'c']
testList1.append(testList2)  # [1, 'a', 'b', [2, 'c']]
testList1.extend(testList2)  # [1, 'a', 'b', [2, 'c'], 2, 'c']
testList1.insert(1, 3) # insert(index, object) 在指定位置index前插入元素object 
# [1, 3, 'a', 'b', [2, 'c'], 2, 'c']
```

### 查找元素

* in（存在）,如果存在那么结果为true，否则为false
* not in（不存在），如果不存在那么结果为true，否则false

```python
if a in b:
        print('b中有a')
elif a not in b:
        print('b中没有a')
```

* index，与字符串中的用法相同
* count，与字符串中的用法相同

### 删除元素

* del：根据下标进行删除  del list\[1\]
* pop：删除最后一个元素  list.pop\(\)
* remove：根据元素的值进行删除  list.remove\(value\)

### 元素排序

```python
>>> a = [1, 4, 2, 3]
>>> a.reverse()
>>> a
[3, 2, 4, 1]
>>> a.sort()
>>> a
[1, 2, 3, 4]
>>> a.sort(reverse=True)
>>> a
[4, 3, 2, 1]
```

### 元组

 元组与列表类似，不同之处在于**元组的元素不能修改，**元组使用小括号，列表使用方括号。

### 索引和切片

```python
# 索引是通过下标取某一个元素
# 切片是通过下标去某一段元素
s = 'Hello World!'  
print(s[4]) # o
print(s[:]) # 取出所有元素（没有起始位和结束位之分），默认步长为1 Hello World!
print(s[1:]) # 从下标为1开始，取出 后面所有的元素（没有结束位）ello World!
print(s[:5])  # 从起始位置开始，取到 下标为5的前一个元素（不包括结束位本身）Hello
print(s[:-1]) # 从起始位置开始，取到 倒数第一个元素（不包括结束位本身）Hello World
print(s[-4:-1]) # 从倒数第4个元素开始，取到倒数第1个元素（不包括结束位本身）rld
print(s[1:5:2]) # 从下标为1开始，取到下标为5的前一个元素，步长为2（不包括结束位本身）el
print(s[::-1])  #字符串快速逆置 从后向前，按步长为1进行取值 !dlroW olleH
```



```python
info = {'name':'hwnzy', 'id':1, 'sex':'man', 'address':'北京'}
info['name']  # 根据键访问值，访问不存在的键抛出KeyError
info.get('age')  # 不确定是否存在某个键又想获取其值，使用get方法
info.get('age', 24)  # 设置默认值，若info中不存在'age'这个键，就返回默认值24
```

### 修改和删除

 **变量名\[键\] = 数据**   \# 键在字典中则修改元素，不存在则新增元素

对字典进行删除操作，有一下几种：

* del dict\[key\]  \# del 删除指定的元素，也可以del dict 删除整个字典
* dict.clear\(\)  \# 清空整个字典

常见操作

```python
dict = {"name": 'hwnzy', 'sex': 'man'}
len(dict)  # 2 测量字典中，键值对的个数
dict.keys()  # 测量字典中，键值对的个数 ['name', 'sex']
dict.values()  # 返回包含字典所有KEY的列表 ['hwnzy', 'man']
dict.items()  # 返回包含所有（键，值）元祖的列表 [('name', 'hwnzy'), ('sex', 'man')]
```

## for ... in ... 

可以遍历字符串、列表、元组、字典等

```python
a_str = "hello"
for char in a_str:
     print(char,end=' ')
a_list = [1, 2, 3, 4, 5]
for num in a_list:
     print(num,end=' ')
a_turple = (1, 2, 3, 4, 5)
for num in a_turple:
    print(num,end=" ")
```

## enumerate\(\)

用于将一个可遍历的数据对象\(如列表、元组或字符串\)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中。

```python
>>> chars = ['a', 'b', 'c', 'd']
>>> for i, chr in enumerate(chars):
...     print i, chr
...
0 a
1 b
2 c
3 d
```

运算符

| 运算符 | Python 表达式 | 结果 | 描述 | 支持的数据类型 |
| :--- | :--- | :--- | :--- | :--- |
| + | \[1, 2\] + \[3, 4\] | \[1, 2, 3, 4\] | 合并 | 字符串、列表、元组 |
| \* | \['Hi'\] \* 4 | \['Hi', 'Hi', 'Hi', 'Hi'\] | 复制 | 字符串、列表、元组 |
| in | 3 in \(1, 2, 3\) | True | 元素是否存在 | 字符串、列表、元组、字典 |
| not in | 4 not in \(1, 2, 3\) | True | 元素是否不存在 | 字符串、列表、元组、字典 |

内置函数

| 序号 | 函数 | 描述 |
| :--- | :--- | :--- |
| 1 | len\(item\) | 计算容器中元素个数 |
| 2 | max\(item\) | 返回容器中元素最大值 |
| 3 | min\(item\) | 返回容器中元素最小值 |
| 4 | del\(item\) | 删除变量 |

dict 是无序的，如果想输出顺序和操作顺序一致, 需要使用有序字典

```python
from collections import OrderedDict

# 创建有序字典
my_dict = OrderedDict()
# 向字典中添加元素
my_dict['one'] = 1
my_dict['two'] = 2
my_dict['three'] = 3
my_dict['four'] = 4
print(my_dict)
```

输出结果

```python
OrderedDict([('one', 1), ('two', 2), ('three', 3), ('four', 4)])
```

在 Python3.6 版本中, dict 字典已经经过优化, 变为有序字典. 并且字典的内存占用减少了20％到25％

```python
# 创建无序字典
my_dict = dict()
# 向字典中添加元素
my_dict['one'] = 1
my_dict['two'] = 2
my_dict['three'] = 3
my_dict['four'] = 4
print(my_dict)
```

输出结果

```python
{'one': 1, 'two': 2, 'three': 3, 'four': 4}
```

#### set是集合类型 <a id="set&#x662F;&#x96C6;&#x5408;&#x7C7B;&#x578B;"></a>

#### set、list、tuple之间可以相互转换 <a id="set&#x3001;list&#x3001;tuple&#x4E4B;&#x95F4;&#x53EF;&#x4EE5;&#x76F8;&#x4E92;&#x8F6C;&#x6362;"></a>

#### 使用set，可以快速的完成对list中的元素去重复的功能 <a id="&#x4F7F;&#x7528;set&#xFF0C;&#x53EF;&#x4EE5;&#x5FEB;&#x901F;&#x7684;&#x5B8C;&#x6210;&#x5BF9;list&#x4E2D;&#x7684;&#x5143;&#x7D20;&#x53BB;&#x91CD;&#x590D;&#x7684;&#x529F;&#x80FD;"></a>

### 函数

函数是具有独立功能的代码块，用于提高代码复用性。

```python
def function()
    '''
    函数的相关说明
    使用help(函数名)查看
    '''
    num1 = 1  # 局部变量
    return 返回值1, 返回值2  # 默认是元组
function()  # 调用函数
```

#### 局部变量

* Python中局部变量中就是在函数内部定义的变量
* 作用范围是函数内部，只能函数中使用

#### 全局变量

* 在函数外定义的变量叫做全局变量
* 全局变量能在所有函数中访问

#### 全局变量和局部变量名字相同问题 <a id="2-&#x5168;&#x5C40;&#x53D8;&#x91CF;&#x548C;&#x5C40;&#x90E8;&#x53D8;&#x91CF;&#x540D;&#x5B57;&#x76F8;&#x540C;&#x95EE;&#x9898;"></a>

* `变量名 = 数据`函数内出现局部变量和全局变量相同名字时，应理解为定义了一个局部变量，而不是修改全局变量的值。

#### 函数内修改全局变量 <a id="3-&#x4FEE;&#x6539;&#x5168;&#x5C40;&#x53D8;&#x91CF;"></a>

```python
a = 1
def fun():
    global a
    a = 2
fun()
print(a)  # 2
```

#### 函数参数

* `缺省参数`：形参中有默认值的参数，缺省参数一定要位于参数列表的最后面
* `不定长参数`：能处理比当初声明时更多的参数, 这些参数叫不定长参数，声明时不命名
  * 加了\*的变量args会存放`未命名变量参数`，args为元组
  * 加\*\*的变量kwargs会存放`命名参数`，即形如key=value的参数， kwargs为字典.

```python
def functionname([formal_args,] *args, **kwargs):
   """函数_文档字符串"""
   function_suite
   return [expression]
```

* 如果很多个值都是不定长参数，那么这种情况下，可以将缺省参数放到 \*args的后面， 但如果有\*\*kwargs的话，\*\*kwargs必须是最后的

```python
def sum_nums_3(a, *args, b=22, c=33, **kwargs):
    print(a)
    print(b)
    print(c)
    print(args)
    print(kwargs)

sum_nums_3(100, 200, 300, 400, 500, 600, 700, b=1, c=2, mm=800, nn=900)

# 100
# 1
# 2
# (200, 300, 400, 500, 600, 700)
# {'mm': 800, 'nn': 900}

```

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



1. 函数可以做为参数传递给另外一个函数, 可以使得函数的实现更加通用.
2. 匿名函数也可以作为参数传递给另外一个函数, 对于只需要用到一次函数, 可以通过匿名函数减少代码量.



* 所谓可变类型与不可变类型是指：数据能够直接进行修改，如果能直接修改那么就是可变，否则是不可变
* 可变类型有： 列表、字典、集合
* 不可变类型有： 数字、字符串、元组



**在python中，值是靠引用来传递来的**

我们可以用`id()`来判断两个变量是否为同一个值的引用。 我们可以将id值理解为那块内存的地址标示

* Python中函数参数是引用传递（注意不是值传递）
* 对于不可变类型，因变量不能修改，所以运算不会影响到变量自身
* 而对于可变类型来说，函数体中的运算有可能会更改传入的参数变量

对返回的数据直接拆包

```text
def func():
    a = 1
    b= 2
    c = 3
    return a, b, c

a, b, c = func()
```

* 拆包时注意拆的数据个数要与变量的个数相同，否则程序会异常
* 除了对元组拆包之外，还可以对列表、字典等拆包

交换2个变量的值

```text
a = 1
b = 2
a, b = b, a
```

面向对象三大特性：封装、继承、多态

定义一个类，格式如下：

```text
class 类名(object):
    方法列表
```

* 定义类时有2种形式：经典类：`class 类名:`新式类：`class 类名(object):`
* object 是Python 里所有类的最顶级父类
* 类名命名规则按照“大驼峰命名法”
* 实例方法第一个参数一般是self，表示**实例对象**，self可换为其它的名字
* 实例方法是共享的，只占一份内存空间，类通过self判断是哪个对象调用了实例方法
* 一个类有多个对象，每个对象属性各自都有各自独立的地址

### 类方法 <a id="1-&#x7C7B;&#x65B9;&#x6CD5;"></a>

需用修饰器`@classmethod`标识其为类方法。类方法第一个参数必须是类对象，一般以`cls`作为第一个参数。用来获取或修改类属性。

```python
class People(object):
    country = 'china'

    @classmethod  # 类方法，用classmethod来进行修饰
    def get_country(cls):
        return cls.country
    @classmethod
    def set_country(cls,country):
        cls.country = country

p = People()
print(p.get_country())    # 可以用过实例对象引用
p.set_country('japan') 
print(People.get_country())    # 可以通过类对象引用
```

结果显示在用类方法对类属性修改之后，**通过类对象和实例对象访问都发生了改变**

### 静态方法 <a id="2-&#x9759;&#x6001;&#x65B9;&#x6CD5;"></a>

需要通过修饰器`@staticmethod`来进行修饰，静态方法不需要多定义参数，可以通过对象和类来访问。

```python
class People(object):
    country = 'china'

    @staticmethod 
    def get_country():  # 静态方法
        return People.country

p = People()
p.get_contry() # 通过对象访问静态方法
print(People.get_country()) # 通过类访问静态方法
```

### 类属性

```python
class People(object):
    name = 'Tom'  # 公有的类属性
    __age = 12  # 私有的类属性
People.name  #类属性
```

### 实例属性\(对象属性\)

```python
class People(object):
    name = 'Tom'  # 公有的类属性
    __age = 12  # 私有的类属性

p = People()
p.name  # 实例属性
```

### 通过实例\(对象\)去修改类属性

类外修改`类属性`，必须通过`类对象`引用后进行修改。



在需要使用父类对象的地方也可以使用子类对象，这种情况就叫多态

#### 如何在程序中使用多态? <a id="&#x5982;&#x4F55;&#x5728;&#x7A0B;&#x5E8F;&#x4E2D;&#x4F7F;&#x7528;&#x591A;&#x6001;"></a>

```python
'''
可以按照以下几个步骤来写代码:
    1.子类继承父类
    2.子类重写父类中的方法
    3.通过对象调用这个方法
'''
class Father:
    def cure(self):
        print("父亲给病人治病...")

class Son(Father):
    # 重写父类中的方法
    def cure(self):
        print("儿子给病人治病...")

def call_cure(doctor):
    doctor.cure()

father = Father()
call_cure(father)
son = Son()
call_cure(son)
```



### 使用多态的好处

> 给call\_cure\(doctor\)函数传递哪个对象，在它里面就会调用哪个对象的cure\(\)方法，让call\_cure\(doctor\)函数变得更加灵活，额外增加了它的功能，提高了它的扩展性。



### **封装的意义：**

* 将属性和方法做为整体，实例化对象处理
* 隐藏内部实现细节
* 对类的属性和方法增加访问权限控制

### 保护变量：在属性名前面加上一个下划线 \_

* 有类对象（即类实例）和子类对象自己能访问到这些变量，需通过类提供的接口进行访问
* 不能用 `from module import *` 导入

### 私有权限：在属性名和方法名前面加上两个下划线 \_\_

* 类的私有属性和私有方法只能在本类内部访问
* 类的私有属性和私有方法不会被子类继承，子类无法访问

### 修改私有属性的值 <a id="&#x4FEE;&#x6539;&#x79C1;&#x6709;&#x5C5E;&#x6027;&#x7684;&#x503C;"></a>

* 修改私有属性的值：定义一个可以调用的公有方法，~~在~~这个公有方法内访问修改。

### 继承的意义

* 继承是多个类之间的所属关系。
* 类A的属性和方法可复用，可通过继承传递到类B。
* 类A就是基类，也叫做父类，类B就是派生类，也叫做子类。

### 单继承：子类只继承一个父类 <a id="&#x5355;&#x7EE7;&#x627F;&#xFF1A;&#x5B50;&#x7C7B;&#x53EA;&#x7EE7;&#x627F;&#x4E00;&#x4E2A;&#x7236;&#x7C7B;"></a>

```python
# 父类
class A(object):
    def __init(self):
        self.num = 1
    def show_num(self):
        print(self.num)
# 子类
class B(A):
    pass
b = B()  # 创建子类实例对象
b.num  # 子类对象可以直接使用父类属性
b.show_num()  # 子类对象可以直接使用父类的方法
```

* 虽然子类没有定义`__init__`方法初始化属性，也没有定义实例方法，但是只要创建子类的对象，就默认执行了继承过来的`__init__`方法

### 多继承：子类继承多个父类

* 多继承继承了所有父类的属性和方法
* 如果多个父类中有同名的属性和方法，默认使用第一个父类的属性和方法（根据类的魔法属性`__mro__`的顺序来查找）

```python
# 父类
class A(object):
    pass
# 父类
class B(object):
    pass
# 子类
class C(A, B):
    pass
```

### 子类重写父类的同名属性和方法

* 子类和父类有同名属性或方法，默认使用子类的

### 子类调用父类同名属性和方法

```python
父类名.父类方法(self)
父类名.父类属性
```

### 多层继承

```python
# 父类
class A(object):
    pass
# 父类
class B(A):
    pass
# 子类
class C(B):
    pass
```

### super\(\)的使用 <a id="super&#x7684;&#x4F7F;&#x7528;"></a>

super\(\) 可逐一调用所有父类方法，且只执行一次。调用顺序遵循 `__mro__` 类属性顺序。



在python中方法名如果是`__xxxx__()`的，那么就有特殊的功能，因此叫做“魔法”方法

### `__init__()`方法

* `__init__()`方法，在创建一个对象时默认被调用，不需要手动调用
* `__init__(self)`中的self参数，不需要开发者传递，python解释器会自动把当前的对象引用传递过去。
* `__init__(self)`中，如果在创建对象时传递2个实参，那么还需要2个形参如`__init__(self,x,y)`

### `__str__()`方法

* 当使用print输出对象的时候，默认打印对象的内存地址。如果类定义了`__str__(self)`方法，那么就会打印从在这个方法中 `return` 的数据
* `__str__`方法通常返回一个字符串，作为这个对象的描述信息

### `__del__()`方法

* 当删除对象时，python解释器默认调用`__del__()`方法
* 当有变量保存了一个对象的引用时，此对象的引用计数就会加1；
* 当使用del\(\) 删除变量指向的对象时，则会减少对象的引用计数。如果对象的引用计数不为1，那么会让这个对象的引用计数减1，当对象的引用计数为0的时候，则对象才会被真正删除（内存被回收）。

## 异常简介

当Python检测错误，解释器无法继续执行，出现错误提示，这就是异常

```python
try:
    f = open('1.txt','r') # 把可能出现问题的代码，放在try中
except Exception as result:  # 捕获所有异常，并且存储异常的基本信息
    print(result) # 把处理异常的代码，放在except中   
else:  # 即如果没有捕获到异常，那么就执行else中的事情
    print('0 error')
finally:  # 即无论异常是否产生都要执行
    f.close()  # 关闭文件
    
try:  # 捕获多个异常的方式
    open('1.txt','r') # 把可能出现问题的代码，放在try中
except (IOError,NameError): 
    pass  # 如果想通过一次except捕获到多个异常可以用一个元组的方式
```

## 异常的传递

* try嵌套，如果里面的try没有捕获到这个异常，外面的try会接收到这个异常进行处理，如果没有捕获到再进行传递。

## 抛出自定义的异常

用raise语句引发一个异常。异常/错误对象必须有名字，它们应是Error或Exception类的子类。

```python
class ShortInputException(Exception):
    '''自定义的异常类'''
    def __init__(self, length, atleast):
        self.length = length
        self.atleast = atleast

def main():
    try:
        s = input()
        if len(s) < 3:
            raise ShortInputException(len(s), 3) # raise 引发一个你定义的异常
    except ShortInputException as result:#x这个变量被绑定到了错误的实例
        print('ShortInputException: 输入的长度是 %d,长度至少应是 %d'% (result.length, result.atleast))
    else:
        print('没有异常发生.')
main()
```



## 模块

每个Python文件都可以作为一个模块，模块的名字就是文件的名字。导入模块，Python解析器对模块位置的搜索顺序：

1. 当前目录
2. 如果不在当前目录，Python则搜索在shell变量PYTHONPATH下的每个目录。
3. 如果都找不到，Python会察看默认路径。UNIX下默认路径一般为/usr/local/lib/python/
4. 模块搜索路径存储在system模块的sys.path变量中。变量里包含当前目录，PYTHONPATH和由安装过程决定的默认目录。

```text
from modname  # modname.function_name()
from modname import name1[, name2[, ... nameN]]  # from 模块名 import 函数名1,函数名2...
from modname import * # 导入全部内容，这种声明不该被过多地使用，容易产生冲突
import modname as rename  # 重命名，为了方便用短名字替换模块名使用
```

## 包

* 包将有联系的模块组织在同一个文件夹下，并在该文件夹下创建名字为`__init__.py` 文件

#### `__init__.py` 控制着包的导入行为

* `__init__.py`为空：仅仅是把这个包导入，不会导入包中的模块
* `__init__.py`中定义一个`__all__`变量，它控制着 from 包名 import \*时导入的模块，如：`__all__ =  ['className', 'function_name']`
* `__init__.py`中编写内容，当导入时这些语句就会被执行



PEP8 提供了 Python 代码的编写约定. 本节知识点旨在提高代码的可读性, 并使其在各种 Python 代码中编写风格保持一致.

1. 缩进使用4个空格, 空格是首选的缩进方式. Python3 不允许混合使用制表符和空格来缩进.
2. 每一行最大长度限制在79个字符以内.
3. 顶层函数、类的定义, 前后使用两个空行隔开.
4. import 导入
   1. 导入建议在不同的行, 例如:

      ```text
      import os
      import sys
      # 不建议如下导包
      import os, sys
      # 但是可以如下:
      from subprocess import Popen, PIPE
      ```

   2. 导包位于文件顶部, 在模块注释、文档字符串之后, 全局变量、常量之前. 导入按照以下顺序分组:
      1. 标准库导入
      2. 相关第三方导入
      3. 本地应用/库导入
      4. 在每一组导入之间加入空行
5. Python 中定义字符串使用双引号、单引号是相同的, 尽量保持使用同一方式定义字符串. 当一个字符串包含单引号或者双引号时, 在最外层使用不同的符号来避免使用反斜杠转义, 从而提高可读性.
6. 表达式和语句中的空格:
   1. 避免在小括号、方括号、花括号后跟空格.
   2. 避免在逗号、分好、冒号之前添加空格.
   3. 冒号在切片中就像二元运算符, 两边要有相同数量的空格. 如果某个切片参数省略, 空格也省略.
   4. 避免为了和另外一个赋值语句对齐, 在赋值运算符附加多个空格.
   5. 避免在表达式尾部添加空格, 因为尾部空格通常看不见, 会产生混乱.
   6. 总是在二元运算符两边加一个空格, 赋值（=），增量赋值（+=，-=），比较（==,&lt;,&gt;,!=,&lt;&gt;,&lt;=,&gt;=,in,not,in,is,is not），布尔（and, or, not
7. 避免将小的代码块和 if/for/while 放在同一行, 要避免代码行太长.

   ```text
   if foo == 'blah': do_blah_thing()
   for x in lst: total += x
   while t < 10: t = delay()
   ```

8. 永远不要使用字母 'l'\(小写的L\), 'O'\(大写的O\), 或者 'I'\(大写的I\) 作为单字符变量名. 在有些字体里, 这些字符无法和数字0和1区分, 如果想用 'l', 用 'L' 代替.
9. 类名一般使用首字母大写的约定.
10. 函数名应该小写, 如果想提高可读性可以用下划线分隔.
11. 如果函数的参数名和已有的关键词冲突, 在最后加单一下划线比缩写或随意拼写更好. 因此 class\_ 比 clss 更好.\(也许最好用同义词来避免这种冲突\).
12. 方法名和实例变量使用下划线分割的小写单词, 以提高可读性



## 你所遵循的代码规范是什么？请举例说明其要求？

PEP8 规范。 

*  变量 
  * 常量：大写加下划线 USER_CONSTANT。_ 
  * 私有变量 : 小写和前导下划线 \_private\_value，只是约定而已。
  * 内置变量 : 小写，两个前导下划线和两个后置下划线 \_\_class\_\_，两个前导下划线会导致变量在解释期间被更名。
* 函数和方法

  > 总体而言应该使用，小写和下划线。

  * 私有方法 ：小写和一个前导下划线 
  * 特殊方法 ：小写和两个前导下划线，两个后置下划线 ，这种风格只应用于特殊函数，比如操作符重载等。 
  * 函数参数 : 小写和下划线，缺省值等号两边无空格

* 类

  > 类总是使用驼峰格式命名，即所有单词首字母大写其余字母小写。

* 模块和包

  > 除特殊模块 init 之外，模块名称都使用不带下划线的小写字母。若是它们实现一个协议，那么通常使用 lib 为后缀，例如:smtplib

* 一行列数

  > PEP 8 规定为 79 列。根据自己的情况，比如不要超过满屏时编辑器的显示列数。

* 一个函数

  > 不要超过 30 行代码, 即可显示在一个屏幕类，可以不使用垂直游标即可看到整个函数。

* 一个类

  > 不要超过 200 行代码，不要有超过 10 个方法。一个模块不要超过 500 行。

## Python高级

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





