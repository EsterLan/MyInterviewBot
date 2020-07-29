## 推荐系统实践chap2：用户行为数据

### 用户行为数据简介

- 日志：在网站上最简单的存在形式。如在电子商务网站中这些行为主要包括网页浏览、购买、点击、评分和评论等。存储在分布式数据仓库中。

在个性化推荐系统中一般分为**显性反馈行为**（评分、评论等）和**隐性反馈行为**（浏览行为）

![image-20200729194507314](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200729194507314.png)

![image-20200729194622260](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200729194622260.png)

四个代表性数据集

### 用户行为分析

互联网很多数据分布满足**长尾分布**，power law：

​							$$f(x) = \alpha x^{k}$$

**仅仅基于用户行为数据设计的推荐算法一般称为协同过滤算法**。进一步有基于邻域的方法、隐语义模型、基于图的随机游走模型等。业界最广泛应用的是基于邻域的方法，其主要包含：

- 基于用户的协同过滤算法：给用户推荐和他兴趣相似的*其他用户*喜欢的物品
- 基于物品的协同过滤方法：给用户推荐他*之前喜欢的物品相似*的物品。

### 数据集和评价指标

### 基于邻域的算法

#### 基于用户的协同过滤UserCF--标志推荐系统的诞生

直到2000年都是最著名算法

社会性，适用于新闻等

![image-20200729195539938](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200729195539938.png)

改进：用户兴趣相似度

#### 基于物品的协同过滤的算法ItemCF

**Why itemCF**？随着用户数的增多，用户兴趣相似度矩阵运算的时间复杂度和空间复杂度平方增长；此外，可解释性不强



**注意：**并不利用物品内容属性计算物品之间相似度，而是通过分析用户行为记录计算物品之间的相似度。eg：**物品A和物品B具有很大的相似度是因为喜欢物品A的用户大都喜欢物品B**，而不是物品A的内容和物品B相似。

![image-20200729200440260](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200729200440260.png)

![image-20200729200741293](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200729200741293.png)

为什么ItemCF的覆盖率和新颖度都不高：哈利波特问题。

- 由于哈利波特太热门了，ItemCF计算出的图书相关表存在一个问题，很多书都和《哈利波特》有关。->加大惩罚权重
- 另一个问题：两个不同领域的最热物品之间往往具有较高的相似度。仅靠用户行为数据不能解决该问题。引入物品的内容数据来解决，beyond ItemCF

### 隐语义模型Latent Factor Model

![image-20200729201357940](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200729201357940.png)

LDA, matrix factorization，





### 基于图的模型

