## JZ-如何构造

### 数据流中的中位数

思路1 ：排序

思路2：堆

```python
# -*- coding:utf-8 -*-

import math
class Solution:
    def __init__(self):
        self.data = []
        
    def Insert(self, num):
        # write code here
        i = 0
        while i < len(self.data):
            if num >= self.data[i]:
                break
            i += 1
        self.data.insert(i, num)
        
    def GetMedian(self, data):
        # write code here
        length = len(self.data)
        if length%2==0:
            return (self.data[length//2]+self.data[length//2-1])/2.0
        else:
            return self.data[int(length)//2]
        


```

思路2：堆

```python
# -*- coding:utf-8 -*-
from heapq import *
class Solution:
    def __init__(self):
        self.A = [] #小顶
        self.B = [] #大顶
    def Insert(self, num):
        # write code here
        if len(self.A) != len(self.B): # 如果两个堆中元素不等，则将新元素放到B中
            heappush(self.A, num)
            heappush(self.B, -heappop(self.A))
        else: # 如果两个堆里的元素数量相等，则将新元素放到A中
            heappush(self.B, -num)
            heappush(self.A, -heappop(self.B))
    def GetMedian(self,fuck):
        # write code here
        return self.A[0] if len(self.A) != len(self.B) else (self.A[0] - self.B[0])/2.0
```

python api中heapq

https://docs.python.org/zh-cn/3/library/heapq.html

（1）索引从0开始

（2）最小堆

![image-20200711154837961](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200711154837961.png)