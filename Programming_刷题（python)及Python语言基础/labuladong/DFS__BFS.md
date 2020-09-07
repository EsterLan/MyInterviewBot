**REFERENCE:https://mp.weixin.qq.com/s/WH_XGm1-w5882PnenymZ7g**

## DFS(回溯问题 )

实际上就是决策树的遍历问题。 TC:O(N!)  SC:T(logN)

### 框架

**三个重要问题：**

（1）路径：记录已经做过的选择

（2）选择列表：当前可以做的选择

（3）结束条件：遍历到决策树的底层，即选择列表为空。

实际上将backtrack看作一个指针，遍历地访问每个节点

```python
result = []
def backtrack(选择列表, 路径):
    if 满足结束条件:
        result.append(路径)
        return
    for 选择 in 选择列表:
        # 做选择
        选择 = 选择列表弹出当前选择
        路径.append(选择)
        backtrack(选择列表, 路径)
        # 撤销选择
        路径.pop(选择)
        选择列表.append(选择)
        
```

<img src="C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200731214933424.png" alt="image-20200731214933424" style="zoom:50%;" />

### 全排列问题 permutation

```python
class Solution:
    def __init__(self):
        self.res = []
        
    def permute(self, nums: List[int]) -> List[List[int]]:
        if not nums:
            return []
        track = []
        self.backtrack(nums, track)
        return self.res
    
    # 路径：track记录走过的节点
    # 选择列表： nums中没有track的部分
    # 结束条件： 选择列表为空即len(track) == len(nums)
    def backtrack(self, nums, track):
        if len(track) == len(nums):
            self.res.append([*track]) # 要将track重构传进去
            return
        for num in nums:
            if num in track: # 参数合法性
                continue
            # 做选择
            track.append(num)
            self.backtrack(nums, track) 
            # 撤销选择
            track.remove(num)
        
```

### 进一步：next permutation

与DFS无关，数学问题

```python
# 倒着遍历两边数组
# 第一遍，找到第一个left<right, i = nums.index(left)
# 第二遍，找到第一个num >left ，i = nums.index(num)
# swap nums[i] and nums[j]
# i之后仍为逆序，则nums[(i + 1):] = reversed(nums[(i+1):])
```



```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> List[List[int]]:
        for i in range(len(nums)-1, -1, -1):
            if nums[i-1] < nums[i]:
                break
        if i != 0:
            for j in range(len(nums)-1, -1, -1):
                if nums[j] > nums[i-1]:
                    nums[i-1], nums[j] = nums[j], nums[i-1]
                    break
        nums[i:] = reversed(nums[i:]) # 对后半部分逆序进行正序，不用sort()会产生多余空间       
        
```

mysolution：

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if len(nums) < 2:
            return
        # 倒着遍历两边数组
        # 第一遍，找到第一个left<right, i = nums.index(left)
        # 第二遍，找到第一个num >left ，i = nums.index(num)
        # swap nums[i] and nums[j]
        # i之后仍为逆序，则nums[(i + 1):] = reversed(nums[(i+1):])
        i = len(nums) - 1
        while i > 0:
            if nums[i - 1] < nums[i]:
                break
            i -= 1
        if i == 0:
            if nums[0] > nums[1]:
                # 若为逆序，则返回asc序列
                nums.sort()
                return
            if nums[0] < nums[1]:
                nums[0], nums[1] = nums[1], nums[0]
                nums[1:].sort()
                return
        j = len(nums) - 1
        while j > 0:
            if nums[j] > nums[i-1]:
                break
            j -= 1
        nums[i-1], nums[j] = nums[j] , nums[i-1]
        # i-1后半部分倒序
        nums[i:] = sorted(nums[i:])
        return
```

### 更进一步：有重复数字的全排列

思路: 用visited来表达选择列表， （1）后剪枝：生成所有路劲，去除重复路径（2）预剪枝： 在选择中，进行剪枝

```python
class Solution:
    def permuteUnique(self, nums):
        def backtrack(nums, track):
            if len(track) == len(nums) and track not in res: # 后剪枝，时长边长
                res.append(track[:])
                return
            for i in range(len(nums)):
                
                if not visited[i]:
                    # 做选择
                    track.append(nums[i])
                    visited[i] = True
                    backtrack(nums, track)
                    # 撤销选择
                    track.pop() # 注意此处采用pop(),确保删除最后一个元素
                    visited[i] = False
            
        if not nums:
            return []
        res = []
        track = []
        visited = [False for _ in range(len(nums))]
        backtrack(nums, track)
        return res
```

预剪枝版本： 注意一定要对nums进行排序再传入

```python
class Solution:
    def permuteUnique(self, nums):
        def backtrack(nums, track):
            if len(track) == len(nums):
                res.append(track[:])
                return
            for i in range(len(nums)):
                if i > 0 and nums[i] == nums[i-1] and not visited[i-1]:
                    # 预剪枝
                    continue
                if not visited[i]:
                    # 做选择
                    track.append(nums[i])
                    visited[i] = True
                    backtrack(nums, track)
                    # 撤销选择
                    track.pop() # 注意此处采用pop(),确保删除最后一个元素
                    visited[i] = False
        
        if not nums:
            return []
        res = []
        track = []
        visited = [False for _ in range(len(nums))]
        nums.sort()
        backtrack(nums, track)
        return res

```

## BFS

BFS实质上是解决最短距离的问题，从起始点到重点的最少步数

时间复杂度：O(nlogn)  空间复杂度高: 最坏情况存储最底层N/2节点O(N)

### 框架

```python
step = 0
def BFS(start, end):
    queue = [] # 核心
    visited = [] # 记录访问过的节点，防止回头路
    
    queue.append(start)
    visited.append(start)
    step = 0 # 记录扩散的步数
    while queue:
        next_level = [] # 下层
        while queue:
            # 扩散
            cur = queue.pop() 
            visited.append(cur)
            # 重点：判断是否为目标节点 #
            if cur == target:
                return step
            for node in cur.adj(): # 当前节点的周围节点
                if node not in visited:
           			# 不为目标节点，将当前节点的周围节点（未访问）入队
                    next_level.append(node)
        # 重点：更新步数 #
        step += 1
        queue = next_level
    return -1
```

### 二叉树的深度

### 解开密码锁的最少次数

### 之字形打印二叉树

```python
def Print(root):
    if not root:
        return []
    
    queue = [root]
    res = []
    cnt = 0 # 层数
    while queue:
        next_level = [] # 下一层
        row = [] # 当前层
        cnt += 1
        while queue:
            # 遍历当前层
            node = queue.pop(0)
            row.append(node.val)
            if node.left:
                next_level.append(node.left)
            if node.right:
                next_level.append(node.right)
        if cnt % 2 == 0:
            row = row[::-1]
        res.append(row)
        queue = next_level
    return res
    
    
```





