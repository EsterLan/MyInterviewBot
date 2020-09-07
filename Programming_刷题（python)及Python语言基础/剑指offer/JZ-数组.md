## JZ --数组

### 数组中的重复数字-- 数组

题目描述：在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

1. 考察点：数组

2. 解析：

   （1）哈希+遍历：

   题目中含有**重复**的字眼，第一反应应该想到哈希，set。这里我们用哈希来解。
   算法步骤：

   1. 开辟一个长度为n的vector<bool>, 初始化为false</bool> 
   2. 遍历数组，第一次遇到的数据，对应置为true 
   3. 如果再一次遇到已经置为true的数据，说明是重复的。返回即可。

   ```python
    # Solution1 : Hash
           # O(n)， O（n）
           if len(numbers) < 2:
               return False
           # 开辟新数组0~n-1，将其全部设置为False
           flag = [0 for _ in range(len(numbers))]
           # 遍历数组，访问过的数据设为1；访问下一元素时，对应位置为1，则返回该元素
           for num in numbers:
               if flag[num] == 0:
                   # 首次访问， 将flag设为1
                   flag[num] = 1
               else:
                   # 对应flag为1,为重复数字
                   duplication[0] = num
                   return True
           return False
   ```

   (2)in_place

   链接：https://www.nowcoder.com/questionTerminal/623a5ac0ea5b4e5f95552655361ae0a8?answerType=1&f=discussion
   来源：牛客网

   

   方法一中的一个条件我们没有用到。也就是数据的范围是0-n-1。所以我们可以这么做：

   1. 设置一个指针i指向开头0，
   2. 对于arr[i]进行判断，如果arr[i] == i， 说明下标为i的数据**正确的放在了该位置上**，让i++
   3. 如果arr[i] != i, 说明没有正确放在位置上，那么我们就把arr[i]放在正确的位置上，也就是交换
      arr[i] 和arr[arr[i]]。交换之后，如果arr[i] ！= i, 继续交换。
   4. 如果交换的过程中，arr[i] == arr[arr[i]]，说明遇到了重复值，返回即可

   O(N) O(1)

   ```python
def duplicate(self, numbers, duplication):
           # write code here
        # Solution2 : 应用0~n-1
           # in-place O(N) O(1)
           if len(numbers) < 2:
               return False
           i = 0
           while i < len(numbers):
               if numbers[i] == i:
                   i += 1
               while numbers[i] != i:
                   if numbers[i] == numbers[numbers[i]]:
                       duplication[0] = numbers[i]
                       return True
                   numbers[i], numbers[numbers[i]] = numbers[numbers[i]], numbers[i]
           return False
   ```

### 构建乘积数组--逻辑抽象

题目描述：给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。（注意：规定B[0] = A[1] * A[2] * ... * A[n-1]，B[n-1] = A[0] * A[1] * ... * A[n-2];）

1. 考察点：不能用除法，难点在于如何简化计算

2. 思路：

![image-20200620094034288](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200620094034288.png)

```python
class Solution:
    def multiply(self, A):
        # write code here
        if not A:
            return []
        B = [0 for _ in range(len(A))]
        # 计算下三角部分
        B[0] = 1
        for i in range(1, len(A)):
            B[i] = B[i-1]*A[i-1]
        # 计算上三角部分
        tmp = 1
        for j in range(len(A)-2, -1, -1):
            tmp *= A[j+1]
            B[j] *= tmp            
        return B
```



3. 复杂度：O(N) O(1)

### 数组中只出现一次的数字

题目描述：一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

1. 考察点

2. 思路:

   思路1：Hash + 遍历

   ```python
    def FindNumsAppearOnce(self, array):
           # write code here
           if not array:
               return []
           elif len(array) < 3:
               return array
           # Hash O(N) O(N)
           count = [0 for _ in range(max(array)-min(array)+1)]
           for num in array:
               count[num-min(array)] += 1
           re = []
           for i in range(len(count)):
               if count[i] == 1:
                   re.append(i+min(array))
               if len(re) == 2:
                   return re
   ```

   **利用list的函数**

   ```python
    def FindNumsAppearOnce(self, array):
           # write code here
           if not array:
               return []
           tmp = []
           for a in array:
               if a in tmp:
                   tmp.remove(a)
               else:
                   tmp.append(a)
           return tmp
   ```

   

   思路2：位运算 **没看懂，不知道实现 后看**

   ![image-20200620104039872](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200620104039872.png)

   https://www.nowcoder.com/questionTerminal/e02fdb54d7524710a7d664d082bb7811?answerType=1&f=discussion

3. 复杂度

### 和为S的连续正整数

题目描述：

#### 思路：

- 思路1：前缀法

  ![image-20200624111544948](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200624111544948.png)

- 思路2：滑动窗口（未编译过，请接着编）

  ![image-20200624111618771](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200624111618771.png)

```python
# -*- coding:utf-8 -*-
class Solution:
    def FindContinuousSequence(self, tsum):
        # write code here
        # 前缀法：O（N^2）,O(1)
        if tsum < 3:
            return []
        re = []
        for i in range(1, tsum//2+2):
            tmp_sum = i
            tmp_l = [i]
            while tmp_sum < tsum:
                cur = i + 1 
                tmp_sum += cur
                tmp_l.append(cur)
                if tmp_sum == tsum:
                    re.append(tmp_l)
                    break
                i = cur
        return re
        
         # 滑动窗口
        if tsum < 3:
            return []
        lo = hi = 1
        tmp = 0        
        re = []
        while lo <= tsum/2:
    
```



### 和为s的两个数

题目描述：输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的

1.思路：

思路1：hash遍历. enumerate的用法

