# 字典

```python
info = {'name':'hwnzy', 'id':1, 'sex':'man', 'address':'北京'}
info['name']  # 根据键访问值，访问不存在的键抛出KeyError
info.get('age')  # 不确定是否存在某个键又想获取其值，使用get方法
info.get('age', 24)  # 设置默认值，若info中不存在'age'这个键，就返回默认值24
```

### 修改和删除

 **变量名\[键\] = 数据**   \# 键在字典中则修改元素，不存在则新增元素

对字典进行删除操作，有一下几种：

* del dict\[key\]  \# del 删除指定的元素，也可以del dict 删除整个字典
* dict.clear\(\)  \# 清空整个字典

常见操作

```python
dict = {"name": 'hwnzy', 'sex': 'man'}
len(dict)  # 2 测量字典中，键值对的个数
dict.keys()  # 测量字典中，键值对的个数 ['name', 'sex']
dict.values()  # 返回包含字典所有KEY的列表 ['hwnzy', 'man']
dict.items()  # 返回包含所有（键，值）元祖的列表 [('name', 'hwnzy'), ('sex', 'man')]
```





