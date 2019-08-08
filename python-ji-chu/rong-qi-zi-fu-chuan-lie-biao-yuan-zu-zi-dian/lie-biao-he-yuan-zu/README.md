# 列表和元组

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