```python
# -*- coding:utf-8 -*-
class Solution:
    def FindNumbersWithSum(self, array, tsum):
        # write code here
        ls = []
        if not isinstance(array, list):
            return ls
        for i, v in enumerate(array):
            for v1 in array[i:]:
                if (v + v1) == tsum:
                    ls.append([v, v1])
        if ls:
            return ls[0]
        else:
            return l
```



 思路2：双指针法， 左右夹逼，由于是递增排序数组

（1）将两个指针i，j分别初始化为首尾元素

（2）a[i] + a[j] < s, 则i++; a[i] + a[j] > s, 则j--;a[i] + a[j] == s, 将其存起来；

（3）注意对上下指针重叠判断

```python
# -*- coding:utf-8 -*-
class Solution:
    def FindNumbersWithSum(self, array, tsum):
        # write code here
        # 参数合法性
        if len(array) < 2 or tsum < array[0]:
            return []
        # 双指针
        down = 0 
        up = len(array) - 1
        product = list()
        re = list()
        while down <= len(array)/2:
            if array[down] + array[up] < tsum:
                down += 1
            elif array[down] + array[up] > tsum:
                up -= 1
            else:
                if down == up:
                    break
                re.append([array[down], array[up]])
                product.append(array[down]*array[up])
                down += 1
                up -= 1
        if len(re) == 0:
            return []
        return re[product.index(min(product))]
```

复杂度：O（N）， O（1）



### 不用加减乘除做加法（背

题目描述：

写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

思路： 位运算

![image-20200709205155455](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200709205155455.png)

```python
MAX_BIT = 2**32
MAX_BIT_COMPLIMENT = -2**32
class Solution:
    def Add(self, num1, num2):
        while num2 != 0:
            if num2 == MAX_BIT:
                return num1 ^ MAX_BIT_COMPLIMENT
            carry = (num1 & num2) << 1
            num1 = num1 ^ num2 # 相加
            num2 = carry # 进位
        return num1
```



### 顺时针打印数组

题目描述：

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

1. 考察点： 数组操作

2. 思路：

   **思路1**： 打印第一行；然后去掉第一行；逆时针旋转数组；重复上述直至数组为空

   > 踩过的坑：1. 旋转数组前，应该对数组是否为空进行判断；2. 如何逆时针旋转数组

   ```python
   # -*- coding:utf-8 -*-
   class Solution:
       # matrix类型为二维列表，需要返回列表
       def printMatrix(self, matrix):
           # write code here
           if not matrix:
               return []
           res = []
           while matrix:
               # 读取首行
               #for i in range(len(matrix[0])):
                #   res.append(matrix[0][i])
               res.extend(matrix[0])
               # matrix去掉首行，逆时针旋转
               matrix.pop(0)
               if matrix:
                   matrix = [[row[i] for row in matrix] for i in range(len(matrix[0])-1, -1, -1)]
           return res
   
   ```

   **思路2**: 打印第一行；对数组进行判断，若数组多余1行，则去掉第一行，再循环；否则，退出循环（=1时，已经打印了第一行）

   这样可以在列数很大时，少旋转一次

   ```python
   class Solution:
       def printMatrix(self, matrix):
           outlist = []
           while 1:
               outlist.extends(matrix[0])
               # 当前matrix超过1行, 去掉首行，逆时针旋转
               if len(matrix) > 1:
                   matrix = matrix[1:]
                   matrix = [[row[i] for row in matrix] for i in range(len(matrix[0])-1, -1, -1)]
               else:
                   break
           return outlist
   ```

   

**Notes**: 1. list.extends(iterable) 

   2. 二维数组的旋转

      ```python
      # 逆时针旋转
      matrix = [[row[i] for row in matrix] for i in range(len(matrix[0])-1, -1, -1)]
      # 顺时针旋转
      matrix = [[matrix[i][j] for i in range(len(matrix)-1, -1, -1)] for j in range(len(matrix[0]))]
      # 转置
      matrix = [[row[i] for row in matrix] for i in range(len(matrix[0]))]
      ```

### 数组中的逆序对（再写）

题目描述：

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

1. 考察点： 归并排序

2. 思路：

   我的解法： 动态规划；**注意理解题意**，理解成相邻的两个数是不对的

   思路1： 暴力解法 O(N^2) O(1) -> 超时

   思路2：归并 没有看到python有通过的

   在归并中进行统计，利用有序的特性;需要辅助空间

   O(NlogN) O(N)

   ```java
   链接：https://www.nowcoder.com/questionTerminal/96bd6684e04a44eb80e6a68efc0ec6c5?answerType=1&f=discussion
   来源：牛客网
   
   
   public class Solution {
   
       static int mod = (int) 1e9 + 7;
       static int count = 0;
       static int[] arr = new int[220000];
   
       public static void Merge(int l, int mid, int r, int[] array) {
           int i = l, j = mid + 1, k = l;
           while (i <= mid && j <= r) {
               if (array[i] <= array[j]) {
                   arr[k++] = array[i++];
               } else {
                   count = (count + mid - i + 1) % mod;
                   arr[k++] = array[j++];
               }
           }
           while (i <= mid) {
               arr[k++] = array[i++];
           }
           while (j <= r) {
               arr[k++] = array[j++];
           }
           for (i = l; i <= r; i++) {
               array[i] = arr[i];
           }
       }
   
       public static void MergeSort(int l, int r, int[] array) {
           if (l < r) {
               int mid = (l + r) >> 1;
               MergeSort(l, mid, array);
               MergeSort(mid + 1, r, array);
               Merge(l, mid, r, array);
           }
       }
   
       public static int InversePairs(int[] array) {
           if (array == null || array.length == 0) {
               return 0;
           }
           MergeSort(0, array.length - 1, array);
           return count;
       }
   
   }
   ```

   

   