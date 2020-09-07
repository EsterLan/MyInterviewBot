数组遍历框架：典型线性迭代

```python
def traverse(arr):
    for i in range(len(arr)):
        # 迭代访问arr[i]
```

链表遍历框架：线性&非线性

```python
# 单链表节点
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None
# 线性遍历+访问：for/while 非线性：递归
def traverse(head):
    p = head
    while p:
        # 迭代访问p.val
        p = p.next

def traverse(head):
    # 递归访问
    traverse(head.next)
```

二叉树遍历框架：典型递归

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
def traverse(root):
    # preorder
    traverse(root.left)
    # inorder
    traverse(root.right)
    # afterOrder
```

```python
## 扩展为N叉树遍历
class TreeNode:
    def __initz__(self, x):
        self.val = x
        self.children = []
 def traverse(root):
    for child in root.children:
        traverse(child)
```

