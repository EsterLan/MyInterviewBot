SQL执行顺序：从From开始执行

7种JOIN：

```SQL
SELECT * 
FROM TABLE A 
INNER JOIN TABLE B
ON A.key = B.key
```



```SQL
SELECT * 
FROM TABLE A
LEFT JOIN TABLE B
ON A.key = B.key
WHERE B.key is NULL
```



```SQL
SELECT * 
FROM TABLE A
FULL OUTER JOIN TABLE B
ON A.key = B.key
WHERE A.key is NULL
OR B.key is NULL
```

# 索引

## 索引简介

[**MySQL 索引的原理与应用：索引类型，存储结构与锁**](https://juejin.im/post/5cf3d550f265da1b76388a34)

### 索引是什么：

- 索引（Index）是帮助MySQL高效获得数据的**数据结构**
- 索引的目的在于提高查询效率，类比字典。
- 

**"*排好序*的*快速查找*数据结构"**，两大功能：

（1）排序 -- ORDER BY（2）查找--where

索引会影响WHERE后面的查找和ORDER BY 的排序

- 除了数据外，数据库还维护这满足特定算法的数据结构（索引），这种数据结构以某种方式指向数据。这样可以在这些数据结构的基础上实现查找算法。这种数据结构就是索引。

  B+树

  

  

  why 查询快，增删慢：INSERT UPDATE DELETE因为增删除了对数据进行操作外，还是需要对索引进行维护；
  
  ![image-20200726103428832](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726103428832.png)

### 索引的优势和劣势

![image-20200726102021011](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726102021011.png)

![image-20200726102034673](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726102034673.png)

### 索引分类和建立索引

![image-20200726103258447](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726103258447.png)

![image-20200726103329629](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726103329629.png)

### 索引结构和检索原理

[全文索引、**b-tree、Hash**、r-tree](https://blog.csdn.net/ZYC88888/article/details/81701712)

[Hash索引和B+树索引的区别](https://blog.csdn.net/Sophia_0331/article/details/105288057)

![微信图片_20200726102902](D:\E01Software\Typora\figs\微信图片_20200726102902.png)

![微信图片_20200726102910](D:\E01Software\Typora\figs\微信图片_20200726102910.png)

Hash索引与B+树索引的区别

由于Hash索引结构和B+ 树不同，因此在索引使用上也会有差别：

（1）Hash索引不能进行范围查询，而B+树可以。
这是因为Hash索引指向的数据是无序的，而B+ 树的叶子节点是个有序的链表。

（2）Hash索引不支持联合索引的最左侧原则（即联合索引的部分索引无法使用），而B+树可以。
对于联合索引来说，Hash索引在计算Hash值的时候是将索引键合并后再一起计算Hash值，所以不会针对每个索引单独计算Hash值。因此如果用到联合索引的一个或多个索引时，联合索引无法被利用。

（3）Hash索引不支持Order BY排序，而B+树支持。
因为Hash索引指向的数据是无序的，因此无法起到排序优化的作用，而B+树索引数据是有序的，可以起到对该字段Order By 排序优化的作用。

（4）Hash索引无法进行模糊查询。而B+ 树使用 LIKE 进行模糊查询的时候，LIKE后面前模糊查询（比如%开头）的话可以起到优化的作用。

（5）Hash索引在等值查询上比B+树效率更高。

### 哪些情况下需要/不要建立索引

分组的前提是排序的

## 性能分析

### MySql Query Optimizer

### MySql常见瓶颈

### Explain

https://blog.csdn.net/Coding13/article/details/79131401?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase

```sql
declare @i varchar(max)  --声明全局变量
set @i = 1
while @i < 1000001 -- 执行1000000次insert--
begin
insert INTO
表名(id, name, times) values('12345678'+@i, '小明', '12345')
set @i=@i+1
end

```





![image-20200620202102919](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200620202102919.png)

![image-20200620202437897](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200620202437897.png)

建立表的范式？

> 第一范式：原子性，每个字段应该清晰的定义
>
> 第二范式： 唯一性，非主键属性唯一依赖主键。
>
> 第三范式：每个非主键属性都不易依赖于其他非主键属性

SELECT 子句顺序

| 子句     | 说明                 | 是否必须使用             |
| -------- | -------------------- | ------------------------ |
| SELECT   | 要返回的列或者表达式 | 是                       |
| FROM     | 从中检索数据的表     | 仅在从表中选择数据时使用 |
| WHERE    | 行级过滤             | 否                       |
| GROUP BY | 分组说明             | 仅在按组计算聚集时使用   |
| HAVING   | 组级过滤             | 否                       |
| ORDER BY | 要输出排序顺序       | 否                       |
| LIMIT    | 要检索的行数         | 否                       |

