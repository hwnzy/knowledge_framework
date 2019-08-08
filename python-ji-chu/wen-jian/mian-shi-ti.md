# 面试题

## 4G 内存怎么读取一个 5G 的数据？

方法一： 可以通过生成器，分多次读取，每次读取数量相对少的数据（比如 500MB）进行处理，处理结束后 在读取后面的 500MB 的数据。 

方法二： 可以通过 linux 命令 split 切割成小文件，然后再对数据进行处理，此方法效率比较高。可以按照行 数切割，可以按照文件大小切割。

## 现在考虑有一个 jsonline 格式的文件 file.txt 大小约为 10K，之前处理文件的 代码如下所示

```python
def get_lines():
    l = []
    with open(‘file.txt’，‘rb’ ) as f:
        for eachline in f:
            l.append(eachline)
    return l
if __name__ == ‘__main__’ :
    for e in get_lines():
        process(e) #处理每一行数据
```

现在要处理一个大小为 10G 的文件，但是内存只有 4G，如果在只修改 get\_lines 函数而其他代 码保持不变的情况下，应该如何实现？需要考虑的问题都有哪些？

```python
def get_lines():
    l = []
    with open(‘file.txt’，’ rb’ ) as f:
        data = f.readlines(60000)
    l.append(data)
    yield l
```

要考虑到的问题有： 内存只有 4G 无法一次性读入 10G 的文件，需要分批读入。分批读入数据要记录每次读入数据的位 置。分批每次读入数据的大小，太小就会在读取操作上花费过多时间。

## read、 readline 和 readlines 的区别?

read:读取整个文件

readline：读取下一行，使用生成器方法

readlines：读取整个文件到一个迭代器以供我们遍历

## 补充缺失的代码？

```python
def print_directory_contents(sPath):
    """
    这个函数接收文件夹的名称作为输入参数
    返回该文件夹中文件的路径
    以及其包含文件夹中文件的路径
    """
# 补充代码
```

```python
import os
for sChild in os.listdir(sPath):
    sChildPath = os.path.join(sPath, sChild)
    if os.path.isdir(sChildPath):
        print_directory_contents(sChildPath)
    else:
        print(sChildPath)
```

