# 一些小知识

 牛顿迭代法猜测平方根

```python
def sqrt(x):
    y = 1.0
    while abs(y*y-x) > 1e-6:  # 误差是1e-6
        y = (y + x/y) / 2
    return y
```





