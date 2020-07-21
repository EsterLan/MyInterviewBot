## leetcode经典题解

OUTLINE

- 整数转罗马数字easy
- 罗马数字转整数 easy

### 整数转罗马数字 & 罗马数字转整数

题目描述：


请将给出的整数转化为罗马数字

保证输入数字的范围在1 到 3999之间。

```python
Class Solution:
    def intToRoman(self, num):
        if not num:
            return ''
        roman = ['I', 'IV', 'V', 'IX', 'X', 'XL', 'L', 'XC', 'C', 'CD', 'D', 'CM', 'M']
        integer = [1, 4, 5, 9, 10, 40, 50, 90, 100, 400, 500, 900, 1000]
        res = ''
        for i in range(len(roman)-1, -1, -1):
           	while num > integer[i]:
                res += roman[i]
                num -= integer[i]
        return res
        
```



```python
Class Solution:
    def intToRoman(self, s):
        if not s:
            return ''
        roman = ['I', 'V', 'X', 'L', 'C', 'D', 'M']
        integer = [1, 5, 10, 50, 100, 500, 1000]
        if len(s) == 1:
            # 字符串长度为1
            if s in roman:
                return integer[roman.index(s)]
            else:
                return 0
        else:
            # 2个字符及以上
            i = len(s) - 1 # 末尾字符索引
            while i > 0:
                # 从末尾开始比较两个
                index_cur = roman.index(s[i])
                index_left = roman.index(s[i-1])
                if index_left < index_cur:
                    # 两个字符表示一个数字
                    res += integer[index_cur] - integer[index_left]
                    i -= 2
                else:
                    res += integer[index_cur]
                    i -= 1
            # 对s[0]进行判断，仅对其单独表达一个数字考虑
            if roman.index(s[0]) > roman.index(s[1]):
                res += intefer[roman.index(s[0])]
                    
        
```

**Note:**注意书写过程中思路的清晰性和边界的考虑。第一次写没有通过就是因为：if index_left and index_left < index_cur这句；还有对最高位的判断

另一：字典的应用

```python
dirt={'I':1,'V':5,'X':10,'L':50,'C':100,'D':500,'M':1000}
class Solution:
    def romanToInt(self , s ):
        i,i0,i1=0,0,0
        if s == '':
            return 0# write code here
        chars = list(s)
        i = 0
        for n in range(len(chars)-1):
            if self.check(chars[n])>=self.check(chars[n+1]):
                i += self.check(chars[n])
            else:
                i -= self.check(chars[n])
        i += self.check(chars[-1])
        return i
     
    def check(self,s):
        num = dirt.get(s)
        return num
```

### Longest substring without repeatation

题目描述：给定一个字符串，找出最长的不具有重复字符的子串的长度。例如，“abcabcbb”不具有重复字符的最长子串是“abc”，长度为3。对于“bbbbb”，最长的不具有重复字符的子串是“b”，长度为1。

```python
#
# 
# @param s string字符串 
# @return int整型
#
class Solution:
    def lengthOfLongestSubstring(self , s ):
        # write code here
        if not s:
            return 0
        elif len(s) < 2:
            return len(s)
        res = s[0]
        length = 1
        for ch in s[1:]:
            if ch in res:
                # 碰到重复, 比较当前长度和新的字串长度，并将子串置为当前重复元素
                length = max(length, len(res))
                res = res[(res.index(ch)+1):] + ch
            else:
                # 不同元素则添加到子串后
                res += ch
        return length
```

注意：第一次写通过60%的原因。对重复的错误，不应该置空res，而是从重复字符处截断

