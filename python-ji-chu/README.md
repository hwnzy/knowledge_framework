# Python基础

注释

在python的语法规范中中文支持推荐使用的方式：

```text
# -*- coding:utf-8 -*-
```

变量

![&#x53D8;&#x91CF;&#x53CA;&#x5176;&#x7C7B;&#x578B;](../.gitbook/assets/image%20%2893%29.png)

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



