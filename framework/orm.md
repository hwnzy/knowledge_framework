# ORM

第一部分 - 数据类型Field类的定义

\# 定义Field类，包含两个属性，分别是字段的名称name与类型column\_type

```text
class Field(object):
    # 类Fiedl的构造函数有两个属性: name, column_type
    def __init__(self, name, column_type):
        self.name = name
        self.column_type = column_type
    def __str__(self):
        return '<%s:%s>' % (self.__class__.__name__, self.name)
```

\# 类StringField继承自类Field，同样有两个属性name与类型column\_type

```text
class StringField(Field):
    # 此处仅属性name是强制属性
    def __init__(self, name):
        # 通过super()函数调用parent类的构造函数
        # 其中name就直接传递给Field, column_type传递固定值'varchar(100)'
        # 所以StringField('username').__dict__ == Field('username', 'varchar(100)').__dict__
        super(StringField, self).__init__(name, 'varchar(100)')
```

\# 同理类IntegerField继承自类Field，同样有两个属性name与类型column\_type

```text
class IntegerField(Field):
    def __init__(self, name):
        super(IntegerField, self).__init__(name, 'bigint')
```

第二部分 - 元类ModelMetaclass的定义

```text
class ModelMetaclass(type):
    # 此处说明下__new__和__init__的区别：
    # __new__是用来创造一个类对象的构造函数，而__init__是用来初始化一个实例对象的构造函数
    # 类似于__init__，__new__接收的第一个参数cls（类对象）其实就是相当于__init__的self（实例对象）
    # 在初始化__init__之前，类是通过__new__创建的，所以在__init__前一定有__new__来构造类cls，之后__init__才能初始化对象self
    def __new__(cls, name, bases, attrs):
        # 对于名称为Model的类不做其他操作，直接通过type()函数生成类对象Model
        if name == 'Model':
            return type.__new__(cls, name, bases, attrs)
        print('Found model: %s' % name)
        # 对于名称不是Model的类，如User类，通过下面代码过滤类对象User的属性
        # 因为类属性以dict格式存在attrs中，所以是对dict格式的操作：
        # 第一步，过滤出满足条件的属性
        # 先新建一个空dict，如果对象的属性值是Field类的实例对象(如StringField('username'))，则将这些属性放入dict格式的mappings变量中
        mappings = dict()
        for k, v in attrs.items():
            # 下面判断属性的值是否是Field类格式，满足Field类格式形态如下：
            # isinstance(StringField('username'), Field)
            # 或者isinstance(Field('name', StringField('username')), Field)
            # 注意此处是属性的值，不是属性，如：属性name的值为StringField('username')
            if isinstance(v, Field):
                print('Found mapping: %s ==> %s' % (k, v))
                # 将过滤出的dict数据写入mappings变量
                mappings[k] = v
        # 其实上面的这几行可以简化为一行 mappings = {k:v for k, v in attrs.items() if isinstance(v, Field)} 吧？

        # 第二步，把上一步过滤出满足条件的属性从类对象User的属性dict中移除
        for k in mappings.keys():
            attrs.pop(k)

        # 第三步，把dict格式的mappings变量添加到类对象User的属性__mapping__中
        # 即__mapping__成为了类对象User的一个属性，该属性值为dict格式，内容为满足Field类格式的原类对象的属性值
        attrs['__mappings__'] = mappings
        # 到目前为止其实就是把实例对象User的一些属性（满足Field格式）移了个位置
        # 下面再新建一个属性__table__，并且赋值为该类对象的名字，如User
        attrs['__table__'] = name  # 假设表名和类名一致
        # 将修改后的类对象User的属性值返回给type().__new__
        return type.__new__(cls, name, bases, attrs)
```

其实定义好的元类ModelMetaclass，仍然是调用type.\_\_new\_\_\(cls, name, bases, attrs\)函数去构造类对象，可以试下如下代码

不同的是type.\_\_new\_\_\(\)构造出的类的属性就在\_\_dict\_\_下，但是ModelMetaclass将属性移到\_\_mappings\_\_下了

```text
type.__new__(type, 'Model', (dict, ), {'id': IntegerField('id')}).__dict__
```

