# 类属性和实例属性

### 类属性

```python
class People(object):
    name = 'Tom'  # 公有的类属性
    __age = 12  # 私有的类属性
People.name  #类属性
```

### 实例属性\(对象属性\)

```python
class People(object):
    name = 'Tom'  # 公有的类属性
    __age = 12  # 私有的类属性

p = People()
p.name  # 实例属性
```

### 通过实例\(对象\)去修改类属性

类外修改`类属性`，必须通过`类对象`引用后进行修改。

