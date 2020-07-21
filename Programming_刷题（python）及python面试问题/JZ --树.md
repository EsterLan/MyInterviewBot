## JZ --树

### 二叉树的下一节点 ***

题目描述：给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

1. 考察点：二叉树，中序遍历，查找节点

2. 解析：

   （1）需要考虑3种情况	

   1. 二叉树为空，返回
   2. 节点有右子树，返回右子树的最左节点
   3. 节点无右子树，若该节点为其父节点的左子树，则返回父节点；否则，父节点上移
   
   ```python
   # -*- coding:utf-8 -*-
   # class TreeLinkNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   #         self.next = None
   class Solution:
       def GetNext(self, pNode):
           # write code here
           if not pNode:
               return None
           # 有右子树,只用对其右子树进行考虑
           elif pNode.right:
               pRight = pNode.right
               while pRight.left:
                   pRight = pRight.left
               return pRight
           # 无右子树
           while pNode.next:
               tmp = pNode.next
               if tmp.left == pNode:
                   return tmp
               pNode = tmp
           return None
   ```

### 二叉树的深度

题目描述：输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

1. 考察点：二叉树的深度

2. 思路1: 分治法：O(N), O(n)*退化为链表*


```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def TreeDepth(self, pRoot):
        # write code here
        if not pRoot:
            return 0
        lval = self.TreeDepth(pRoot.left)
        rval = self.TreeDepth(pRoot.right)
        return max(lval, rval)+1
```

思路2：BFS --O(N) O(N)*最差情况下，树平衡时，队列最多存储n/2个节点。*

```python
class Solution:
    def TreeDepth(self, pRoot):
        # write code here
        if not pRoot:
            return 0
        # BFS : O(N) O(N)
        tmp = [pRoot]
        cnt = 0
        while tmp:
            next_level = []
            while tmp:
                node = tmp.pop(0)
                if node.left:
                    next_level.append(node.left)
                if node.right:
                    next_level.append(node.right)
            tmp = next_level
            cnt += 1
        return cnt
```



### 二叉搜索树的第k个节点（BFS模板）

题目描述：给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）  中，按结点数值大小顺序第三小结点的值为4。

1. 考察点： BST的性质，中序遍历（inOder）

2. 思路:

   思路1：将BST的中序遍历进行存储，即为递增数列

   ```python
   # -*- coding:utf-8 -*-
   # class TreeNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   class Solution:
       # 返回对应节点TreeNode
       def __init__(self):
           self.re = []
           
       def KthNode(self, pRoot, k):
           # write code here
           if not pRoot or not k:
               return None
           self.inOrder(pRoot)
           if len(self.re) < k:
               return None
           else:
               return self.re[k-1]
           
       def inOrder(self, pRoot):
           if not pRoot:
               return None
           self.inOrder(pRoot.left)
           self.re.append(pRoot)
           self.inOrder(pRoot.right)
   ```

   **利用global变量的方法**

   ```python
   class Solution:
       # 返回对应节点TreeNode
       def KthNode(self, pRoot, k):
           # write code here
           #第三个节点是4
           #前序遍历5324768
           #中序遍历2345678
           #后序遍历2436875
           #所以是中序遍历，左根右
           global result
           result=[]
           self.midnode(pRoot)
           if  k<=0 or len(result)<k:
               return None
           else:
               return result[k-1]
                 
       def midnode(self,root):
           if not root:
               return None
           self.midnode(root.left)
           result.append(root)
           self.midnode(root.right)
   ```

   

   > 优化点： 如何在寻找到第k个节点时停止无用搜索，剪枝

   ```python
   # -*- coding:utf-8 -*-
   # class TreeNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   class Solution:
       # 返回对应节点TreeNode      
       def __init__(self):
           self.count = 0 # 计数器
       def KthNode(self, pRoot, k):
           # write code here
           if not pRoot or not k:
               return None
           # 左子树中寻找
           node = self.KthNode(pRoot.left, k)
           if node:
               return node
           self.count += 1
           if self.count == k:
               return pRoot
           node = self.KthNode(pRoot.right, k)
           if node:
               return node
           return None
   ```

   ***DFS: 先/中/后序遍历二叉树*(递归版本)**
   
   ```python
   def SearchTree(head):
       if not head:
           return None
       
       print(head.data)        # 先序
       SearchTree(head.left)
       # print(head.data)      # 中序
       SearchTree(head.right)
       # print(head.data)      # 后序
   ```
   
   

