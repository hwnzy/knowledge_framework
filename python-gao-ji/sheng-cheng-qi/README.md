# 生成器

 数据不是一次性全部生成处理，而是使用一个，再生成一个，可以**节约大量的内存**。

```python
my_generator = (i * 2 for i in range(5))  # 生成器推导式使用小括号
print(my_generator) # <generator object <genexpr> at 0x101367048>

def my_generater(n):
    for i in range(n):
        yield i  # 代码执行到yield会暂停返回结果，下次启动生成器会在暂停的位置继续往下执行
```



