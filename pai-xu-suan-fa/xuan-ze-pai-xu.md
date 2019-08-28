# 选择排序

```python
def selection_sort(lst):
    _len = len(lst)
    for i in range(_len - 1):
        k = i
        for j in range(i + 1, _len):
            if lst[j] < lst[k]:
                k = j
        lst[k], lst[i] = lst[i], lst[k]
    return lst
```

