# JZ- 字符串

### 替换空格-- 字符串

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

   >1. 字符串截取/替换/查找/分割：
   >   - 截取：str[start:end]
   >   - 替换：str.replace('k'，'8',[次数]) # (默认)所有k替换为8， 需重新赋值
   >   - 查找： str.find('字符串'[, start, end]) # 返回位置；没有返回-1
   >   - 分割： str.split('分割标示符'[，分割次数])
   >2. 字符串拼接--join 和 +的区别
   >
   >- 结果一致
   >
   >-  由于字符串是不可变对象，当使用“+”连接字符串的时候，每执行一次“+”操作都会申请一块新的内存，然后复制上一个“+”操作的结果和本次操作的有操作符到这块内存空间中，所以用“+”连接字符串的时候会涉及内存申请和复制；join在连接字符串的时候，首先计算需要多大的内存存放结果，然后一次性申请所需内存并将字符串复制过去。在用"+"连接字符串时，结果会生成新的对象，而用join时只是将原列表中的元素拼接起来，因此在连接字符串数组的时候会考虑优先使用join。
   >
   >  *Reference:*
   >
   >  [1]https://blog.csdn.net/zhuhai__yizhi/article/details/77581625
   >
   >  [2]]https://blog.csdn.net/cxj540947672/java/article/details/104892237

### 第一个只出现一次的字符

题目描述：在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.（从0开始计数）

1. 思路：

   思路1：Hash+遍历

   思路2：两个数组b1 b2;

   - 字符出现1次:b1=1；2次及以上:b2=1;
   - 找出第一个b1==1&&b2==0的索引位置

   ```python
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

### 表达数值的字符串

题目描述：

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

1. 考察点：字符串，正则表达式/logic

2. 思路1:e/E进行分段，然后层层判断

   思路2： 正则表达式一步到位（难

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

   ### 字符表示数值

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

   ### 正则表达式

   



