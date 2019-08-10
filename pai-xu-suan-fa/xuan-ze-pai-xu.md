# 选择排序

```python
def selection_sort(lst):
    length = len(lst)
    for i in range(length - 1):
        least = i
        for k in range(i + 1, length):
            if lst[k] < lst[least]:
                least = k
        lst[least], lst[i] = lst[i], lst[least]
    return collection
```

