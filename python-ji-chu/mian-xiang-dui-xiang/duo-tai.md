# 多态

在需要使用父类对象的地方也可以使用子类对象，这种情况就叫多态.

#### 如何在程序中使用多态? <a id="&#x5982;&#x4F55;&#x5728;&#x7A0B;&#x5E8F;&#x4E2D;&#x4F7F;&#x7528;&#x591A;&#x6001;"></a>

```python
'''
可以按照以下几个步骤来写代码:
    1.子类继承父类
    2.子类重写父类中的方法
    3.通过对象调用这个方法
'''
class Father:
    def cure(self):
        print("父亲给病人治病...")

class Son(Father):
    # 重写父类中的方法
    def cure(self):
        print("儿子给病人治病...")

def call_cure(doctor):
    doctor.cure()

father = Father()
call_cure(father)
son = Son()
call_cure(son)
```



### 使用多态的好处

> 给call\_cure\(doctor\)函数传递哪个对象，在它里面就会调用哪个对象的cure\(\)方法，让call\_cure\(doctor\)函数变得更加灵活，额外增加了它的功能，提高了它的扩展性。

