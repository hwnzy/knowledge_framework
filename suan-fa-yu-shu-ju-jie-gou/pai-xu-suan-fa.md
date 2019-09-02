# Algorithms

## 插入排序

```python
def insertion_sort(lst):
    for i in range(1, len(lst)):
        x = lst[i]
        j = i
        while j > 0 and lst[j-1] > x:
            lst[j] = lst[j-1]
            j -= 1
        lst[j] = x
    return lst
```

## shell排序

```python
def shell_sort(lst):
    gaps = [701, 301, 132, 57, 23, 10, 4, 1]
    for gap in gaps:
        i = gap
        while i < len(lst):
            temp = lst[i]
            j = i
            while j >= gap and lst[j - gap] > temp:
                lst[j] = lst[j - gap]
                j -= gap
            lst[j] = temp
            i += 1
    return collection
```

## 选择排序

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

## 交换排序

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

## 快速排序

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

## 归并排序

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

## Tim排序

```python
def binary_search(lst, item, start, end):
    if start == end:
        return start if lst[start] > item else start + 1
    if start > end:
        return start

    mid = (start + end) // 2
    if lst[mid] < item:
        return binary_search(lst, item, mid + 1, end)
    elif lst[mid] > item:
        return binary_search(lst, item, start, mid - 1)
    else:a
        return mid


def insertion_sort(lst):
    length = len(lst)

    for index in range(1, length):
        value = lst[index]
        pos = binary_search(lst, value, 0, index - 1)
        lst = lst[:pos] + [value] + lst[pos:index] + lst[index + 1 :]

    return lst


def merge(left, right):
    if not left:
        return right

    if not right:
        return left

    if left[0] < right[0]:
        return [left[0]] + merge(left[1:], right)

    return [right[0]] + merge(left, right[1:])


def tim_sort(lst):
    length = len(lst)
    runs, sorted_runs = [], []
    new_run = [lst[0]]
    sorted_array = []
    i = 1
    while i < length:
        if lst[i] < lst[i - 1]:
            runs.append(new_run)
            new_run = [lst[i]]
        else:
            new_run.append(lst[i])
        i += 1
    runs.append(new_run)

    for run in runs:
        sorted_runs.append(insertion_sort(run))
    for run in sorted_runs:
        sorted_array = merge(sorted_array, run)

    return sorted_array
```

