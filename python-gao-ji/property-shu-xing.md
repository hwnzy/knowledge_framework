# property属性

### 装饰器方式

```python
class Person(object):
    def __init__(self):
        self.__age = 0

    @property  # 读取age
    def age(self):
        return self.__age

    @age.setter  # 设置age
    def age(self, new_age):
        if new_age >= 120:
            print("too old")
        else:
            self.__age = new_age

p = Person()  # 0
p.age = 100  # 100
p.age = 1000  # too_old
```

### 类属性方式

```python
class Person(object):
    def __init__(self):
        self.__age = 0

    def get_age(self):
        return self.__age

    def set_age(self, new_age):
        if new_age >= 120:
            print("too old")
        else:
            self.__age = new_age

    age = property(get_age, set_age)  #(获取属性, 设置属性)

p = Person()
p.age = 100
p.age = 1000
```