### 把二叉树打印成多行及其变种（之字形打印）

题目描述：从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

1. 考察点：BFS

```python
# simpler but same meaning version
class Solution:
    # 返回二维列表[[1,2],[4,5]]
    def Print(self, pRoot):
        # write code here
        # BFS
        if not pRoot:
            return []
        tmp = [pRoot]
        res = []
        while tmp:
            row = []
            next_level = []
            while tmp:
                node = tmp.pop(0)
                row.append(node.val)
                if node.left:
                    next_level.append(node.left)
                if node.right:
                    next_level.append(node.right)
            res.append(row)
            tmp = next_level
        return res
```



```python
# my codes
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回二维列表[[1,2],[4,5]]
    def Print(self, pRoot):
        # write code here
        # BFS
        if not pRoot:
            return []
        re = []
        tmp_up = [pRoot]
        tmp_down = []
        while tmp_up:
            re.append(tmp_up)
            for node in tmp_up:
                if node.left:
                    tmp_down.append(node.left)
                if node.right:
                    tmp_down.append(node.right)
            tmp_up = tmp_down
            tmp_down = []
        res = []
        for node_level in re:
            num = []
            for node in node_level:
                num.append(node.val)
            res.append(num)
        return res
```

2. ***BFS模板***

链接：https://www.nowcoder.com/questionTerminal/445c44d982d04483b04a54f298796288?answerType=1&f=discussion
来源：牛客网

所以BFS的模板为：

1. 如果不需要确定当前遍历到了哪一层，模板如下：

   ```c++
   void bfs() {
    vis[] = {0}; // or set
    queue<int> pq(start_val);
   
    while (!pq.empty()) {
        int cur = pq.front(); pq.pop();
        for (遍历cur所有的相邻节点nex) {
            if (nex节点有效 && vis[nex]==0){
                vis[nex] = 1;
                pq.push(nex)
            }
        } // end for
    } // end while
   }
   ```

上述是伪代码，不仅可用于二叉树，可针对所有用BFS解题。

1. 如果需要确定遍历到哪一层，模板如下；

   ```c++
   void bfs() {
    int level = 0;
    vis[] = {0}; // or set
    queue<int> pq(original_val);
    while (!pq.empty()) {
        int sz = pq.size();
   
        while (sz--) {
                int cur = pq.front(); pq.pop();
            for (遍历cur所有的相邻节点nex) {
                if (nex节点有效 && vis[nex] == 0) {
                    vis[nex] = 1;
                    pq.push(nex)
                }
            } // end for
        } // end inner while
        level++;
   
    } // end outer while
   }
   ```

python_version

下面我们看用队列实现的二叉树层次遍历：

```python

from collections import deque
def levelOrder(head):
    if not head:
        return None
    queue = deque()
    queue.append(head)
    while queue:
        node = queue.popleft()
        print(node.data)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
```
如何按层输出呢？很简单，按层遍历，下一层的结点归入另一个队列里即可，道理类似杨辉三角那道题。
```python

def levelOrder(root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        

        res = []
        queue = [root]
        
        while queue:
            next_level = []
            vals = []
            while queue:
                p = queue.pop(0)
                vals.append(p.val)
                if p.left:
                    next_level.append(p.left)
                if p.right:
                    next_level.append(p.right)
            res.append(vals)
            queue = next_level
        
        return res
```
————————————————
版权声明：本文为CSDN博主「xutiantian1412」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xutiantian1412/java/article/details/89392946

3. 变种（之字形打印）

   注意对行数的判断应该为该层读取完毕后

   ```python
   # -*- coding:utf-8 -*-
   # class TreeNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   class Solution:
       def Print(self, pRoot):
           # write code here
           if not pRoot:
               return []
           tmp = [pRoot]
           cnt = 1
           res = []
           while tmp:
               next_level = []  # 下一层节点
               row = []  # 当前层
               while tmp:
                   # 当前层的节点值
                   node = tmp.pop(0)
                   row.append(node.val)
                   # 将下一层节点入队
                   if node.left:
                       next_level.append(node.left)
                   if node.right:
                       next_level.append(node.right)
               if cnt % 2 == 0:
                   # 偶数行进行反序
                   row = row[::-1]
               res.append(row)
               tmp = next_level
               cnt +=1
           return res
   ```

