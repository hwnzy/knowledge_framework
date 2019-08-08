# 面试题

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

