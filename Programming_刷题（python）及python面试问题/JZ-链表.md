## JZ --链表

#### 孩子的游戏（圆圈中最后剩下的数）

题目描述：每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)

如果没有小朋友，请返回-1

1. 思路：

   - 遍历数组

   - 递归法：p230-231

     ```python
     # -*- coding:utf-8 -*-
     class Solution:
         def LastRemaining_Solution(self, n, m):
             # write code here
             if n <1 or m < 1:
                 return -1
             else:
                 last = 0
                 for i in range(2, n+1):
                     last = (last + m)%i
                 return last
             if not m or not n:
                 return -1
             res = range(n)
             i = 0
             while len(res)>1:
                 i = (m+i-1)%len(res)
                 res.pop(i)
             return res[0]
     ```

     



#### 删除重复节点

题目描述：在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5  

1. 考察点：单链表
2. 思路: 主要是对头结点是否重复进行考量

思路1： in-place删除

```python
class Solution:
    def deleteDuplication(self, pHead):
        # write code here
        if not pHead or not pHead.next:
            return pHead
        vHead = ListNode(0)
        vHead.next = pHead
        pre, cur = vHead, pHead
        while cur:
            # 头结点重复
            if cur.next and cur.val == cur.next.val:
                cur = cur.next
                while cur.next and cur.val == cur.next.val:
                    cur = cur.next
                cur = cur.next
                pre.next = cur
            # 头结点为非重复结点
            else:
                pre, cur = cur, cur.next
        return vHead.next
```

注意：此时头结点重复的情况中，无需对是否为末尾节点进行考量，这是因为：若尾结点与头节点相同，则返回cur.next=None;若不相同，则会在cur.val == cur.next.val中跳出，而不是cur.next中跳出；

与递归思路不同的是：vHead是指向pHead，而aft在pHead之后

思路2：递归 

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def deleteDuplication(self, pHead):
        # write code here
        if not pHead or not pHead.next:
            return pHead
        
        aft = pHead.next
        # 若头结点为非重复字符，保留当前节点，则pHead.next为后半部分del
        if pHead.val != aft.val:
            pHead.next = self.deleteDuplication(aft)
        # 若头结点为重复字符, 对其后的节点，进行判断，相同则删除，知道为不相同节点
        else:
            while aft.val == pHead.val and aft.next:
                aft = aft.next
            # 跳出循环后：aft为不重复节点或者最后一个节点
            # 进行操作：删除pHead；
            # 特殊情况aft为最后一个节点且值相同
            if aft.val == pHead.val:
                return None
            else:
                pHead = self.deleteDuplication(aft) # 为aft， pHead.next无效
        return pHead
        
