# Data\_Structures\_and\_Algorithms

 算法设计模式

*  枚举法。根据具体问题枚举出各种可能，从中选出有用信息或者问题的解。利用计算机的计算速度优势，在解决简单问题时有效。
*  贪心法。根据问题的信息尽可能做出部分的解，并基于部分解逐步扩充得到完整的解。在解决复杂问题时，这种做法未必能得到最好的解。
*  分治法。把复杂的问题分解为相对简单的子问题，分别求解，最后通过组合起子问题的解的方式得到原问题的解。
*  回溯法（搜索法）。通过搜索的方式求解。如果问题很复杂，没有清晰的求解路径就可能需要分步骤进行，而每一个步骤又可能有多个选择。这种情况下，只能采用试探的方式，根据实际情况选择一个可能的方向。在后面的求解步骤无法继续时，就需要退回到前面的步骤，另行选择求解路径，这种动作称为回溯。
* 动态规划法。在复杂情况下，问题求解很难直截了当进行，因此需要在前面的步骤中积累信息，在后续步骤中根据已知信息，动态选择已知的最好求解路径（同时可能进一步积累信息）这种算法模式被称为动态规划。
* 分支界限法。可以看作搜索方法的一种改良形式。如果在搜索过程中可以得到一些信息，确定某些可能的选择实际上并不真正有用，就可以及早将其删除以缩小求解空间，加速问题求解过程。

 算法时间复杂度和空间复杂

| 时间复杂度 | 复杂度函数 |
| :--- | :--- |
| 常量复杂度 | O\(1\) |
| 对数复杂度 | O\($$\log^n$$\) |
| 线性复杂度 | O\(n\) |
| 平方复杂度 | O\($$n^2$$\) |
| 指数复杂度 | O\($$2^n$$\) |



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

## 字符串匹配

### 朴素的串匹配算法

1. 从左到右逐个字符匹配
2. 发现不匹配时转去考虑目标串里的下一个位置是否与模式串匹配

