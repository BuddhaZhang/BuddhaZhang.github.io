---
layout:     post
title:      "2017新年计划"
subtitle:   "好好地规划一下未来"
date:       2017-01-03
author:     "Michael Zhang"
header-img: "img/post-bg-2015.jpg"
tags:
    - 计划
---


# 前言

总结与计划

# 正文

```java
public void startThreadUseRunnalbe() {
Thread thread = new Thread(new Runnable() {
public void run() {
System.out.println("start thread using runnable");
}
});
thread.start();
}
```

2016年已经过去，想想自己都做了什么事。

2017对自己的规划是：成为一名关于数据的有偏全栈工程师（从数据采集到数据分析，最后数据的展示）。

具体细则有：

一、机器学习方面

1.目标是掌握主流的机器学习算法的原理及设计实现，包括：<br>
（1）监督学习：回归模型、决策树、最大熵模型、贝叶斯分类、SVM<br>
（2）无监督学习：聚类<br> 
（3）增强学习：感知机模型、神经网络（深度学习） <br>
（4）其他：隐含马尔可夫模型、条件随机场、ADBoost。肯定还有一些是随着以后学习才了解其学习必要性的。

2.实现目标的方法是：<br>
（1）学习《机器学习实战》，周志华《机器学习》，李航《统计学习方法》等书籍<br>
（2）学习网上在线课程，包括coursea的华盛顿大学的Machine Learning专项课程（共6门小课），吴恩达的Stanford机器学习公开课。完成作业与测试。<br>
（3）对于这些主流的机器学习算法，考虑到完成时间和实现难度，对一些可以自己实现的算法自己编写代码实现一次（非练习不足以深刻理解，真正掌握） <br>
（4）学习使用常用的机器学习库，包括TensorFlow、Scikit-learn<br>
（5）最后利用淘宝和京东等网上商城的公开的商品信息，下载下来，构建一个基于图片的推荐系统

二、爬虫方面

1.目标是能够构建稳定迅速的分布式爬虫系统，能够满足数据采集的需要

2.实现目标的方法是：<br>
（1）继续学习分布式爬虫的教程<br>
（2）学习scrapy（因为它包含下载->解析出新的url->下载的循环）<br>
（3）暂时设想的分布式架构：master分配网页源码下载任务给slave -> slave下载，存到NoSQL数据库 -> master分配解析任务给slave -> slave解析，提取出待下载url，入库 ->回到第一步

三、大数据方面

1.基本了解再到能够使用Hadoop生态圈的一些东西，包括HDFS，HBase，Spark，Storm，能够编写MapReduce方法<br>
2.在自己的系统中使用这些分布式架构

四、数据分析

1.目标是能够使用Python做常用的数据分析工作<br>
2.学习Python数据分析的库，包括Numpy、Scipy、Pandas、Matplotlib、IPython。做一些实际的工作

五、网站方面

1.看完Django的官方文档