```
注意：重复头结点中的一句注释。
```python
aft = pHead.next
aft = aft.next
aft == pHead.next ### False
```

#### 两个链表的公共节点

题目描述：输入两个链表，找出它们的第一个公共结点。（注意因为传入数据是链表，所以错误测试数据的提示是用其他方式显示的，保证传入数据是正确的）

1. 考点： 链表

2. 思路：

   思路1： Hash +遍历

   思路2： 双指针法

   <img src="C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200619102551551.png" alt="image-20200619102551551" style="zoom:67%;" />

   ![image-20200619102635666](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200619102635666.png)

   ```python
   # -*- coding:utf-8 -*-
   # class ListNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.next = None
   class Solution:
       def FindFirstCommonNode(self, pHead1, pHead2):
           # write code here
           # 双指针法
           if not pHead1 or not pHead2:
               return None
           ta = pHead1
           tb = pHead2
           while ta != tb:
               ta = ta.next
               tb = tb.next
               if ta != tb: #为了不让其进入while循环而无法终止
                   if ta == None:
                       ta = pHead2
                   if tb == None:
                       tb = pHead1
           return ta
          
           # Hash
           # O(m+n) O(m)
           list1 = []
           list2 = []
           node1 = pHead1
           node2 = pHead2
           while node1:
               list1.append(node1)
               node1 = node1.next
           while node2:
               if node2 in list1:
                   return node2
               else:
                   node2 = node2.next
           
   ```

   

3. 空间复杂度：

   1：O(m+n) O(n)

   2: O(m+n) O(1)

#### 链表中环的入口节点

题目描述：

1. 思路

   思路1： Hash + 遍历

   思路2: 双指针

   

   ![image-20200619104621950](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200619104621950.png)

   ```python
   # -*- coding:utf-8 -*-
   # class ListNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.next = None
   class Solution:
       def EntryNodeOfLoop(self, pHead):
           # write code here
           # Hash
           if not pHead or not pHead.next:
               return None
           
           visited = [pHead]
           while pHead.next:
               if pHead.next in visited:
                   return pHead.next
               visited.append(pHead.next)
               pHead = pHead.next
           return None
           
             # 双指针法
           if not pHead or not pHead.next:
               return None
           slow = fast = pHead
           while fast and fast.next:
               slow = slow.next
               fast = fast.next.next
               if fast == slow:
                   # 定位相遇的节点
                   slow2 = pHead
                   while slow != slow2:
                       slow = slow.next
                       slow2 = slow2.next
                   return slow2
           return None
   ```

2. 复杂度

   1：O(N)  O(N)

   2：O(N) O(1)

### 复杂节点的深拷贝

题目描述：输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针random指向一个随机节点），请对此链表进行深拷贝，并返回拷贝后的头结点。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

1. 考察点：深拷贝

   赋值引用，浅拷贝和深拷贝

   <img src="C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200707115607448.png" alt="image-20200707115607448" style="zoom:50%;" />

2. 思路：

   思路1： map映射保存

   ```PYTHON
   # -*- coding:utf-8 -*-
   # class RandomListNode:
   #     def __init__(self, x):
   #         self.label = x
   #         self.next = None
   #         self.random = None
   class Solution:
       def deepCopy(self, pHead):
           if not pHead:
               return None
           new_head = None
           p_head = None
           old_new_dic = {}
           random_dic = {}
           while pHead:
               node = RandomListNode(pHead.label)
               node.random = pHead.random
               old_new_dic[id(pHead)] = id(node)
               random_dic[id(node)] = node
               pHead = pHead.next
               # 连接新节点
               if new_head:
                   new_head.next = node
                   new_head = new_head.next
               else:
                   # new_head前一节点 p_head保存头结点
                   new_head = p_head = node
            # 对random进行修改：
           new_head = p_head
           while new_head:
               if new_head.random != None:
                   # 当前节点有random时，将其指向新链表的random
                   new_head.random = random_dic[old_new_dic[id(new_head.random)]]
               new_head =  new_head.next
           return p_head
               
   ```

   

   思路2：新节点插入到老节点后，

   <img src="C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200707114241954.png" alt="image-20200707114241954" style="zoom:50%;" />





#### Summary：链表常用操作

### 扑克牌顺子：理清逻辑和关键判断条件

题目描述：

LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。

1. 考察点：抽象、逻辑具体化

2. 思路：要满足上述，应当满足3个条件

   a. max - min < 5

   b. 除了0外没有重复数字

   c. 数组长度为5(最大为13，最小为0)

   思路1：set + 遍历。非零元素set+遍历

   思路2：排序 + 遍历。排序后重复元素相邻

   ```python
   # -*- coding:utf-8 -*-
   class Solution:
       def IsContinuous(self, numbers):
           # write code here
           # 满足条件：
           # 1.max-min<5
           # 2.除了0外没有重复的数字
           # 3.数组长度为    5
           # set + 遍历
           # O(N), O(N)
           if len(numbers) != 5:
               return False
           # 非0元素且不重复元素(用python set)
           new_numbers = []
           cnt = 0 # 非0元素的个数
           for num in numbers:
               if num > 0:
                   cnt += 1
                   if num not in new_numbers:
                       new_numbers.append(num)
           if len(new_numbers) != cnt:
               return False
           return max(new_numbers) - min(new_numbers) < 5:
                   
               
           # sol2: 排序 + 遍历
           # O(Nlogn)， O（1）
           if len(numbers) != 5:
               return False
           new_numbers = sorted(numbers)
           down = 0 #第一个非0元素的位置
           for i in range(len(new_numbers)):
               if new_numbers[i] == 0:
                   down += 1
                   continue
               # 判断是否有重复元素,若有则返回
               if (i+1 < len(new_numbers) and new_numbers[i] == new_numbers[i+1]):
                   return False
           return (new_numbers[-1]-new_numbers[down])<5
               
           
               
           if len(numbers) != 5:
               return False
           # 0的个数
           cnt = sum([x==0 for x in numbers])
           if cnt == 0:
               min_num = min(numbers)
               return sum(numbers)==(min_num*5+10)
           else:
               new_numbers = []
               for num in numbers:
                   if num != 0:
                       new_numbers.append(num)
               if cnt == 1:
                   least = min(new_numbers)*4+6
                   re_list = [least+i for i in range(4)]
                   return sum(new_numbers) in re_list
               elif cnt == 3:
                   least = min(new_numbers)*2+1
                   re_list = [least+i for i in range(4)]
                   return sum(new_numbers) in re_list
               elif cnt == 2:
                   least = min(new_numbers)*3+3
                   re_list = [least+i for i in range(2,5)]
                   return sum(new_numbers) in re_list
               else:
                   return True
   
   ```

   

3. 复杂度：

   1:O(N) O(N)

   2.O(NlogN) O(1)

   

