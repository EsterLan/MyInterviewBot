## mysql_锁机制

[MySQL中锁详解（行锁、表锁、页锁、悲观锁、乐观锁等）](https://blog.csdn.net/tanga842428/article/details/52748531?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param)

### 锁机制概述

锁是计算机协调<u>多个线程或进程</u>**并发**访问某一资源的机制。

在数据库中，除传统的计算资源(如CPU、ram、I/O等)的争用外，数据也是一种供许多用户共享资源，如何保证**数据的并发访问的一致性、有效性是**所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度说，锁对数据库显得尤其重要，也更加复杂。

![image-20200726085511365](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726085511365.png)

### 锁分类

#### 表锁：

偏向MyISAM引擎、开销小、加锁快；无死锁；锁定粒度大，锁冲突概率高，并发性低。

![image-20200726100227753](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726100227753.png)

![image-20200726100248164](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726100248164.png)

![image-20200726100304536](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726100304536.png)

#### 行锁

：InnoDB引擎、开销大、加锁慢；死锁；锁定粒度小，锁冲突概率低，并发性高；

InnoDB相较于MyISAM：1. 事务；2.默认行锁

事务：![image-20200726100722180](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726100722180.png)

![image-20200726100740402](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726100740402.png)

![image-20200726100827218](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726100827218.png)

![image-20200726100856872](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726100856872.png)

间隙锁

![image-20200726100801035](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726100801035.png)

#### 页锁

![image-20200726100910984](C:\Users\Ester.L\AppData\Roaming\Typora\typora-user-images\image-20200726100910984.png)