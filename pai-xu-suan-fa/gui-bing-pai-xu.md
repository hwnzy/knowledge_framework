# 归并排序

```python
def merge_sort(lst):
    def merge(left, right):
        result = []
        while left and right:
            result.append((left if left[0] <= right[0] else right).pop(0))
        return result + left + right
    if len(lst) <= 1:
        return lst
    mid = len(lst) // 2
    return merge(merge_sort(lst[:mid]), merge_sort(lst[mid:]))
```

