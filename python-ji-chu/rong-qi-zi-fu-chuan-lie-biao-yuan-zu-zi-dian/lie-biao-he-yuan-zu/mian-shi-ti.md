# 面试题

## 下面代码的输出结果将是什么？

```python
list = ['a'， 'b'， 'c'， 'd'， 'e']
print list[10:]
```

下面的代码将输出\[\]，不会产生IndexError错误，list\[10\]会导致IndexError

## 写一个列表生成式，产生一个公差为 11 的等差数列

```python
print([x*11 for x in range(10)])
```

## 给定两个列表，怎么找出他们相同的元素和不同的元素?

```python
list1 = [1，2，3]
list2 = [3，4，5]
set1 = set(list1)
set2 = set(list2)
print(set1 & set2)
print(set1 ^ set2)
```

## 请写出一段 Python 代码实现删除一个 list 里面的重复元素?

* 内置的set

```python
l1 = ['b'，'c'，'d'，'b'，'c'，'a'，'a']
l2 = list(set(l1))
```

* 如果想要保持他们原来的排序

```python
l1 = ['b', 'c', 'd', 'b', 'c', 'a', 'a']
l2 = list(set(l1))
l2.sort(key=l1.index)
```

```python
l1 = ['b', 'c', 'd', 'b', 'c', 'a', 'a']
l2 = sorted(set(l1), key=l1.index)
```

* 使用遍历

```python
l1 = ['b', 'c', 'd', 'b', 'c', 'a', 'a']
l2 = []
for i in l1:
    if not i in l2:
        l2.append(i)
```

## 下面这段代码的输出结果是什么？请解释？

```python
def extendlist(val, list=[]):
    list.append(val)
    return list

list1 = extendlist(10)
list2 = extendlist(123, [])
list3 = extendlist('a')

print("list1 = %s" %list1)
print("list2 = %s" %list2)
print("list3 = %s" %list3)
```

输出结果：

```python
list1 = [10, 'a']
list2 = [123]
list3 = [10, 'a']
```

新的默认列表只在函数被定义的那一刻创建一次。

当 extendList 被没有指定特定参数 list 调用时，这组 list 的值随后将被使用。

带有默认参数的表达式在函数被定义的时候被计算，不是在调用的时候被计算。

## 将以下 3 个函数按照执行效率高低排序

```python
def f1(lIn):
    l1 = sorted(lIn)
    l2 = [i for i in l1 if i<0.5]
    return [i*i for i in l2]

def f2(lIn):
    l1 = [i for i in l1 if i<0.5]
    l2 = sorted(l1)
    return [i*i for i in l2]

def f3(lIn):
    l1 = [i*i for i in lIn]
    l2 = sorted(l1)
    return [i for i in l1 if i<(0.5*0.5)]
```

按执行效率从高到低排列：f2、f1、f3。

要证明这个答案是正确的，你应该知道如何分析自己代码的性能。 

```python
import random
import cProfile
lIn = [random.random() for i in range(100000)]
cProfile.run('f1(lIn)')
cProfile.run('f2(lIn)')
cProfile.run('f3(lIn)')
```