### 是否为二叉搜索树的后序遍历

题目描述：输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

1. 考察点： BST, 后续遍历

2. 思路：（1）后序遍历按照左->右->根。数组的最后一个元素必为根节点，对其左右进行判断

   （2）然后数组可分为左右部分，再分别对这两个序列进行判断，进行递归

   （3）注意递归跳出的条件，由于分左右子树时，仅对小于root的元素判断，即左子树；跳出条件即为判断右边元素是否均大于根节点的值。

   ``` python
   class Solution:
       def VerifySequenceOfBST(self, sequence):
           if not sequence:
               return False
           elif len(sequence) == 1:
               return True
           # 找出左子树数组部分
           root = sequence[-1]
           for k in range(len(sequence)):
               if sequence[k] > root:
                   # 碰到第一个大于root的值，退出循环
                   break
           left = sequence[:k]
           right = sequence[k:len(sequence)-1]
           # 对右半部分进行判断
           for num in right:
               if num < root:
                   return False
           # 左右子树均存在的情况下进行递归，不存在为True
           leftIs = rightIs = True
           if len(left) > 0:
               leftIs = self.VerifySequenceOfBST(left)
           if len(right) > 0:
               rightIs = self.VerifySequenceOfBST(right)
           return leftIs and rightIs
   ```

3. 复杂度：O(N) O(N)

   > **注意点** ： 递归判断时，要先对左右子树是否会为空的情况进行考虑；因为基线条件中 为空返回false。

   **related：**从两种排序中重构二叉树

### 得到镜像二叉树

操作给定的二叉树，将其变换为源二叉树的镜像。
**输入描述**:

```
二叉树的镜像定义：源二叉树 
    	    8
    	   /  \
    	  6   10
    	 / \  / \
    	5  7 9 11
    	镜像二叉树
    	    8
    	   /  \
    	  10   6
    	 / \  / \
    	11 9 7  5
```

```python
class Solution:
    def Mirror(self, root):
        if not root:
            return
        root.left, root.right = root.right, root.left
        self.Mirror(root.left)
        self.Mirror(root.right)
```

### 对称的二叉树

题目描述：

1. 考察点：对于树的题目，在纸上画大点的树进行判断

2. 思路：

   <img src="C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200630163651949.png" alt="image-20200630163651949" style="zoom:67%;" />

   满足对称的条件：

   - L.val == R.val
   - L.left.val == R.right.val
   - L.right.val == R.left.val

```python
# -*- coding:utf-8 -*-
class Solution:
    def isSame(self, p1, p2):
        # 判断两个节点是否对称
        if not p1 and not p2: # 均为None时对称
            return True
        elif not p1 or not p2: # 有一个为空时不对称
            return False
        return p1.val == p2.val and self.isSame(p1.left, p2.right) and self.isSame(p1.right, p2.left)
    def isSymertic(self, pRoot):
        # write code here
        if not pRoot:
            return True
        return self.isSame(pRoot.left, pRoot.right)
```

3. 复杂度：O（N）， O（N）

### 二叉树中和为某一值的路径

题目描述：输入一颗二叉树的根节点和一个整数，按字典序打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回二维列表，内部每个列表表示找到的路径
    def FindPath(self, root, expectNumber):
        # write code here
        if not root:
            return []
        elif not root.left and not root.right and root.val == expectNumber:
            return [[root.val]]
        left_path = self.FindPath(root.left, expectNumber-root.val)
        right_path = self.FindPath(root.right, expectNumber-root.val)
        res = []    
        for path in left_path+right_path:
            res.append([root.val]+path) # []也考虑进去了
        return res
