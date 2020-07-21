## DFS/BFS

### 矩阵中路径是否存在

题目描述：

考察点：DFS 回溯

思路：

```python
# -*- coding:utf-8 -*-
class Solution:
    def hasPath(self, matrix, rows, cols, path):
        # write code here
        if not path or not matrix:
            return False
        #global flag
        flag = [0 for _ in range(len(matrix))] # 是否访问
        for i in range(rows):
            for j in range (cols):
                #if matrix[i*cols+j]==path[0]:
                if self.findpath(i,j,path,list(matrix),rows,cols, 0, flag):
                    return True
        return False
    
    def findpath(self,i,j,path,matrix,rows,cols, k, flag):
        if i<0 or j<0 or i>=rows or j >= cols:
            return False
        index = i * cols + j # 注意此句应该乘上 cols列数因为每扫过一行，增加cols个元素
        if matrix[index] != path[k] or flag[index] == 1:
            return False
        if k == len(path) - 1:
            return True
        flag[index] = 1
        if (self.findpath(i+1, j, path, matrix, rows, cols, k+1, flag) or
            self.findpath(i-1, j, path, matrix, rows, cols, k+1, flag) or
            self.findpath(i, j+1, path, matrix, rows, cols, k+1, flag) or
            self.findpath(i, j-1, path, matrix, rows, cols, k+1, flag)):
            return True
        flag[index] = 0
        return False

```

DFS解决问题模板：

```python
链接：https://www.nowcoder.com/questionTerminal/c61c6999eecb4b8f88a98f66b273a3cc?answerType=1&f=discussion
来源：牛客网

dfs(){

    // 第一步，检查下标是否满足条件

    // 第二步：检查是否被访问过，或者是否满足当前匹配条件

    // 第三步：检查是否满足返回结果条件

    // 第四步：都没有返回，说明应该进行下一步递归
    // 标记
    dfs(下一次)
    // 回溯
}  


main() {
    for (对所有可能情况) {
        dfs()
    }
}
```



### BFS

```python
graph = {
    "A": ["B", "C"],
    "B": ["A", "C", "D"],
    "C": ["A", "B", "D", "E"],
	"E": ["C", "D"],
    "F": ["D"]
}
```

以下代码的问题在哪儿呢？

```python
def BFS(graph, start):
    queue = []
    queue.append(start)
    res = []
    while len(queue) > 0:
        node = queue.pop(0)
        res.append(node)
        while len(graph[node]) > 0:
            queue.append(graph[node].pop(0))
# 有问题， node的邻接矩阵会有重复  
# 树可以这么写，因为不存在重复
```

```python
def BFS(graph, start):
    queue = []
    queue.append(start)
    res = []
    seen = set() # 保存访问过的节点,python用hash表，比list快
    seen.add(start)
    while (len(queue) > 0):
        vertex = queue.pop(0)
        #res.append(vertex)
        nodes = graph[vertex]
        for w in nodes:
           # if w is NOT SEEN:
        	if w not in seen:
                queue.append(w)
                seen.add(w)
         print(vertex)
```

BFS拓展：通过BFS打印最短路径

### <img src="C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200714222326647.png" alt="image-20200714222326647" style="zoom:67%;" />

映射通过字典实现

```python
def BFS(graph, start):
    queue = []
    queue.append(start)
    res = []
    seen = set() # 保存访问过的节点,python用hash表，比list快
    seen.add(start)
    parent = {start: None} # 映射：当前点：前一个节点
    while (len(queue) > 0):
        vertex = queue.pop(0)
        #res.append(vertex)
        nodes = graph[vertex]
        for w in nodes:
           # if w is NOT SEEN:
        	if w not in seen:
                queue.append(w)
                seen.add(w)
                parent[w] = vertex
         print(vertex)
      return parent
```

```python
parent = BFS(graph, 'E')
# 从E到B的最短路径
v = 'B'
while v != None:
    print(v)
    v = parent(v)

```



### DFS

将BFS中的队列改为栈

```python
def DFS(graph, start):
    stack = []
    stack.append(start)
    seen = set{}
    set.append(start)
    while stack:
        vertex = stack.pop()
        nodes = graph[vertex]
        for w in nodes:
            if w not in seen:
                stack.append(w)
                seen.add(w)
        print(vertex)
        
```



