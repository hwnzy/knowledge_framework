# 数组与矩阵

## 3. 数组中重复的数字 <a id="_3-&#x6570;&#x7EC4;&#x4E2D;&#x91CD;&#x590D;&#x7684;&#x6570;&#x5B57;"></a>

### 题目描述 <a id="&#x9898;&#x76EE;&#x63CF;&#x8FF0;"></a>

在一个长度为 n 的数组里的所有数字都在 0 到 n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字是重复的，也不知道每个数字重复几次。请找出数组中任意一个重复的数字。

```text
Input:
{2, 3, 1, 0, 2, 5}

Output:
2
```

### 解题思路 <a id="&#x89E3;&#x9898;&#x601D;&#x8DEF;"></a>

要求时间复杂度 O\(N\)，空间复杂度 O\(1\)。因此不能使用排序的方法，也不能使用额外的标记数组。

对于这种数组元素在 \[0, n-1\] 范围内的问题，可以将值为 i 的元素调整到第 i 个位置上进行求解。在调整过程中，如果第 i 位置上已经有一个值为 i 的元素，就可以知道 i 值重复。

```python
class Solution(object):
    def findRepeatNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        for idx, val in enumerate(nums):
            while nums[idx] != idx:
                if nums[val] != nums[idx]:
                    nums[idx], nums[val] = nums[val], nums[idx]
                    idx = val
                else:
                    return val
        return 0
```

##  4. 二维数组中的查找 <a id="_4-&#x4E8C;&#x7EF4;&#x6570;&#x7EC4;&#x4E2D;&#x7684;&#x67E5;&#x627E;"></a>



### 题目描述 <a id="&#x9898;&#x76EE;&#x63CF;&#x8FF0;"></a>

给定一个二维数组，其每一行从左到右递增排序，从上到下也是递增排序。给定一个数，判断这个数是否在该二维数组中。

```text
Consider the following matrix:
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]

Given target = 5, return true.
Given target = 20, return false.
```

### [\#](http://www.cyc2018.xyz/%E7%AE%97%E6%B3%95/%E5%89%91%E6%8C%87%20Offer%20%E9%A2%98%E8%A7%A3/4.%20%E4%BA%8C%E7%BB%B4%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E6%9F%A5%E6%89%BE.html#%E8%A7%A3%E9%A2%98%E6%80%9D%E8%B7%AF)解题思路 <a id="&#x89E3;&#x9898;&#x601D;&#x8DEF;"></a>

要求时间复杂度 O\(M + N\)，空间复杂度 O\(1\)。其中 M 为行数，N 为 列数。

该二维数组中的一个数，小于它的数一定在其左边，大于它的数一定在其下边。因此，从右上角开始查找，就可以根据 target 和当前元素的大小关系来快速地缩小查找区间，每次减少一行或者一列的元素。当前元素的查找区间为左下角的所有元素。

```python
class Solution(object):
    def findNumberIn2DArray(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if not matrix:
            return False
        m, n = len(matrix), len(matrix[0])
        i, j = 0, n-1
        while 0 <= i < m and 0 <= j < n:
            if target > matrix[i][j]:
                i += 1
            elif target < matrix[i][j]:
                j -= 1
            else:
                return True

        return False
```

## 29. 顺时针打印矩阵 <a id="_29-&#x987A;&#x65F6;&#x9488;&#x6253;&#x5370;&#x77E9;&#x9635;"></a>

### 题目描述 <a id="&#x9898;&#x76EE;&#x63CF;&#x8FF0;"></a>

按顺时针的方向，从外到里打印矩阵的值。

```python
matrix = [[1,2,3],[4,5,6],[7,8,9]]
```

矩阵打印结果为：

```python
[1,2,3,6,9,8,7,4,5]
```

解题思路

一层一层从外到里打印，观察可知每一层打印都有相同的处理步骤，唯一不同的是上下左右的边界不同了。因此使用四个变量 r1, r2, c1, c2 分别存储上下左右边界值，从而定义当前最外层。打印当前最外层的顺序：从左到右打印最上一行-&gt;从上到下打印最右一行-&gt;从右到左打印最下一行-&gt;从下到上打印最左一行。应当注意只有在 r1 != r2 时才打印最下一行，也就是在当前最外层的行数大于 1 时才打印最下一行，这是因为当前最外层只有一行时，继续打印最下一行，会导致重复打印。打印最左一行也要做同样处理。

```python
class Solution(object):
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        if not len(matrix):
            return []
        m, n = len(matrix), len(matrix[0])
        r1, r2, c1, c2 = 0, m-1, 0, n-1
        ret = []
        while r1 <= r2 and c1 <= c2:
            # 上
            for i in range(c1, c2+1):
                ret.append(matrix[r1][i])
            # 右
            for i in range(r1+1, r2+1):
                ret.append(matrix[i][c2])
            # 下
            if r1 != r2:
                for i in range(c2-1, c1-1, -1):
                    ret.append(matrix[r2][i])
            # 左
            if c1 != c2:
                for i in range(r2-1, r1, -1):
                    ret.append(matrix[i][c1])

            r1 += 1
            r2 -= 1
            c1 += 1
            c2 -= 1
        
        return ret
```

## 第一个只出现一次的字符位置 <a id="_50-&#x7B2C;&#x4E00;&#x4E2A;&#x53EA;&#x51FA;&#x73B0;&#x4E00;&#x6B21;&#x7684;&#x5B57;&#x7B26;&#x4F4D;&#x7F6E;"></a>

### 题目描述 <a id="&#x9898;&#x76EE;&#x63CF;&#x8FF0;"></a>

在一个字符串中找到第一个只出现一次的字符，并返回它的位置。字符串只包含 ASCII 码字符。

```text
Input: abacc
Output: b
```

### 解题思路 <a id="&#x89E3;&#x9898;&#x601D;&#x8DEF;"></a>

最直观的解法是使用 HashMap 对出现次数进行统计：字符做为 key，出现次数作为 value，遍历字符串每次都将 key 对应的 value 加 1。最后再遍历这个 HashMap 就可以找出出现次数为 1 的字符。

```python
class Solution(object):
    def firstUniqChar(self, s):
        """
        :type s: str
        :rtype: str
        """
        from collections import defaultdict
        cnts = defaultdict(int)
        for c in s:
            cnts[c] += 1
        for c in s:
            if cnts[c] == 1:
                return c
        
        return " "
```

