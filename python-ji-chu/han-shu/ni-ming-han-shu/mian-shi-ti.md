# 面试题

## 什么是 lambda 函数？ 有什么好处？

lambda 函数是一个可以接收任意多个参数\(包括可选参数\)并且返回单个表达式值的函数 

1.  lambda 函数比较轻便，即用即仍，很适合需要完成一项功能，但是此功能只在此一处使用， 连名字都很随意的情况下
2. 匿名函数，一般用来给 filter， map 这样的函数式编程服务
3. 作为回调函数，传递给某些应用，比如消息处理

## 下面这段代码的输出结果将是什么？请解释

```python
def multipliers():
    return [lambda x : i * x for i in range(4)]
print [m(2) for m in multipliers()]
```

上面代码输出的结果是\[6， 6， 6， 6\]，而不是我们想的\[0， 2， 4， 6\]

上述问题产生的原因是 **Python 闭包的延迟绑定**。这意味着内部函数被调用时，参数的值在闭包内进行查找。因此当任何由 multipliers\(\)返回的函数被调用时，i 的值将在附近的范围进行查找。**那时不管返回的函数是否被调用，for 循环已经完成**，i 被赋予了最终的值 3。因此，每次返回的函数乘以传递过来的值 3，因为上段代码传过来的值是 2，它们最终返回的都是 6。

## 你如何修改上面的 multipliers 的定义产生想要的结果？

一种解决方法就是用 Python 生成器

```python
def multipliers():
    for i in range(4): 
        yield lambda x : i * x
```

另外一个解决方案就是创造一个闭包，利用默认函数立即绑定

```python
def multipliers():
    return [lambda x, i=i : i * x for i in range(4)]
```

