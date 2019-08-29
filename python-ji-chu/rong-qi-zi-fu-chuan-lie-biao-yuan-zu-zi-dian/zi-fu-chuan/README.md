# 字符串

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

