# 快速排序

```python
def quick_sort(lst):
    def qsort(lst, begin, end):
        if begin >= end:
            return
        pivot = lst[begin]
        i = begin
        for j in range(begin+1, end+1):
            if lst[j] < pivot:
                i += 1
                lst[i], lst[j] = lst[j], lst[i]
        lst[begin], lst[i] = lst[i], lst[begin]
        qsort(lst, begin, i-1)
        qsort(lst, i+1， end)
    qsort(lst, 0, len(lst)-1)
```

