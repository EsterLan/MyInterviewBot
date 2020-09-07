# JZ- 字符串
## 题目s

### 替换空格-- 字符串

题目描述：

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

1. **难点**： 返回void， 在原字符串上操作返回

2. 解析： 遍历字符串，无非是从左往右或是从右往左。

（1）若从左往右，遇到空格填充，会覆盖后面的字符

（2）从右往左，遇到空格填充，否则移动元字符；

我的解答：O(N), O(N)

```python
# -*- coding:utf-8 -*-
class Solution:
    # s 源字符串
    def replaceSpace(self, s):
        # write code here
        if not s:
            return s
        re = ''
        for ch in s:
            if ch == ' ':
                re += '%20'
            else:
                re += ch
        return re
```



```jav
class Solution {
public:
 void replaceSpace(char *str,int length) {
     if (str == nullptr || length <= 0) return; // 养成良好习惯，判空操作
     int cnt = 0;  // 空格的个数
     for (int i=0; i != length; ++i) {
         if (str[i] == ' ') ++cnt;
     }
     if (!cnt) return; // 没有空格，直接返回
     int new_length = length+cnt*2;
     for (int i=length; i >= 0; --i) {
         if (str[i] == ' ') {
             str[new_length--] = '0';
             str[new_length--] = '2';
             str[new_length--] = '%';
         }
         else {
             str[new_length--] = str[i];
         }
     }
 }
};
```

3. 复杂度分析
   时间复杂度：O(length) 只遍历了一遍字符串
   空间复杂度：O(1) 没有开辟空间

### 翻转单词顺序列

题目描述：牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

1. 思路： 将字符串split为字符数组后，倒序再拼接

   ```python
   # -*- coding:utf-8 -*-
   class Solution:
       def ReverseSentence(self, s):
           # write code here
           s_list = s.split()
           if len(s_list) < 2:
               return s
           re =''
           cor_slist = s_list[::-1]
           for s in cor_slist:
               re = re + s + ' '
           return re[:len(re)-1]
       #Sol2
       	l=s.split(' ')
           return ' '.join(l[::-1])
   
   ```

   

### 第一个只出现一次的字符

题目描述：在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.（从0开始计数）

1. 思路：

   思路1：Hash+遍历
