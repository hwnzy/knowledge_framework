# 静态方法和类方法

### 类方法 <a id="1-&#x7C7B;&#x65B9;&#x6CD5;"></a>

需用修饰器`@classmethod`标识其为类方法。类方法第一个参数必须是类对象，一般以`cls`作为第一个参数。用来获取或修改类属性。

```python
class People(object):
    country = 'china'

    @classmethod  # 类方法，用classmethod来进行修饰
    def get_country(cls):
        return cls.country
    @classmethod
    def set_country(cls,country):
        cls.country = country

p = People()
print(p.get_country())    # 可以用过实例对象引用
p.set_country('japan') 
print(People.get_country())    # 可以通过类对象引用
```

结果显示在用类方法对类属性修改之后，**通过类对象和实例对象访问都发生了改变**

### 静态方法 <a id="2-&#x9759;&#x6001;&#x65B9;&#x6CD5;"></a>

需要通过修饰器`@staticmethod`来进行修饰，静态方法不需要多定义参数，可以通过对象和类来访问。

```python
class People(object):
    country = 'china'

    @staticmethod 
    def get_country():  # 静态方法
        return People.country

p = People()
p.get_contry() # 通过对象访问静态方法
print(People.get_country()) # 通过类访问静态方法
```

###  <a id="&#x603B;&#x7ED3;"></a>