```text
ModelMetaclass('Table1', (object, ), {'name': StringField('username')}).__mappings__
```

第三部分 - 生成Model的实例对象

核心在于：在运行\_\_init\_\_来生成实例对象前，调用元函数ModelMetaclass来生成类对象，用这个类对象再去生成实例对象。 根据ModelMetaclass代码可以知道，当类名称为Model时，直接返回原始的type.\_\_new\_\_\(cls, name, bases, attrs\)， 如：type.\_\_new\_\_\(type, 'Model', \(dict, \), Model\(id=12345, name='Michael'\)\)

此处对于super\(\)函数的理解还是不清楚

```text
class Model(dict, metaclass=ModelMetaclass):
    # 在运行__init__来生成实例对象前，调用元函数ModelMetaclass来生成类对象，用这个类对象再去生成实例对象
    # 根据ModelMetaclass代码可以知道，当类名称为Model时，直接返回原始的type.__new__(cls, name, bases, attrs)
    # 如：type.__new__(type, 'Model', (dict, ), Model(id=12345, name='Michael'))
    # 接下来定义从类对象生成实例对象的__init__函数
    def __init__(self, **kw):
        # 通过super调用父类初始化函数，__init__()动态函数无需self
        # 此处先调用dict
        # ModelMetaclass('User', (type, ), {'id': IntegerField('id')})
        super(Model, self).__init__(**kw)
    # 重新定义getattr函数，可以匹配dict格式输入的{属性:属性值}
    # 如：Model(id = IntegerField('id')).__getattr__
    def __getattr__(self, key):
        '''
        重载__getattr__, __setattr__方法使子类可以像正常的类使用
        '''
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Model' object has no attribute '%s'" % key)
    # 个人觉得可以不定义__setattr__，没什么影响
    def __setattr__(self, key, value):
        self[key] = value
    # 定义save方法，用于动态生成SQL
    def save(self):
        # 定义3个数组
        fields = []
        params = []
        args = []
        # 此处引入了之前ModelMetaclass元类定义的dict格式变量__mappings__
        # 假设__mappings__ = {'name': StringField('username')}，则'name'就是k, StringField('username')是v, 'username'是v.name
        for k, v in self.__mappings__.items():
            # 把__mappings__的v.name（即'username'）写入fields变量
            fields.append(v.name)
            params.append('?')
            # fields变量('username')对应的数值通过重新定义的__getattr__函数获取
            # __mappings__中的k为{'name': StringField('username')}中的'name'
            args.append(getattr(self, k, None))
        # __table__在之前ModelMetaclass元类定义为类对象的名称，即'User'表
        # 通过'sep'.join(seq)函数（','.join(['id', 'username'])）生成表字段和字段值
        sql = 'insert into %s (%s) values (%s)' % (self.__table__, ','.join(fields), ','.join(params))
        print('SQL: %s' % sql)
        print('ARGS: %s' % str(args))
```

第四部分 - 生成User实例对象

```text
class User(Model):
    # 因为User继承了Model，而Model继承了dict类型    # 所以dict(id=12345, name='Michael')直接转化为{'id': 12345, 'name': 'Michael'}格式    id = IntegerField('id')
    name = StringField('username')
    email = StringField('email')
    password = StringField('password')
    test = str('abc')
```

总结User实例对象的创建步骤：

1. User类的创建1: User类继承了Model类，而Model类由元类ModelMetaclass定义类的生成，所以User类对象首先通过ModelMetaclass的type.\_\_new\_\_\(\)构造生成

2. User类的创建2: 在ModelMetaclass的type.\_\_new\_\_\(\)构造函数中，name和bases已经确认，而属性attr是根据输入的属性内容，由\_\_new\_\_构造出

3. User实例的创建1：User类继承了Model类，所以User通过Model的构造函数\_\_init\_\_生成属性的时候会把输入的\(id=12345, name='Michael'\)转化为dict格式{'id': 12345, 'name': 'Michael'}

4. User实例的创建2: User实例的生成中要按照元类ModelMetaclass的定义生成，即生成\_\_mappings\_\_和\_\_Table\_\_等属性

