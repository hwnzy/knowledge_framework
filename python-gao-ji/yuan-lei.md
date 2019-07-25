# 元类

## Python 中类方法、类实例方法、静态方法有何区别？

类方法：是类对象的方法，在定义时需要在上方使用“@classmethod”进行装饰，形参为 cls， 表示类对象。类对象和实例对象都可调用。

类实例方法：是类实例化对象的方法，只有实例对象可以调用，形参为 self，指代对象本身。

静态方法：是一个任意函数，在其上方使用“@staticmethod”进行装饰，可以用对象直接调用， 静态方法实际上跟该类没有太大关系。

## Python 中如何动态获取和设置对象的属性？

```python
if hasattr(Parent, 'x'):
    print(getattr(Parent, 'x'))
    setattr(Parent, 'x', 3)
print(getattr(Parent, 'x'))
```



