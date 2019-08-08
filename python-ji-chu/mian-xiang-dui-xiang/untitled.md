# 继承

### 继承的意义

* 继承是多个类之间的所属关系。
* 类A的属性和方法可复用，可通过继承传递到类B。
* 类A就是基类，也叫做父类，类B就是派生类，也叫做子类。

### 单继承：子类只继承一个父类 <a id="&#x5355;&#x7EE7;&#x627F;&#xFF1A;&#x5B50;&#x7C7B;&#x53EA;&#x7EE7;&#x627F;&#x4E00;&#x4E2A;&#x7236;&#x7C7B;"></a>

```python
# 父类
class A(object):
    def __init(self):
        self.num = 1
    def show_num(self):
        print(self.num)
# 子类
class B(A):
    pass
b = B()  # 创建子类实例对象
b.num  # 子类对象可以直接使用父类属性
b.show_num()  # 子类对象可以直接使用父类的方法
```

* 虽然子类没有定义`__init__`方法初始化属性，也没有定义实例方法，但是只要创建子类的对象，就默认执行了继承过来的`__init__`方法

### 多继承：子类继承多个父类

* 多继承继承了所有父类的属性和方法
* 如果多个父类中有同名的属性和方法，默认使用第一个父类的属性和方法（根据类的魔法属性`__mro__`的顺序来查找）

```python
# 父类
class A(object):
    pass
# 父类
class B(object):
    pass
# 子类
class C(A, B):
    pass
```

### 子类重写父类的同名属性和方法

* 子类和父类有同名属性或方法，默认使用子类的

### 子类调用父类同名属性和方法

```python
父类名.父类方法(self)
父类名.父类属性
```

### 多层继承

```python
# 父类
class A(object):
    pass
# 父类
class B(A):
    pass
# 子类
class C(B):
    pass
```

### super\(\)的使用 <a id="super&#x7684;&#x4F7F;&#x7528;"></a>

super\(\) 可逐一调用所有父类方法，且只执行一次。调用顺序遵循 `__mro__` 类属性顺序。



