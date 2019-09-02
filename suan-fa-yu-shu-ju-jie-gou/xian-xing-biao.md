---
description: >-
  一种线性结构，在一种非空的线性表中，存在着唯一的一个首元素和唯一的一个尾元素，除首元素外，表中每个元素都有且仅有一个前驱元素，除尾元素外，每个元素都有且仅有一个后继元素。
---

# List

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