```python
   
# -*- coding:utf-8 -*-
class Solution:
    def FirstNotRepeatingChar(self, s):
           # write code here
           if len(s) < 0 or len(s) > 10000:
               return -1
           elif len(s) == 1:
               return 0
           ch = [0 for _ in range(128)] # 所有字母对应的ASCII码
           # 遍历填充上述数组：
           for c in s:
               ch[ord(c)] += 1
           for i in range(len(s)):
               if ch[ord(s[i])] == 1:
                   return i
           return -1
       # 哈希法
       # 遍历两次字符串
       
```

   

   思路2：两个数组b1 b2;

   - 字符出现1次:b1=1；2次及以上:b2=1;
   - 找出第一个b1==1&&b2==0的索引位置

   ```python
   
   ```

   >- 难点：用ASCII码来对应字母
   >
   >- python中字母与ASCII码的转换
   >
   >  ord(str): 字符转ASCII
   >
   >  chr(int): ASCII转字符
   >
   >Note：ASCII的局限在于只能显示26个基本拉丁字母、阿拉伯数字和英式标点符号，因此只能用于显示现代美国英语（且处理naïve、café、élite等[外来语](https://zh.wikipedia.org/wiki/外來語)时，必须去除[附加符号](https://zh.wikipedia.org/wiki/附加符號)）。虽然EASCII解决了部分西欧语言的显示问题，但对更多其他语言依然无能为力。因此，现在的软件系统大多采用[Unicode](https://zh.wikipedia.org/wiki/Unicode)。

### 字符流中第一个不重复的字符

题目描述：请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

**难点**：对字符流的理解和构思类, 如何分析

Hash+队列

```python
# -*- coding:utf-8 -*-
class Solution:
    # 返回对应char
    def __init__(self):
        self.s = ''
        self.dict = {}
    def FirstAppearingOnce(self):
        # write code here
        for s in self.s:
            if self.dict[s] == 1:
                return s
        return '#'
    def Insert(self, char):
        # write code here
        self.s = self.s + char
        if char in self.dict:
            self.dict[char] += 1
        else:
            self.dict[char] = 1
```

注意在定义时定义字典来统计字符出现次数

### 表达数值的字符串

题目描述：

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

1. 考察点：字符串，正则表达式/logic

2. 思路1:e/E进行分段，然后层层判断

   ```python
import re
   class Solution:
       def isNumerical(self, s):
           a = re.split('[eE]', s)
           if len(a) > 2:
               return False
           else:
               a = [i.strip() for i in a]
               if len(a) == 1:
                   try:
                       a[0] = float(a[0])
                       return True
                   except:
                       return False
               elif len(a) == 2:
                   if a[1].count('.')==0:
                       # e/E后半部分应该为int
                       try:
                           a[0] = float(a[0])
                           a[1] = int(a[1])
                           return True
                       except:
                           return False
               return False
   ```
   
   思路2： 正则表达式一步到位（难

   正则表达式：

   <img src="C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200709214700170.png" alt="image-20200709214700170" style="zoom:60%;" />

   ```java
   
   import java.util.regex.Pattern;
   
   public class Solution {
       public static boolean isNumeric(char[] str) {
           String pattern = "^[-+]?\\d*(?:\\.\\d*)?(?:[eE][+\\-]?\\d+)?$";
           String s = new String(str);
           return Pattern.matches(pattern,s);
       }
   }
   ```

### 字符换成整数

   题目描述：

   将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0

   ```python
class Solution:
   def StrtoInt(self, s):
       if not s:
          return 0
   numbers=['0','1','2','3','4','5','6','7','8','9']
   if s[0] in numbers:
       sign = 1
   elif s[0] == '+':
       sign = 1
       s = s[1:]
   elif s[0] == '-':
       sign = -1
       s = s[1:]
   else:
       return 0
   num = 0
   for ch in s:
       if ch in numbers:
           num = num*10 + ch.index(numbers)
       else:
           return 0
    return num*sign
   ```

   > 注意此处应该为elif, 否则会如123，对if进行判断后还会对是否为+-号进行判断，则进入else，返回0

### 扑克牌顺子

题目描述

我的思路：分为0-4个joker考虑，程序冗余

```python
class Solution:
    def IsContinuous(self, numbers):
        # write code here
        if len(numbers) != 5:
            return False
        num_joker = sum([True for num in numbers if num == 0])
        numbers = [num for num in numbers if num != 0] # 非零元素
        gap = max(numbers) - min(numbers)
        if num_joker == 0:
            return gap == 4
        elif num_joker == 1:
            return gap == 3 or gap == 4
        elif num_joker == 2:
            return gap == 2 or gap == 3 or gap == 4
        elif num_joker == 3:
            return gap == 1 or gap == 2 or gap == 3 or gap == 4
        elif num_joker == 4:
            return True
        return False
```

discuss精华：对非零元素进行讨论，排序后将其间隔之间的差值加起来<4 即成立，注意对相等元素的考量

```python
class Solution:
    def IsContinuous(self, numbers):
        if not numbers:
            return False
        #c = collections.Counter(numbers)
        #m = c[0]
        new_list = [num for num in numbers if num != 0]
        new_list.sort()
        n = 0
        for j in range(len(new_list)-1):
            if new_list[j+1] - new_list[j] > 0:
              n += new_list[j+1] - new_list[j]
            else:
                return False
         return n <= 4
```

### 左旋转字符串

题目描述：

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

考点：字符串截取&拼接

```python
class Solution:
    def LeftRotateString(self, s, n):
        if not s:
            return ''
        if n >= len(s):
            n = n % len(s)
        s_left = s[:n]
        s_right = s[n:]
        return s_right + s_left
```

### 字符串排列

题目描述：

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则按字典序打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

考点： dp，递归

我的思路：DFS（回溯）

```python
class Solution:
    def Permutation(self, ss):
        def backtrack(ss, track):
            # 结束条件
            if len(track) == len(ss) and track not in res:
                res.append(track[:])
                return
            # 对选择遍历
            for i in range(len(ss)):
                if not visited[i]:
                    # 做选择
                    track.append(ss[i])
                    visited[i] = True
                    backtrack(ss, track)
                    track.pop(-1)
                    visited[i] = False
            
        if not ss or len(ss) > 9:
            return []
        ss = sorted(list(ss))
    	res = []
        visited = [False for _ in range(len(ss))]
        track = []
        backtrack(ss, track)
        return [''.join(s) for s in res]
```

python函数简化

```python
import itertools
class Solution:
    def Permutation(self, ss):
        if not ss:
            return []
        return sorted(list(set(map(''.join, itertools.permutations(ss)))))
```

itertools对元素进行枚举，map函数map(f, *iterables)

[python标准库itertools组合生成器][https://zhuanlan.zhihu.com/p/35084184]



 ###  正则表达式匹配(难）

题目描述：

请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

考察点：动态规划 / 递归

思路：分析如下

假设主串为s，长度为`sn`， 模式串为`p`，长度为`pn`，对于模式串p当前的第`i`位来说，有`'正常字符'、'*'、'.'`三种情况。我们针对这三种情况进行讨论：

1. 如果`p[i]`为正常字符， 那么我们看`s[i]`是否等于`p[i]`, 如果相等，说明第i位匹配成功,接下来看`s[i+1...sn-1] 和 p[i+1...pn-1]`
2. 如果p[i] 为`'.'`, 它能匹配任意字符，直接看`s[i+1...sn-1] 和 p[i+1...pn-1]`
3. 如果`p[i]` 为`'*'`， 表明`p[i-1]`可以重复`0`次或者多次，需要把`p[i-1] 和 p[i]`看成一个整体.
   - 如果`p[i-1]`重复`0`次，则直接看`s[i...sn-1] 和 p[i+2...pn-1]`
   - 如果`p[i-1]`重复一次或者多次,则直接看`s[i+1...sn-1] 和p[i...pn-1]`，但是有个前提：`s[i]==p[i] 或者 p[i] == '.'`

三种情况如下图：
![图片说明](https://uploadfiles.nowcoder.com/images/20200509/284295_1589013766371_56D5186BDC1AB01FA96CC2C22507B1F7)
![图片说明](https://uploadfiles.nowcoder.com/images/20200509/284295_1589013796397_0432A5039CAF5F0EF87F6CB864FF5787)
![ ](https://uploadfiles.nowcoder.com/images/20200509/284295_1589013961988_7005C0EF0F08F8F8753D173CA5021486)
![ ](https://uploadfiles.nowcoder.com/images/20200509/284295_1589013997068_082222C32703837926302706F005BADC)
显然上述的过程可以递归进行计算。

#### way1: 递归三部曲

1. 递归函数功能：`match(s, p) -> bool`, 表示`p`是否可以匹配s
2. 递归终止条件：
   - 如果`s 和 p` 同时为空，表明正确匹配
   - 如果`s不为空，p为空`，表明，不能正确匹配
   - 如果`s为空，p不为空`，需要计算，不能直接给出结果
3. 下一步递归：
   - 对于前面讨论的情况`1，2`进行合并，如果`*s == *p || *p == '.',则match(s+1, p+1)`
   - 对于情况`3`，如果重复一次或者多次，则`match(s+1,p),如果重复0次，则match(s, p+2)`

```python
# -*- coding:utf-8 -*-
class Solution:
    # s, pattern都是字符串
    def match(self, s, pattern):
        # write code here
        # 递归出口
        if (len(s) == 0 and len(pattern) == 0):
            return True
        if (len(s) > 0 and len(pattern) == 0):
            return False
        # 分三种情况讨论
        if (len(pattern) > 1 and pattern[1] == '*'):
            if (len(s) > 0 and (s[0] == pattern[0] or pattern[0] == '.')):
                return (self.match(s, pattern[2:]) or self.match(s[1:], pattern[2:]) or self.match(s[1:], pattern))
            else:
                return self.match(s, pattern[2:])
        if (len(s) > 0 and (pattern[0] == '.' or pattern[0] == s[0])):
            return self.match(s[1:], pattern[1:])
        return False
```

#### way2：动态规划

1. 动态规划转移方程：
   `f[i][j]`表示`s`的前`i`个和`p`的前`j`个能否匹配

- 对于方法一种的`1,2`两种情况可知：`f[i][j] = f[i-1][j-1]`
- 对于第3种情况可知：
  - 如果重复`0`次，`f[i][j] = f[i][j-2]`
  - 如果重复`1`次或者多次，`f[i][j] = f[i-1][j]`

1. 动态规划初始条件：

- `s为空且p为空，为真: f[0][0] = 1`
- `s不为空且p为空,为假: f[1..sn][0] = 0`

more refer to ： [动态规划](./动态规划.md)

```python
class Solution:
    def match(self, s, p):
        m = len(s)
        n = len(p)
        # f[i][j]表示字符串s[:i]和p[:j]是否匹配
        f = [[False for _ in range(n + 1)] for _ in range(m + 1)]
        # initilization and 边界条件
        f[0][0] = True
        for j in range(1, n+1):
            # s= '' p非空
            if p[j-1] == '*':
                f[0][j] = f[0][j-2]
            else:
                f[0][j] = False
        # 遍历填充
        for i in range(1, m+1):
            for j in range(1, n+1):
                # 当前元素索引
                ii = i - 1
                jj = j - 1
                if s[ii] == p[jj] or p[jj] == '.':
                    f[i][j] = f[i-1][j-1]
                elif p[jj] == '*':
                    if s[ii] == p[jj-1] or p[jj-1] == '.':
                        f[i][j] = f[i][j-2] or f[i-1][j]
                    else:
                        f[i][j] = f[i][j-2]
                else:
                    f[i][j] = False
        return f[m][n]
```

## Summary of String 

1. list和string之间的转换：

   `str >> list`

   ```python
   str1 = '12345'
   list1 = list(str1)
   
   str2 = '1234 sjp jfkf'
   list2 = str2.split() # or list2 = str2.split(" ")
   
   str3 = 'www.google.com'
   list3 = str3.split('.')
   ```

   输出为：

   ```python
   ['1', '2', '3', '4', '5']
   ['123', 'sjp', 'jfkf']
   ['www', 'google', 'com']
   ```

   `list >> str`

   ```python
   str4 = "".join(list3)
   str5 = '.'.join(list3)
   str6 = ' '.join(list3)
   ```

   输出为

   ```python
   'wwwgooglecom'
   'www.google.com'
   'www google com'
   ```

   reference:https://blog.csdn.net/roytao2/article/details/53433373

2. 字符串的基本操作

   > 1.`字符串截取/替换/查找/分割：`
   >
   > - 截取：str[start:end]
   > - 替换：str.replace('k'，'8',[次数]) # (默认)所有k替换为8， 需重新赋值
   > - 查找： str.find('字符串'[, start, end]) # 返回位置；没有返回-1
   > - 分割： str.split('分割标示符'[，分割次数])
   > - 某个字符出现的次数: str.count('.') # '.'出现的次数
> - 去掉空格： str.strip()
   > 2. `字符串拼接--join 和 +的区别`
>    - 结果一致
   >    - 由于字符串是不可变对象，当使用“+”连接字符串的时候，每执行一次“+”操作都会申请一块新的内存，然后复制上一个“+”操作的结果和本次操作的有操作符到这块内存空间中，所以用“+”连接字符串的时候会涉及内存申请和复制；join在连接字符串的时候，首先计算需要多大的内存存放结果，然后一次性申请所需内存并将字符串复制过去。在用"+"连接字符串时，结果会生成新的对象，而用join时只是将原列表中的元素拼接起来，因此在连接字符串数组的时候会考虑优先使用join。

    *Reference:*

    [1]https://blog.csdn.net/zhuhai__yizhi/article/details/77581625

    [2]]https://blog.csdn.net/cxj540947672/java/article/details/104892237
   
3. 一些重复字符串判断问题的常用思路--Hash+遍历 / re模块

   Hash表构建采用`列表（索引对应字符ASCII码的方式）或字典`

   ```python
   ch = [0 for _ in range(128)] # 统计ascii码为0-127的字符出现次数,索引为ascii码
   # ord(char): 字符->ASCII
   # chr(int) : ASCII->字符
   ch[ord(char)] == 1 # char在当前字符串出现次数是否等于1
   ```

   re module

   ```python
   # 分割s， 从e/E分割，返回列表a为分割后的字符串
   a = re.split('[eE]', s)
   ```

   

4. 与数值之间转换常用

   ```python
   try:
       a = float(a)
       b = int(b)
       return True
   except:
       return False
   ```

   

   
