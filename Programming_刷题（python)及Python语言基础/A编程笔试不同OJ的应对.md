# 编程笔试不同OJ的应对

https://blog.csdn.net/qq_36306833/article/details/104416578

## 牛客

1. 字符串

   单行

   ```python
   line = input()
   print(line)
   ```

   多行：

   ```python
   import sys
   if __name__ == '__main__':
       data = []
   while True:
       line = sys.stdin.readline().strip() # input()
       if not line:
           break
       data.append(line)
   print(" ".join(data)) # 空格将其分开
   ```

2. 数字

   ```python
   n = int(input())
   print(n) #输出数字
   ```

3. 单行输入输出数组

   ```python
   l = list(map(int, input().split(" ")))
   print(l)
   ```

   

4. 输出形式为矩阵

   ```python
   import sys
   if __name__ == '__main__':
       data = []
   while True:
       line = input()
       if not line:
           break
       tmp = list(map(int, line.split(' ')))
       data.append(tmp)
   print(data)
   ```

一个例子：赛码为例

[reference](https://blog.csdn.net/weixin_45069761/article/details/107524386?utm_medium=distribute.pc_relevant.none-task-blog-baidulandingword-1&spm=1001.2101.3001.4242)

**题目描述**

> 大学的同学来自全国各地，对于远离家乡步入陌生大学校园的大一新生来说，碰到老乡是多么激动的一件事，于是大家都热衷于问身边的同学是否与自己同乡，来自新疆的小赛尤其热衷。但是大家都不告诉小赛他们来自哪里，只是说与谁是不是同乡，从所给的信息中，你能告诉小赛有多少人确定是她的同乡吗？

**输入描述**

> 包含多组测试用例，对于每组测试用例：
> 第一行包括2个整数，**N**（1 <= N <= 1000），**M**(0 <= M <= N*(N-1)/2)，代表现有N个人（用1~N编号）和M组关系；
> 在接下来的M行里，每行包括3个整数，a，b, c，如果c为1，则代表a跟b是同乡；如果c为0，则代表a跟b不是同乡；
> 已知1表示小赛本人。

**输出描述**

> 对于每组测试实例，输出一个整数，代表确定是小赛同乡的人数

**输入样例**

> 3 1
> 2 3 1
> 5 4
> 1 2 1
> 3 4 0
> 2 5 1
> 3 2 1

**样例输出**

> 0
> 3

**具体代码**

```python
def run(res):
    visited = set()
    visited.add(1)
    res.sort()
    # 将与1有关系的，即与小赛是老乡的，则记录在集合里，以免重复记录
    for i in res:
        if i[0] in visited:
            visited.add(i[1])
    # 集合的长度减去小赛本人则为他的老乡的人数
    return (len(visited) - 1)
def main():
    # while True的作用是连续输入多个测试样例，若题目没要求连续输入多个测试样例，则不需要使用while True
    while True:
        res = []
        N, M = map(int, input().split())
        for i in range(M):
            a, b, c = map(int, input().split())
            # a, b若为老乡，则记录下来
            if c == 1:
                res.append([a,b] if a < b else [b,a])
        result = run(res)
        print(result)
    return 
main()
123456789101112131415161718192021222324
```

**输入输出不同点**

> 1.这里的输入为自己键盘敲上去，当然你在本地测试需要自己敲，在网站上测试则不需要自己敲，
> **python3为input()，python2为raw_input()**

> 2.**若一行要求输入一个数字时，直接使用int(input())即可**
> **若一行要求输入多个数字时，使用map(int, input().split())**，分割符用split()表示，默认为所有的空字符，包括空格、换行(\n)、制表符(\t)等，若分割符为逗号，则为**split(’,’)**

> 3.若题目要求输入的是列表，则可以使用先定义个空列表，然后利用append将数据输入进去

> 4.**输出则必须使用print函数将答案输出出来**，直接写return ,则不会返回答案

> 5.**若自己定义函数，则最后切勿忘记调用该函数**，
> 否则该函数不会被调用

> 6.若输出为列表，要求返回列表里的具体值，
> 则可以使用for循环将答案全部输出出来