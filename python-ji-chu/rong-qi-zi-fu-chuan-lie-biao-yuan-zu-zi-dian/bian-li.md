# 遍历

for ... in ... 

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

enumerate\(\)

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



