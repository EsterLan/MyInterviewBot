## LeetCode03

### LC binary-tree-preoder-traversal-easy

二叉树前序遍历

```python
class Solution:
    def preoderTraversal(self, root):
        if not root:
            return []
        stack = [root]
        res = []
        while stack:
            node = stack.pop(-1)
            res.append(node.val)
            if node.right: # 右节点先入栈后出栈
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        return res     
```

more : refer to 二叉树基本操作

