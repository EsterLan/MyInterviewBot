## Fintech项目注意点

### 项目简介

#### 背景

提供**用户标签数据**、过去60天的**交易行为数据**、过去30天的**APP行为数据，**通过有效特征提取，构建信用违约预测模型，输出评分数据集中每个用户的违约概率。

#### 评价指标

评价指标是AUC：aera under curve；ROC曲线下的面积
<figure class="half">
    <img src="C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200722165411542.png" alt="image-20200722165411542" style="zoom:50%;" />
    <img src="C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200722165953130.png" alt="image-20200722165953130" style="zoom:25%;" />
</figure>
$FPR = \frac{FP}{FP+TN}$

$$TPR = \frac{TP}{TP+FN}$$

模型结果越靠近左上角即TPR->1, FPR->0越好，尽量提高1样本的分类，对于本个题目来说就是要尽可能地去找信用不好的客户。

more refer to [分类及回归指标问题指标](分类问题及回归问题的模型评价指标.md)

### 数据预处理

删除缺失值较多的两列特征；类别特征OneHot编码

more refer to:[数据预处理](数据预处理.md)

### 特征构造

在用户交易行为表中提取有效特征

[特征构造]()





### 模型调参

XGB如何调参

![preview](https://pic2.zhimg.com/v2-3fa0a24855370044cb7a9fd7c0626a7e_r.jpg?source=1940ef5c)

### 上分关键

融合一定会起作用吗？有哪几种融合模型的方法

more refer to：[模型融合的方法]()



### 比赛开源中较好的模型

