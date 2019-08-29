# 面试题

## 阅读下面的代码，写出 A0，A1 至 An 的最终值

```python
A0 = dict(zip(('a', 'b', 'c', 'd', 'e'), (1, 2, 3, 4, 5)))
A1 = range(10)
A2 = [i for i in A1 if i in A0]
A3 = [A0[s] for s in A0]
A4 = [i for i in A1 if i in A3]
A5 = {i:i*i for i in A1}
A6 = [[i，i*i] for i in A1]
```

```python
A0 = {'a': 1， 'c': 3， 'b': 2， 'e': 5， 'd': 4}
A1 = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
A2 = []
A3 = [1, 3, 2, 5, 4]
A4 = [1, 2, 3, 4, 5]
A5 = {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81}
A6 = [[0, 0], [1, 1], [2, 4], [3, 9], [4, 16], [5, 25], [6, 36], [7, 49], [8, 64], [9, 81]]
```

##  Python2 中 range 和 xrange 的区别？

> 两者用法相同，不同的是range 返回的结果是一个列表，直接开辟一块内存空间来保存。列表xrange 的结果是一个生成器，是边循环边使用，只有使用时才会开辟内存空间。所以当列表很长时，使用 xrange 性能要比 range 好。

## 考虑以下 Python 代码，如果运行结束，命令行中的运行结果是什么？

```python
l = []
for i in xrange(10):
    l.append({'num' :i})
print l
```

```python
[{‘num’ :0}, {‘num’ :1}, {‘num’ :2}, {‘num’ :3}, {‘num’ :4}, {‘num’ :5}, {‘num’ :6}, {‘num’ :7}, {‘num’ :8}, {‘num’ :9}]
```

```python
l = []
a = {'num' :0}
for i in xrange(10):
    a['num'] = i
    l.append(a)
print l
```

```python
[{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}，{‘num’ :9}]
```

> 字典是可变对象，在下方的l.append\(a\)的操作中把字典a的引用传到列表l中，当后续操作修改a\['num'\]的值时候，l中的值也会跟着改变，相当于浅拷贝

