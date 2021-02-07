# 栈队列堆

## 用两个栈实现队列 <a id="_9-&#x7528;&#x4E24;&#x4E2A;&#x6808;&#x5B9E;&#x73B0;&#x961F;&#x5217;"></a>

### 题目描述 <a id="&#x9898;&#x76EE;&#x63CF;&#x8FF0;"></a>

用两个栈来实现一个队列，完成队列的 Push 和 Pop 操作。

### 解题思路 <a id="&#x89E3;&#x9898;&#x601D;&#x8DEF;"></a>

in 栈用来处理入栈（push）操作，out 栈用来处理出栈（pop）操作。一个元素进入 in 栈之后，出栈的顺序被反转。当元素要出栈时，需要先进入 out 栈，此时元素出栈顺序再一次被反转，因此出栈顺序就和最开始入栈顺序是相同的，先进入的元素先退出，这就是队列的顺序。

```python
class CQueue(object):

    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    def appendTail(self, value):
        """
        :type value: int
        :rtype: None
        """
        self.stack1.append(value)


    def deleteHead(self):
        """
        :rtype: int
        """
        if not self.stack2 and self.stack1:
            self.stack2 = self.stack1[::-1]
            self.stack1 = []
        if self.stack2:
            return self.stack2.pop()
        return -1

```

## 包含 min 函数的栈 <a id="_30-&#x5305;&#x542B;-min-&#x51FD;&#x6570;&#x7684;&#x6808;"></a>

### 题目描述 <a id="&#x9898;&#x76EE;&#x63CF;&#x8FF0;"></a>

实现一个包含 min\(\) 函数的栈，该方法返回当前栈中最小的值。

### 解题思路 <a id="&#x89E3;&#x9898;&#x601D;&#x8DEF;"></a>

使用一个额外的 minStack，栈顶元素为当前栈中最小的值。在对栈进行 push 入栈和 pop 出栈操作时，同样需要对 minStack 进行入栈出栈操作，从而使 minStack 栈顶元素一直为当前栈中最小的值。在进行 push 操作时，需要比较入栈元素和当前栈中最小值，将值较小的元素 push 到 minStack 中。

```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.min_stack = []

    def push(self, x):
        """
        :type x: int
        :rtype: None
        """
        self.stack.append(x)
        if not self.min_stack:
            self.min_stack.append(x)
        else:
            min_val = self.min_stack[-1]
            if min_val >= x:
                self.min_stack.append(x)


    def pop(self):
        """
        :rtype: None
        """
        val = self.stack.pop()
        if self.min_stack and self.min_stack[-1] == val:
            self.min_stack.pop()
        return val


    def top(self):
        """
        :rtype: int
        """
        if self.stack:
            return self.stack[-1]


    def min(self):
        """
        :rtype: int
        """
        if self.min_stack:
            return self.min_stack[-1]

```

## 栈的压入、弹出序列 <a id="_31-&#x6808;&#x7684;&#x538B;&#x5165;&#x3001;&#x5F39;&#x51FA;&#x5E8F;&#x5217;"></a>

### 题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。

例如序列 1,2,3,4,5 是某栈的压入顺序，序列 4,5,3,2,1 是该压栈序列对应的一个弹出序列，但 4,3,5,1,2 就不可能是该压栈序列的弹出序列。

### 解题思路 <a id="&#x89E3;&#x9898;&#x601D;&#x8DEF;"></a>

使用一个栈来模拟压入弹出操作。每次入栈一个元素后，都要判断一下栈顶元素是不是当前出栈序列 popSequence 的第一个元素，如果是的话则执行出栈操作并将 popSequence 往后移一位，继续进行判断。

```python
class Solution(object):
    def validateStackSequences(self, pushed, popped):
        """
        :type pushed: List[int]
        :type popped: List[int]
        :rtype: bool
        """
        pos = 0
        n = len(popped)
        l = []
        for ele in pushed:
            l.append(ele)
            while l and l[-1] == popped[pos]:
                l.pop()
                pos += 1
        while l and l[-1] == popped[pos]:
            pos += 1
            l.pop()
        if n == pos:
            return True
        return False
```

## 最小的 K 个数 <a id="_40-&#x6700;&#x5C0F;&#x7684;-k-&#x4E2A;&#x6570;"></a>

### 解题思路 <a id="&#x89E3;&#x9898;&#x601D;&#x8DEF;"></a>

#### 大小为 K 的最小堆 <a id="&#x5927;&#x5C0F;&#x4E3A;-k-&#x7684;&#x6700;&#x5C0F;&#x5806;"></a>

* 复杂度：O\(NlogK\) + O\(K\)
* 特别适合处理海量数据

维护一个大小为 K 的最小堆过程如下：使用大顶堆。在添加一个元素之后，如果大顶堆的大小大于 K，那么将大顶堆的堆顶元素去除，也就是将当前堆中值最大的元素去除，从而使得留在堆中的元素都比被去除的元素来得小。

应该使用大顶堆来维护最小堆，而不能直接创建一个小顶堆并设置一个大小，企图让小顶堆中的元素都是最小元素。

```python
class Solution(object):
    def getLeastNumbers(self, arr, k):
        """
        :type arr: List[int]
        :type k: int
        :rtype: List[int]
        """
        import heapq
        res = []
        if not k:
            return res
        for i in arr:
            if len(res) < k:
                heapq.heappush(res, -i)
            elif res[0] < -i:
                heapq.heappop(res)
                heapq.heappush(res, -i)
        ans = []
        while res:
            ans.append(-heappop(res))
        return ans[::-1]


```

