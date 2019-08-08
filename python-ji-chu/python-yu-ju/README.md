# 基本语句

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