![&#x6734;&#x7D20;&#x7684;&#x5B57;&#x7B26;&#x4E32;&#x5339;&#x914D;](.gitbook/assets/image%20%2820%29.png)

```python
def naive_matching(t, p):
    m, n = len(p), len(t)
    i, j = 0, 0
    while i < m and j < n:  # i == m 说明找到匹配
        if p[i] == t[j]:  # 字符串相同：考虑下一对字符串
            i, j = i + 1, j + 1
        else:  # 字符不同：考虑t中下一个位置
            i, j = 0, j - i + 1
    if i == m:  # 找到匹配， 返回其开始下标
        return j - i
    return -1  # 无匹配，返回特殊值
```

### 无回溯串匹配算法（KMP算法）

![&#x6734;&#x7D20;&#x5339;&#x914D;&#x548C;KMP&#x5339;&#x914D;&#x8FC7;&#x7A0B;](.gitbook/assets/image%20%2812%29.png)

| 字串 | a | g | c | t | a | g | c | a | g | c | t | a | g | c | t | g |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| pnext | -1 | 0 | 0 | 0 | 0 | 1 | 2 | 3 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 4 |

```python
def generate_pnext(p):
    i, k, m = 0, -1, len(p)
    pnext = [-1] * m
    while i < m - 1:  # 生成下一个pnext元素值
        if k == -1 or p[i] == p[k]:
             i, k = i + 1, k + 1
             if p[i] == p[k]:
                 pnext[i] = pnext[k]
             else:
                 pnext[i] = k  # 设置pnext元素
         else:
             k = pnext[k]  # 退到更短相同前缀
     return pnext
```

```python
def matching_KMP(t, p, pnext):
    n, m = len(t), len(p)
    j, i = 0, 0
    while j < n and i < m:  # i == m 说明找到了匹配
        if i == -1 or t[j] == p[i]:  # 考虑p中下一个字符
            j, i = j + 1, i + 1
        else:  # 失败：考虑pnext决定下一个字符
            i = pnext[i]
    if i == m:  # 找到匹配，返回其下标
        return j - i
    return -1  # 无匹配，返回特殊值
```

## 栈

```python
class Stack:
    def __init__(self):
        self._items = []
    def is_empty(self):
        return not bool(self._items)
    def push(self, item):
        self._items.append(item)
    def pop(self):
        return self._items.pop()
    def peek(self):
        return self._items[-1]
    def size(self):
        return len(self._items)
```

## 队列

```python
class Queue:
    def __init__(self):
        self._items = []
    def is_empty(self):
        return not bool(self._items)
    def enqueue(self, item):
        self._items.insert(0, item)
    def dequeue(self):
        return self._items.pop()
    def size(self):
        return len(self._items)
```

## 链表

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class Linked_List:
    def __init__(self):
        self.Head = None    
    def insert_tail(self, data):
        if(self.Head is None): 
            self.insert_head(data) 
        else:
            temp = self.Head
            while(temp.next != None):
                temp = temp.next
            temp.next = Node(data) 
    def insert_head(self, data):
        newNod = Node(data)
        if self.Head != None:
            newNod.next = self.Head
        self.Head = newNod
    def printList(self):
        tamp = self.Head
        while tamp is not None:
            print(tamp.data)
            tamp = tamp.next
    def delete_head(self):
        temp = self.Head
        if self.Head != None:
            self.Head = self.Head.next
            temp.next = None
        return temp
    def delete_tail(self):
        tamp = self.Head
        if self.Head != None:
            if(self.Head.next is None):
                self.Head = None
            else:
                while tamp.next.next is not None:
                    tamp = tamp.next
                tamp.next, tamp = None, tamp.next
        return tamp
    def isEmpty(self):
        return self.Head is None
    def reverse(self):
        prev = None
        current = self.Head
        while current:
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node
        self.Head = prev
```

## 二叉排序树

BST是指一棵空树或者具有以下性质的二叉树：

* 左子节点的值比父节点小
* 右子节点的值比父节点大
* 任意节点的左右字树也分别为二叉查找树
* 没有键值相等的点

![BST](.gitbook/assets/image%20%2888%29.png)

在理想情况下，二叉查找树增删改查的时间复杂度为O\(logN\)，但是若是二叉树极度不平衡，比如下图这样形成了一个线性链后，就会产生最坏运行情况O\(N\)

![&#x6700;&#x574F;&#x8FD0;&#x884C;&#x590D;&#x6742;&#x5EA6;](.gitbook/assets/image%20%2830%29.png)

## 红黑树

红黑树从本质上来说就是一颗二叉查找树，但是在二叉树的基础上增加了着色相关的性质，使得红黑树可以保证相对平衡，从而保证红黑树的增删改查的时间复杂度最坏也能达到O\(log N\)。

红黑树的应用非常广泛，常见的函数库，如C++中的map采用了红黑树。

* 节点是红色或黑色。 
* 根节点是黑色。 
* 每个红色节点的两个子节点都是黑色。\(从每个叶子到根的所有路径上不能有两个连续的红色节点\) 
* 从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点

![&#x7EA2;&#x9ED1;&#x6811;](.gitbook/assets/image%20%2851%29.png)

## B树

### m阶B树

1. 每个结点至多拥有m棵子树
2. 根结点至少拥有两颗子树（存在子树的情况下）
3. 除了根结点以外，其余每个分支结点至少拥有 m/2 棵子树
4. 所有的叶结点都在同一层上
5. 有 k 棵子树的分支结点则存在 k-1 个关键码，关键码按照递增次序进行排列
6. 关键字数量需要满足m/2-1（向上取整） &lt;= n &lt;= m-1

### 插入

![](.gitbook/assets/image%20%2870%29.png)

### 

![](.gitbook/assets/image%20%2856%29.png)

![](.gitbook/assets/image%20%2886%29.png)

### 删除

![](.gitbook/assets/image%20%2874%29.png)

![](.gitbook/assets/image%20%282%29.png)

![](.gitbook/assets/image%20%2882%29.png)

## B+树

### M阶B+树

1. 其定义基本与B-树同，除了
2. 非叶子结点的子树指针与关键字个数相同
3. 非叶子结点的子树指针P\[i\]，指向关键字值属于\[K\[i\], K\[i+1\]\)的子树（B-树是开区间）
4. 为所有叶子结点增加一个链指针
5. 所有关键字都在叶子结点出现

![](.gitbook/assets/image%20%2881%29.png)



## 深度优先遍历

1. 首先访问顶点V，并将其标记已访问
2. 查询V的邻接顶点，从中选一个尚未访问的顶点，从它出发继续进行深度优先搜索（递归）。不存在这种邻接顶点时回溯
3. 反复执行上述操作直到V出发可达的所有顶点都已访问（递归）
4. 如果图中存在未访问的顶点，则选出一个未访问的顶点，从它的出发重复前述过程，直到图中所有顶点都已访问为止

![&#x6DF1;&#x5EA6;&#x4F18;&#x5148;&#x904D;&#x5386;](.gitbook/assets/image%20%2833%29.png)

```python

```

