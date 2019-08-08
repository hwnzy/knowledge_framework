# 有序字典：OrderDict

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

