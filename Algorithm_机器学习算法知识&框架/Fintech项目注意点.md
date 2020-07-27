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

[特征构造](特征工程：feature extraction & feature creation.md)





### 模型调参

![preview](https://pic2.zhimg.com/v2-3fa0a24855370044cb7a9fd7c0626a7e_r.jpg?source=1940ef5c)

[各类模型的优缺点和适用场景](各类模型的优缺点和适用场景.md)

#### XGB优点

XGBoost算法可以给预测模型带来能力的提升。当我对它的表现有更多了解的时候，当我对它的高准确率背后的原理有更多了解的时候，我发现它具有很多优势：

##### 1、正则化

- 标准GBM的实现没有像XGBoost这样的正则化步骤。正则化对减少过拟合也是有帮助的。
- 实际上，XGBoost以“正则化提升(regularized boosting)”技术而闻名。

##### 2、并行处理

- XGBoost可以实现并行处理，相比GBM有了速度的飞跃。
- 不过，众所周知，Boosting算法是顺序处理的，它怎么可能并行呢？每一课树的构造都依赖于前一棵树，那具体是什么让我们能用多核处理器去构造一个树呢？我希望你理解了这句话的意思。如果你希望了解更多，点击这个链接。
- XGBoost 也支持Hadoop实现。

##### 3、高度的灵活性

- XGBoost 允许用户定义**自定义优化目标和评价标准**
- 它对模型增加了一个全新的维度，所以我们的处理不会受到任何限制。

##### 4、缺失值处理

- XGBoost内置处理缺失值的规则。
- 用户需要提供一个和其它样本不同的值，然后把它作为一个参数传进去，以此来作为缺失值的取值。XGBoost在不同节点遇到缺失值时采用不同的处理方法，并且会学习未来遇到缺失值时的处理方法。

##### 5、剪枝

- 当分裂时遇到一个负损失时，GBM会停止分裂。因此GBM实际上是一个**贪心算法**。
- XGBoost会一直分裂到指定的最大深度(max_depth)，然后回过头来剪枝。如果某个节点之后不再有正值，它会去除这个分裂。
- 这种做法的优点，当一个负损失（如-2）后面有个正损失（如+10）的时候，就显现出来了。GBM会在-2处停下来，因为它遇到了一个负值。但是XGBoost会继续分裂，然后发现这两个分裂综合起来会得到+8，因此会保留这两个分裂。

##### 6、内置交叉验证

- XGBoost允许在每一轮boosting迭代中使用交叉验证。因此，可以方便地获得最优boosting迭代次数。
- 而GBM使用网格搜索，只能检测有限个值。

##### 7、在已有的模型基础上继续

- XGBoost可以在上一轮的结果上继续训练。这个特性在某些特定的应用上是一个巨大的优势。
- sklearn中的GBM的实现也有这个功能，两种算法在这一点上是一致的。

相信你已经对XGBoost强大的功能有了点概念。注意这是我自己总结出来的几点，你如果有更多的想法，尽管在下面评论指出，我会更新这个列表的！ 
你的胃口被我吊起来了吗？棒棒哒！如果你想更深入了解相关信息，可以参考下面这些文章： 
XGBoost Guide - Introduce to Boosted Trees 
Words from the Auther of XGBoost [Viedo]



#### XGB如何调参：以下（1）（2）按顺序调参

[XGB调优完全指南](https://www.cnblogs.com/haobang008/p/5909207.html)

[自定义mae xgb调参](https://www.cnblogs.com/nxf-rabbit75/p/10595551.html)

![xgb参数](D:\A02UESTCGraduate05\找个好工作\MyInterviewBot\Algorithm_机器学习算法知识&框架\xgb参数.png)

将其参数分为3类：

（1）通用参数：

- **booster**, silent, nthreads；默认gbtree，有gbtree(基于树的模型)，gblinear（线性模型）

(2)booster参数：

- **max_depth, min_child_weight;** min_child_weight:决定最小叶子节点样本权重和，该参数用于避免过拟合。较大时可避免模型学习到局部的特殊样本；过高会导致欠拟合，用CV来处理
- **gamma;**该参数决定节点是否继续分裂，只有节点分裂后的下降值大于gamma，才会继续分裂。gamma越大，算法约保守，与损失函数息息相关
-  **subsample, colsample_bytree;** subsample控制每棵树随机采样的比例，减小该值，算法更保守，避免过拟合；过小会欠拟合；0.5-1；colsample_bytree控制每棵随机采样的列数的占比（每列是特征）0.5-1
- **alpha, lambda;**正则项参数 
- **eta**；学习速率
- scale_pos_weight; 样本类别不均衡时，设置为正值使算法更快收敛。

(3)学习目标参数：

- Objective[默认reg:linear]；binary：logistics；multi: softmax; multi:softprob
- eval_metric:![eval_metric](D:\A02UESTCGraduate05\找个好工作\MyInterviewBot\Algorithm_机器学习算法知识&框架\eval_metric.png)
- seed:随机数种子

### 上分关键

如何构造的特征：



融合一定会起作用吗？有哪几种融合模型的方法

more refer to：[模型融合的方法]()



### 比赛开源中较好的模型

