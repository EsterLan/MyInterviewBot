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

### 索引是什么：

- 索引（Index）是帮助MySQL高效获得数据的**数据结构**

- 索引的目的在于提高查询效率，类比字典。

**"*排好序*的*快速查找*数据结构"**，两大功能：

（1）排序 -- ORDER BY（2）查找--where

索引会影响WHERE后面的查找和ORDER BY 的排序

- 除了数据外，数据库还维护这满足特定算法的数据结构（索引），这种数据结构以某种方式指向数据。这样可以在这些数据结构的基础上实现查找算法。这种数据结构就是索引。

  B+树

  

  

  why 查询快，增删慢：INSERT UPDATE DELETE因为增删除了对数据进行操作外，还是需要对索引进行维护；

### 索引的优势和劣势

### 索引分类和建立索引

### 索引结构和检索原理

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

