JZ-排序&排序总结

### 最小的k的数

题目描述：

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,

方法一：蒂姆排序

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        if tinput == [] or k > len(tinput):
            return []
        tinput.sort()
        return tinput[: k]
```

方法二：快速排序

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        def quick_sort(lst):
            if not lst:
                return []
            pivot = lst[0]
            left = quick_sort([x for x in lst[1: ] if x < pivot])
            right = quick_sort([x for x in lst[1: ] if x >= pivot])
            return left + [pivot] + right
 
        if tinput == [] or k > len(tinput):
            return []
        tinput = quick_sort(tinput)
        return tinput[: k]
```

方法三：归并排序

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        def merge_sort(lst):
            if len(lst) <= 1:
                return lst
            mid = len(lst) // 2
            left = merge_sort(lst[: mid])
            right = merge_sort(lst[mid:])
            return merge(left, right)
        def merge(left, right):
            l, r, res = 0, 0, []
            while l < len(left) and r < len(right):
                if left[l] <= right[r]:
                    res.append(left[l])
                    l += 1
                else:
                    res.append(right[r])
                    r += 1
            res += left[l:]
            res += right[r:]
            return res
        if tinput == [] or k > len(tinput):
            return []
        tinput = merge_sort(tinput)
        return tinput[: k]
```

方法四：堆排序

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        def siftup(lst, temp, begin, end):
            if lst == []:
                return []
            i, j = begin, begin * 2 + 1
            while j < end:
                if j + 1 < end and lst[j + 1] > lst[j]:
                    j += 1
                elif temp > lst[j]:
                    break
                else:
                    lst[i] = lst[j]
                    i, j = j, 2 * j + 1
            lst[i] = temp
 
        def heap_sort(lst):
            if lst == []:
                return []
            end = len(lst)
            for i in range((end // 2) - 1, -1, -1):
                siftup(lst, lst[i], i, end)
            for i in range(end - 1, 0, -1):
                temp = lst[i]
                lst[i] = lst[0]
                siftup(lst, temp, 0, i)
            return lst
 
        if tinput == [] or k > len(tinput):
            return []
        tinput = heap_sort(tinput)
        return tinput[: k]
```

方法五：冒泡排序

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        def bubble_sort(lst):
            if lst == []:
                return []
            for i in range(len(lst)):
                for j in range(1, len(lst) - i):
                    if lst[j-1] > lst[j]:
                        lst[j-1], lst[j] = lst[j], lst[j-1]
            return lst
 
        if tinput == [] or k > len(tinput):
            return []
        tinput = bubble_sort(tinput)
        return tinput[: k]
```

方法六：直接选择排序

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        def select_sort(lst):
            if lst == []:
                return []
            for i in range(len(lst)-1):
                smallest = i
                for j in range(i, len(lst)):
                    if lst[j] < lst[smallest]:
                        smallest = j
                lst[i], lst[smallest] = lst[smallest], lst[i]
 
            return lst
 
        if tinput == [] or k > len(tinput):
            return []
        tinput = select_sort(tinput)
        return tinput[: k]
```

方法七：插入排序

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        def Insert_sort(lst):
            if lst == []:
                return []
            for i in range(1, len(lst)):
                temp = lst[i]
                j = i
                while j > 0 and temp < lst[j - 1]:
                    lst[j] = lst[j - 1]
                    j -= 1
                lst[j] = temp
            return lst
 
        if tinput == [] or k > len(tinput):
            return []
        tinput = Insert_sort(tinput)
        return tinput[: k]
```

### 调整顺序使得奇数位于偶数之前（IDE去调插入算法

题目描述：

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

思路1：辅助数组，遍历，奇数存一个，偶数存一个

思路2：in-place，保持稳定的排序有插入、归并等

```python
#冒泡的思想
class Solution:
    def reOrderArray(self, array):
        if not array:
            return []
        for i in range(len(array)):
            for j in range(len(array)-1, i, -1): #注意倒序时从len(array)-1开始
                if array[j]%2 == 1 and array[j-1]%2 == 0:
                    array[j-1], array[j] = array[j], array[j-1]
        return array
```

```python
# 待调试 插入的思想
# -*- coding:utf-8 -*-

class Solution:
    def reOrderArray(self, array):
        # write code here
        if not array:
            return
        i = 0 # 指示奇数的后一个位置
        j = 0
        while j < len(array):
            if array[j] % 2 == 1:
                if j == 0:
                    j+=1
                    i+=1
                else:
                    tmp = array[j]
                    for k in range(j, i, -1):
                        array[k] = array[k-1]
                    array[i] = tmp
                    i += 1
            else:
                j += 1
        #return array
    

```

