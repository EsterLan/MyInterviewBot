# LeetCode 02

## LC142. reverse_integer:easy

题目描述：将给出的整数x翻转。
例1:x=123，返回321
例2:x=-123，返回-321

你有思考过下面的这些问题么？

如果整数的最后一位是0，那么输出应该是什么？比如10,100
你注意到翻转后的整数可能溢出吗？假设输入是32位整数，则将翻转10000000003就会溢出，你该怎么处理这样的样例？抛出异常？这样做很好，但是如果不允许抛出异常呢？这样的话你必须重新设计函数（比如添加一个额外的参数）。

**难点**：对溢出情况的考察

我的解法：

```python
#
# 
# @param x int整型 
# @return int整型
#
class Solution:
    def reverse(self , x ):
        # write code here
        # 保存正负号
        sign = 1
        if x < 0:
            sign = -1
        s = str(abs(x))[::-1]
        if len(s) == 10 and s[-1] != '0':
            if self.isOverflow(x):
                return 0
        reversed_x = int(s)
        return sign * reversed_x
    
    def isOverflow(self, x):
        # 判断x的翻转是否溢出
        s = str(abs(x))
        res = 0
        for i in range(len(s)-1, -1, -1):
            res += 10**(i-len(s)+1) * int(s[i])
        MAX_VALUE = 2**31 - 1
        return res > MAX_VALUE
        
```

别人简短的解法：

```python
class Solution:
    def reverse(self , x ):
        if x==0:
            return 0
        s=str(x)
        if(x>0):
            x=int(s[::-1])
             
            return x
        if(x<0):
            x=int(s[1:][::-1])
             
            return -x
         
    def isoverflow(self,x):
        if x>0x7fffffff or x<0x80000000:
            return 1
        return 0
```

**注意：**此处对界限的判断。0x7fffffff 0x80000000

## LC140 palindrome-number easy

题目描述：在不使用额外的内存空间的条件下判断一个整数是否是回文

提示：

负整数可以是回文吗？（比如-1）

如果你在考虑将数字转化为字符串的话，请注意一下不能使用额外空间的限制

你可以将整数翻转。但是，如果你做过题目“Reverse Integer”，你会知道将整数翻转可能会出现溢出的情况，你怎么处理这个问题？

这道题有更具普遍性的解法。

**难点**：不使用字符串进行判断（不能使用额外空间; 负数为非回文

考点：整数构造。a//10右移。 a*10左移。a%10取末尾元素

我的解法

```python
#
# 
# @param x int整型 
# @return bool布尔型
#
class Solution:
    def isPalindrome(self , x ):
        # write code here
        if x == 0:
            return True
        if x < 0:
            # 负数非回文
            return False
        count = 0 # x的位数
        while x > 0:
            count += 1
            x = x // 10
        i, j = 0, count-1 # 指示最高位和最低位置
        power = count - 1
        while i < j:
            lo = x%10
            hi = (x // (10**power))%10
            if lo != hi:
                return False
            x = x // 10
            i += 1
            j -= 1
            power -= 2
        return True
        
```

**重新构造数的解法（取这个**

```python
class Solution:
    def isPalindrome(self, x):
        if x == 0:
            return True
        elif x < 0:
            return False
        reverse = 0
        t = x
        while t:
            reverse = reverse * 10 + t % 10
            t = t // 10
       return reverse == x
```

溢出也进行了考虑

### LC longest-common-prefix

题目描述：

编写一个函数来查找字符串数组中的最长公共前缀。

Write a function to find the longest common prefix string amongst an array of strings.

1. 思路：遍历字符串数组，遍历第一个字符串，若有不相同的字符串，退出循环，返回公共字符串；

2. 坑：1. 考虑没有公共前缀的情况；2. 不知道为啥答案写的过了，且并没有判断是否为a-zA-Z

```python
class Solution:
    def longestCommonPrefix(self , strs ):
        # write code here
        if not strs: return ''
        res=strs[0]
        for i in range(1,len(strs)):
            j=0
            while j<len(res) and j<len(strs[i]):
                if res[j]!=strs[i][j]:
                    break
                j+=1
            res=res[0:j]
            if res=='': return res
        return res
```

我的写法：

```python
class Solution:
    def longestCommonPrefix(self , strs ):
        # write code here
        if not strs: 
            return ""
        alphabet =  [chr(i) for i in range(97,123)]
        if len(strs) == 1:
            if strs[0] not in alphabet:
                return ""
            else:
                return strs[0]
        prefix = ''
        for i in range(len(strs[0])):
            prefix += strs[0][i]
            for string in strs[1:]:
                if string[:(i+1)] != prefix:
                    return prefix[:(len(prefix)-1)]
        
```

相比较标准写法不好的地方：

1. 思路不明晰：如都是将第一个字符串与后边作比较，应该将strs写在外层，里层用while进行相关操作

2. for i in range(1,1)是不会报错的，只是默认不执行。

   另： Python 一句话生成字母表：

   

   ``` python
   # List
   s =  [chr(i) for i in range(97,123)]
   # ['a', 'b', 'c', ....]
   # String
   ''.join([chr(i) for i in range(97, 123)])
   # 两句话
   import string
   string.ascii_lowcase
   #'abcd..'
   # dict
   dict.fromkeys(string.ascii_lowcase, 0)
   # {'a': 0, 'b': 1, ....}
   ```

   