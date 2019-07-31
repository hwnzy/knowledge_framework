# 相关操作

#### 1. 文件重命名 <a id="1-&#x6587;&#x4EF6;&#x91CD;&#x547D;&#x540D;"></a>

os模块中的rename\(\)可以完成对文件的重命名操作

rename\(需要修改的文件名, 新的文件名\)

```text
import os
os.rename("1.txt", "1-1.txt")
```

#### 2. 删除文件 <a id="2-&#x5220;&#x9664;&#x6587;&#x4EF6;"></a>

os模块中的remove\(\)可以完成对文件的删除操作

remove\(待删除的文件名\)

```text
import os
os.remove("1.txt")
```

#### 3. 创建文件夹 <a id="3-&#x521B;&#x5EFA;&#x6587;&#x4EF6;&#x5939;"></a>

```text
import os
os.mkdir("hwnzy")
```

#### 4. 获取当前目录 <a id="4-&#x83B7;&#x53D6;&#x5F53;&#x524D;&#x76EE;&#x5F55;"></a>

```text
import os
os.getcwd()
```

#### 5. 改变默认目录 <a id="5-&#x6539;&#x53D8;&#x9ED8;&#x8BA4;&#x76EE;&#x5F55;"></a>

```text
import os
os.chdir("../")
```

#### 6. 获取目录列表 <a id="6-&#x83B7;&#x53D6;&#x76EE;&#x5F55;&#x5217;&#x8868;"></a>

```text
import os
os.listdir("./")
```

#### 7. 删除文件夹 <a id="7-&#x5220;&#x9664;&#x6587;&#x4EF6;&#x5939;"></a>

```text
import os
os.rmdir("hwnzy")
```