```

> 注意：由于返回的二维列表，插入时注意列表的连接可直接用+，如[1,2,3]+[1] = [1,2,3,1]

###  二叉树和双向链表

题目描述：

输入一棵二叉搜索树，将该二叉搜索树转换成一个**排序**的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

1. 考察点： BST，链表

2. 思路：

   思路1 ： 由于BST的中序遍历为升序的，则可以将其中序遍历后再进行节点指针指向。

   思路2： 递归；左子树不为空时，将左子树的最右节点指向root；root的右边指向右子树的最左节点

   ```python
   # -*- coding:utf-8 -*-
   # class TreeNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   class Solution:
       def __init__(self):
           self.tmp = []
    ## 思路1：O（N） O（N）       
       def Convert(self, pRootOfTree):
           # write code here
           if not pRootOfTree or (not pRootOfTree.left and not pRootOfTree.right):
               return pRootOfTree            
           self.inOder(pRootOfTree)
           for i in range(len(self.tmp)-1):
               self.tmp[i].right, self.tmp[i+1].left = self.tmp[i+1], self.tmp[i]
           self.tmp[-1].left, self.tmp[-2].right = self.tmp[-2], self.tmp[-1]
           return self.tmp[0]
           
       def inOder(self, root):
           if not root:
               return 
           self.inOder(root.left)
           self.tmp.append(root)
           self.inOder(root.right)
     ## 思路2： 递归
       def Convert(self, pRootOfTree):
           # write code here
           if not pRootOfTree or (not pRootOfTree.left and not pRootOfTree.right):
               return pRootOfTree            
           left_link = self.Convert(pRootOfTree.left)
           right_link = self.Convert(pRootOfTree.right)
           tmpL = left_link
           if left_link:
               while tmpL.right:
                   tmpL = tmpL.right
               tmpL.right = pRootOfTree
               pRootOfTree.left = tmpL
           tmpR = right_link
           if right_link:
               while tmpR.left:
                   tmpR = tmpR.left
               tmpR.left, pRootOfTree.right = pRootOfTree, tmpR
           while pRootOfTree.left:
               pRootOfTree = pRootOfTree.left
           return pRootOfTree
   ```

   > **递归注意点：**为啥第一次没写对呢？
   >
   > 我写的时候认为递归的返回时左右子树遍历的最左节点，而实际为根节点（pRootOfTree的左右节点）。
   >
   > 认清返回的时根节点，就很好写了。

### 二叉树的序列化

   题目描述：

   请实现两个函数，分别用来序列化和反序列化二叉树

   二叉树的序列化是指：把一棵二叉树按照某种遍历方式的结果以某种格式保存为字符串，从而使得内存中建立起来的二叉树可以持久保存。序列化可以基于先序、中序、后序、层序的二叉树遍历方式来进行修改，序列化的结果是一个字符串，序列化时通过 某种符号表示空节点（#），以 ！ 表示一个结点值的结束（value!）。

   二叉树的反序列化是指：根据某种遍历顺序得到的序列化字符串结果str，重构二叉树。

   

   例如，我们可以把一个只有根节点为1的二叉树序列化为"1,"，然后通过自己的函数来解析回这个二叉树

   1. 考察点：遍历及从遍历序列中构造二叉树; 难点在于反序列化

   2. 思路

      ```python
      # -*- coding:utf-8 -*-
      # class TreeNode:
      #     def __init__(self, x):
      #         self.val = x
      #         self.left = None
      #         self.right = None
      class Solution:
          flag = -1
          def Serialize(self, pRoot):
            	s = ''
              s = self.SerializeHelper(pRoot, s)
              return s
          def SerializeHelper(self, root, s):
              if not root:
                  return '#,'
              s += str(root.val) + ','
              left = self.SerializeHelper(root.left)
              right = self.SerializeHelper(root.right)
              s += left + right
              return s
          
          def Deserialize(self, s):
              self.flag += 1
              if not s:
                  return None
              l = s.split(',')
              if self.flag >= len(s): 
                  # 到字符串末尾，返回空节点
                  return None
              root = None
              if l[self.flag] != '#':  
                  # 非空节点
                  root = TreeNode(int(l[self.flag]))
                  # 此处仍为s是因为，self.flag指示元素索引，每次调用方法时都会加一
                  root.left = self.Deserialize(s)
                  root.right = self.Deserialize(s)
              return root
      ```

      

### 树的子结构

描述:输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

**注意：**理解题目的意思，做的时候子结构想着直到叶子节点，用中序遍历存储比较，就不大可能对

思路：

链接：https://www.nowcoder.com/questionTerminal/6e196c44c7004d15b1610b9afca8bd88?answerType=1&f=discussion
来源：牛客网

第一步：
根据题意可知，需要一个函数判断树A和树B是否有相同的结构。显然是个递归程序。可考察**递归程序3部曲**。

1. 递归函数的功能：判断2个数是否有相同的结构，如果相同，返回true，否则返回false 
2. 递归终止条件： 

- 如果树B为空，返回true，此时，不管树A是否为空，都为true 

- 否则，如果树B不为空，但是树A为空，返回false，此时B还没空但A空了，显然false 

  3.下一步递归参数： 

- 如果A的根节点和B的根节点不相等，直接返回false 

- 否则，相等，就继续判断A的左子树和B的左子树，A的右子树和B的右子树

```python
bool dfs(TreeNode *r1, TreeNode *r2) {
    if (!r2) return true;
    if (!r1) return false;
    return r1->val==r2->val && dfs(r1->left, r2->left) && dfs(r1->right, r2->right);
}
```

第二步：
有了上面那个函数，接下来就应该让树A的每个节点作为根节点来和B树进行比较。
遍历树A的每个节点，可用遍历算法。这里采用先序遍历。
先序遍历的模板：

```python
void preOrder(TreeNode *r) {
    if (!r) return;
    // process r
    preOrder(r->left);
    preOrder(r->right);
}
```

这里用个例子来展示上述的分析：

![图片说明](https://uploadfiles.nowcoder.com/images/20200418/284295_1587180465848_38101B54D314F9EEAD38BBC2533869AC) 

因此，结合两个函数。可得到整个函数的代码、

```
bool dfs(TreeNode *r1, TreeNode *r2) {
    if (!r2) return true;
    if (!r1) return false;
    return r1->val==r2->val && dfs(r1->left, r2->left) && dfs(r1->right, r2->right);
}

bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
{
    if (!pro1 || !pRoot2) return false;
    return dfs(pRoot1, pRoot2) || HasSubtree(pRoot1->left, pRoot2) ||
    HasSubtree(pRoot1->right, pRoot2);

}
```

时间复杂度：O（m），m为A树的节点数，n为B树的节点数。首先A树中的每个节点必须遍历一次，然后
A树中最多有(m/n)个与B树的根节点相等，然后(m/n)*n=m,所以时间复杂度为O（2*m）
> **技巧**
一般返回bool类型的递归程序，&& 与 || 的灵活使用，可以使代码非常简洁。

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def HasSubtree(self, pRoot1, pRoot2):
        # write code here
        if not pRoot1 or not pRoot2:
            return False
        return self.subtreeHelper(pRoot1, pRoot2) or self.HasSubtree(pRoot1.left, pRoot2) or self.HasSubtree(pRoot1.right, pRoot2)
        
    def subtreeHelper(self, A, B):
        # 判断AB是否有相同的结构
        if not B: return True
        elif not A or A.val != B.val:
            return False
        return self.subtreeHelper(A.left, B.left) and self.subtreeHelper(A.right, B.right)
    
```

***递归问题的模板***

- 递归函数功能
- 递归终止条件
- 下一步递归

### 平衡二叉树

题目描述：输入一棵二叉树，判断该二叉树是否是平衡二叉树。在这里，我们只需要考虑其平衡性，不需要考虑其是不是排序二叉树

1. 考察点： 平衡树，输的深度&高度

2. 一些定义和区分

   > 平衡树(Balance Tree，BT) 指的是，任意节点的子树的高度差都小于等于1。常见的符合平衡树的有，B树（多路平衡搜索树）、AVL树（二叉平衡搜索树）等。

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def IsBalanced_Solution(self, pRoot):
        if not pRoot:
            return True
        if abs(self.getHeight(pRoot.left) - self.getHeight(pRoot.right)) > 1:
            return False
        return self.IsBalanced_Solution(pRoot.left) and self.IsBalanced_Solution(pRoot.right)
    def getHeight(self, root):
        if not root:
            return 0
        left = self.getHeight(root.left)
        right = self.getHeight(root.right)
        return max(left+1, right+1)
```
改进： 然后对上述代码加以改造，如果不满足平衡二叉树的定义，则返回-1，并且如果左子树不满足条件了，直接返回-1，右子树也是如此，相当于剪枝，加速结束递归


```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def IsBalanced_Solution(self, pRoot):
        # write code here
        return self.depth(pRoot) != -1
    
    def depth(self, root):
        if not root:
            return 0
        left = self.depth(root.left)
        if left == -1: return -1
        right = self.depth(root.right)
        if right == -1: return -1
        if abs(left-right) > 1: return -1
        return max(left+1, right+1)
```



**TIPS:**二叉树的基本操作，深度，高度等
