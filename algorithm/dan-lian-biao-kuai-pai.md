# 单链表快排

```python
// 链表结点类
class Node():  
    def __init__(self, item=None):
        self.item = item  // 数据域
        self.next = None  // 指针域


// 链表类，生成链表以及定义相关方法
class LinkList():
    def __init__(self):
        self.head = None
        
    // 生成链表，这里使用list来生成
    def create(self, item):
        self.head = Node(item[0])
        p = self.head
        for i in item[1:]:
            p.next = Node(i)
            p = p.next

    // 遍历显示
    def print(self):
        p = self.head
        while p != None:
            print(p.item, end=' ')
            p = p.next
        print()

    // 根据索引取值
    def getItem(self, index):
        p = self.head
        count = 0
        while count != index:
            p = p.next
            count += 1
        return p.item

    // 根据索引设值
    def setItem(self, index, item):
        p = self.head
        count = -1
        while count < index-1:
            p = p.next
            count += 1
        p.item = item

    // 互换
    def swapItem(self, i, j):
        t = self.getItem(j)
        self.setItem(j, self.getItem(i))
        self.setItem(i, t)

    def quicksortofloop(self, left, right):
        if left < right:
    		// 初始化
            i = left
            j = i+1
            start = self.getItem(i)

        	// 大循环条件，j不能超过链表长度
            while (j <= right):
        		// 如果 j 指向的值大于等于基准数字，直接跳过
                while (j <= right and self.getItem(j) >= start):
                    j += 1
        	    // 否则，j 指向的值小于基准，则交换
                if (j <= right):
                    i += 1
                    self.swapItem(i, j)
                    self.print()
                    j += 1
            self.swapItem(left, i)
            self.quicksortofloop(left, i-1)
            self.quicksortofloop(i+1, right)


if __name__ == "__main__":
    L = LinkList()
    L.create([4, 2, 5, 3, 7, 9, 0, 1])
    L.quicksortofloop(0, 7)
    L.print()
```

