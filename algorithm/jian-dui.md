# 建堆

## 建立最小堆

```python
#!/usr/bin/env python

import os
import sys
import math

def heap(list):
    n = len(list)
    for i in range(0,int(math.log(n,2))): # 每循环依次就完成了一层的建堆
        for j in range(0,n//2):
            k = 2*j+2 if 2*j+2 < n and list[2*j+2] < list[2*j+1] else 2*j+1 # 让k成为较小的子节点的index
            if list[j] > list[k]:
                (list[j],list[k]) = (list[k],list[j]) # 交换值

def main(argv):
    list = [int(arg) for arg in argv]
    heap(list)
    print(list)
if __name__ == "__main__":
    if len(sys.argv) > 1:
        main(sys.argv[1:])
```

这个算法的复杂度为O\(nlogn\), 但实际上，最优的建堆算法的复杂度是O\(n\)

