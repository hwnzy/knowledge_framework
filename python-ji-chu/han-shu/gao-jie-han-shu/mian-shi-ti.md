# 面试题

## map 函数和 reduce 函数？

* 从参数方面来讲

> map\(\)包含两个参数，第一个参数是一个函数，第二个是序列（列表 或元组）。其中，函数（即 map 的第一个参数位置的函数）可以接收一个或多个参数。

> reduce\(\)第一个参数是函数，第二个是序列（列表或元组）。但是，其函数必须接收两个参数。

* 从对传进去的数值作用来讲

> map\(\)是将传入的函数依次作用到序列的每个元素，每个元素都是独自被函数“作用”一次 。

> reduce\(\)是将传人的函数作用在序列的第一个元素得到结果后，把这个结果继续与下一个元素作用 （累积计算）

## 递归函数停止的条件？

> 递归的终止条件一般定义在递归函数内部，在递归调用前要做一个条件判断，根据判断的结果选择 是继续调用自身，还是 return终止递归。

* 终止的条件：

1. 判断递归的次数是否达到某一限定值
2. 判断运算的结果是否达到某个范围等，根据设计的目的来选择

## 回调函数，如何通信的?

回调函数是把函数的地址作为参数传递给另一个函数，将整个函数当作一个对象赋值给调用的函数。

## Python 主要的内置数据类型都有哪些？ `print dir('a')` 的输出？

内建类型：布尔类型、数字、字符串、列表、元组、字典、集合

## 输出字符串'a'的内建方法

print\(list\(map\(lambda x: x \* x, \[y for y in range\(3\)\]\)\)\)的输出？

`[0， 1， 4]`

## hasattr\(\) getattr\(\) setattr\(\) 函数使用详解？

* hasattr\(object, name\)函数

> 判断一个对象里面是否有name属性或者name方法，返回bool值，有name属性\(方法\)返回True， 否则返回 False。

* getattr\(object, name\[,default\]\) 函数

> 获取对象 object 的属性或者方法，如果存在则打印出来，如果不存在，打印默认值，默认值可选。 注意：如果返回的是对象的方法，则打印结果是方法的内存地址，如果需要运行这个方法，可以在后面添加括号\(\)。

* setattr\(object,name,values\)函数

> 给对象的属性赋值，若属性不存在，先创建再赋值。

```python
class function_demo(object):
    name = 'demo'
    def run(self):
        return "hello function"
functiondemo = function_demo()
res = hasattr(functiondemo, 'addr') # 先判断是否存在 
if res:
    addr = getattr(functiondemo, 'addr')
    print(addr)
else:
    addr = getattr(functiondemo, 'addr', setattr(functiondemo, 'addr', '北京首都'))
    #addr = getattr(functiondemo, 'addr', '美国纽约')
    print(addr)
```

## 一句话解决阶乘函数？

`reduce(lambda x,y: x*y, range(1,n+1))  # python2中使用，python3中取消了该函数`

