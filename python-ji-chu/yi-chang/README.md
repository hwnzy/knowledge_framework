# 异常

## 异常简介

当Python检测错误，解释器无法继续执行，出现错误提示，这就是异常

```text
try:
    open('1.txt','r') # 把可能出现问题的代码，放在try中
except Exception as result:  # 捕获所有异常，并且存储异常的基本信息
    print(result) # 把处理异常的代码，放在except中   
else:  # 即如果没有捕获到异常，那么就执行else中的事情
    print('0 error')
finally:  # 即无论异常是否产生都要执行
    f.close()  # 关闭文件
    
try:  # 捕获多个异常的方式
    open('1.txt','r') # 把可能出现问题的代码，放在try中
except (IOError,NameError): 
    pass  # 如果想通过一次except捕获到多个异常可以用一个元组的方式
```

## 异常的传递

* try嵌套，如果里面的try没有捕获到这个异常，外面的try会接收到这个异常进行处理，如果没有捕获到再进行传递。

## 抛出自定义的异常

用raise语句引发一个异常。异常/错误对象必须有名字，它们应是Error或Exception类的子类。

```python
class ShortInputException(Exception):
    '''自定义的异常类'''
    def __init__(self, length, atleast):
        self.length = length
        self.atleast = atleast

def main():
    try:
        s = input()
        if len(s) < 3:
            raise ShortInputException(len(s), 3) # raise 引发一个你定义的异常
    except ShortInputException as result:#x这个变量被绑定到了错误的实例
        print('ShortInputException: 输入的长度是 %d,长度至少应是 %d'% (result.length, result.atleast))
    else:
        print('没有异常发生.')
main()
```



