# 交换排序

```python
def bubble_sort(lst):
    _len = len(lst)
    for i in range(_len-1):
        swapped = False
        for j in range(_len-1-i):
            if lst[j] > lst[j+1]:
                swapped = True
                lst[j], lst[j+1] = lst[j+1], lst[j]
        if not swapped: 
            break
    return lst
```

