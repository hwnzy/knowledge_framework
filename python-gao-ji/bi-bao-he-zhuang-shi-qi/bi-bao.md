# 闭包

### 闭包

1. 函数嵌套
2. 内部函数使用外部函数的变量\(包括外部函数参数\)
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

* 闭包保存外部函数的变量，不随外部函数调用完而销毁，消耗内存

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

