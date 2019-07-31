# 模块和包

## 模块

每个Python文件都可以作为一个模块，模块的名字就是文件的名字。导入模块，Python解析器对模块位置的搜索顺序：

1. 当前目录
2. 如果不在当前目录，Python则搜索在shell变量PYTHONPATH下的每个目录。
3. 如果都找不到，Python会察看默认路径。UNIX下默认路径一般为/usr/local/lib/python/
4. 模块搜索路径存储在system模块的sys.path变量中。变量里包含当前目录，PYTHONPATH和由安装过程决定的默认目录。

```text
from modname  # modname.function_name()
from modname import name1[, name2[, ... nameN]]  # from 模块名 import 函数名1,函数名2...
from modname import * # 导入全部内容，这种声明不该被过多地使用，容易产生冲突
import modname as rename  # 重命名，为了方便用短名字替换模块名使用
```

## 包

* 包将有联系的模块组织在同一个文件夹下，并在该文件夹下创建名字为`__init__.py` 文件

`__init__.py` 控制着包的导入行为

* `__init__.py`为空：仅仅是把这个包导入，不会导入包中的模块
* `__init__.py`中定义一个`__all__`变量，它控制着 from 包名 import \*时导入的模块，`__all__ =  ['className', 'function_name']`
* `__init__.py`中编写内容，当导入时这些语句就会被执行

