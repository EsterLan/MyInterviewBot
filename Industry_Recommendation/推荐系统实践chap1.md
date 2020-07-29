## 推荐系统实践chap1

### 什么是推荐系统

推荐系统的基本任务是**联系用户和物品**，解决信息过载问题。

与搜索引擎存在互补关系：***搜索引擎***需要用户主动提供**准确的关键词**寻找信息；而***推荐系统***不需要用户提供明确需求而是通过**用户的历史行为**给用户的兴趣建模，从而给用户推荐满足其兴趣和需求的信息

![image-20200728211832591](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200728211832591.png)

### 推荐系统应用

- 电子商务
- 社交网络
- 电影和视频网站
- 个性化音乐网络电台
- 个性化阅读
- 个性化广告

### 推荐系统测评

![image-20200728212150705](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200728212150705.png)

#### 推荐系统试验方法：

离线实验、用户调查、在线实验

##### offline experiment

![image-20200728212528105](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200728212528105.png)

![image-20200728212551579](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200728212551579.png)

##### user study

上线测试前需要做用户调查，需要真实用户在测试的推荐系统上完成一些任务，他们完成任务时观察记录其行为。

![image-20200728212848327](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200728212848327.png)

##### online experiment

完成离线实验和必要的用户调查后，将推荐系统上线做ABtest

![image-20200728213040836](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200728213040836.png)

![image-20200728213410060](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200728213410060.png)

流量分配：使得不同层之间的流量正交？

> Summary:一个新的推荐算法的最终上线需要
>
> - 离线实验：证明该算法在指标上优于现有的算法
> - 用户调查确定其用户满意度不低于现有算法
> - 在线AB测试确定它在我们关心的指标上优于现有算法

#### 评价指标

- **用户满意度**：问卷调查；购买度；视频网站和豆瓣对推荐结果是否满意的反馈；点击率、用户停留时间和转换率等。

- **预测准确度**：度量一个推荐算法**预测用户行为**的能力

  （1）评分预测：rmse

  （2）TopN推荐：precision/recall

- **覆盖率**：描述一个推荐系统**对物品长尾的发掘**能力。

- **多样性**：描述了推荐列表中**物品两两之间的不相似性**

- 新颖性

- 惊喜度

- 信任度：用户越信任推荐系统，月趋向于和它交互

- **实时性**：实时更新推荐列表**满足用户新的行为变化**（eg：买iphone推荐配件）；**冷启动问题**，推荐系统需要能够加入新的推荐物品给用户。

- 健壮性

#### 评测维度

![image-20200728214604200](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200728214604200.png)