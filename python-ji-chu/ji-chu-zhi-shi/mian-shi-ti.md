# 面试题

## Python 是强语言类型还是弱语言类型？

Python 是`强类型`的`动态脚本语言`。

`强类型`：不允许不同类型相加。

 `动态`：不使用显示数据类型声明，确定一个变量类型是在第一次赋值时。 

`脚本语言`：解释型语言，运行代码只需要解释器，不需要编译。

## 关于 Python 程序的运行方面，有什么手段能提升性能？

1. 使用多进程，充分利用机器的多核性能 
2. 对于性能影响较大的部分代码，可以使用 C 或 C++编写 
3. 对于 IO 阻塞造成的性能影响，可以使用 IO 多路复用来解决
4. 尽量使用 Python 的内建函数
5. 尽量使用局部变量

## Python 中的作用域？

变量的作用域总是由在代码中被赋值的地方所决定，当 Python 遇到变量时按照如下顺序进行搜索： 

本地作用域\(Local\)---&gt;当前作用域被嵌入的本地作用域\(Enclosing locals\)---&gt;全局/模块作用域 \(Global\)---&gt;内置作用域\(Built-in\)

## 代码中要修改不可变数据会出现什么问题? 抛出什么异常?

> 代码不会正常运行，抛出 TypeError 异常

## a=1,b=2,不用中间变量交换 a 和 b 的值？

*  方法一

```python
a = a + b
b = a - b
a = a - b
```

*  方法二

```python
a = a ^ b
b = b ^ a
a = a ^ b
```

* 方法三

```python
a, b = b, a
```

## print 调用 Python 中底层的什么方法?

> print 方法默认调用 sys.stdout.write 方法，即往控制台打印字符串

## 下面这段代码的输出结果将是什么？请解释？

```python
class Parent(object):
    x = 1
class Child1(Parent):
    pass
class Child2(Parent):
    pass
print Parent.x, Child1.x, Child2.x
Child1.x = 2
print parent.x, Child1.x, Child2.x
parent.x = 3
print Parent.x, Child1.x, Child2.
```

1.  继承自父类的类属性x，所以都一样，指向同一块内存地址
2. 更改Child1，Child1的x指向了新的内存地址
3. 更改Parent，Parent的x指向了新的内存地址

##  简述你对 input\(\)函数的理解？

> 在 Python3 中，input\(\)获取用户输入，不论用户输入的是什么，获取到的都是字符串类型的。
>
>  在 Python2 中有 raw\_input\(\)和 input\(\), raw\_input\(\)和 Python3 中的 input\(\)作用是一样的， input\(\)输入的是什么数据类型的，获取到的就是什么数据类型的。

