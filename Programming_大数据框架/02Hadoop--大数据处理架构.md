##  Hadoop--大数据处理架构

### Hadoop发展历程

#### 简介

Apache软件基金会下开源软件；Java语言开发；跨平台应用。

Hadoop：一整套大数据技术的统称。两大核心技术：HDFS+MapReduce（存储+处理）

03GFS->04HDFS 05MapReduce->06开源实现

Hadoop:2008排序

#### 特性

可靠性；高速性；高可扩展性；容错性；成本低（集群）；linux平台；跨平台，支持多种语言

#### 应用现状

Facebook 移动等日至Hadoop集群处理。

<img src="/home/esterl/.config/Typora/typora-user-images/image-20200730095120862.png" alt="image-20200730095120862" style="zoom:67%;" />



### Hadoop生态系统组件

#### 不同版本Apache Hadoop

<img src="/home/esterl/.config/Typora/typora-user-images/image-20200730095536212.png" alt="image-20200730095536212" style="zoom:67%;" />

> MapReduce到Hadoop2.0，是YARN框架支持的第一个框架;
>
> YARN还支持：Spark：内存计算；Storm：流计算。
>
> HDFS：可扩展性不好—>NN Federation HA

还有其他企业版本：Hortoworks, clodera（CDH），MapR，星环(国内)

如何选择：1.开源2.稳定3.是否实践检验4.社区支持

#### Hadoop项目结构

![image-20200730100426060](/home/esterl/.config/Typora/typora-user-images/image-20200730100426060.png)

YARN：内存、CPU等；Tez：构成有向无环图。Spark：及与内存；mapreduce基于磁盘；Hive：**批数据处理**，Hadoop平台数据仓库功能，用于企业决策分析，SQL语句->MapReduce作业；Pig：**流数据处理**PigLatin，轻量分析；Zookeeper：一致性协调，集群管理，分布式锁；Oozie：工作流调度；Hbase：实时；Flume:实时日志搜集；Sqoop：Hadoop平台与关系型数据库之间转换,传统数据库<->HDFS, HBASE,HIVE;Ambari：快速部署工具

### Hadoop安装&使用方法



### Hadoop集群部署和使用方法

