# Mahout 简介

## 一、实验介绍

### 1.1 实验内容

> - Mahout 简介

- Mahout 历史
- Mahout 特性
- Mahout 应用场景

### 1.2 实验知识点

- 了解 Mahout 的历史，特性，应用场景

### 1.3 实验环境

- hadoop1.2.1
- Mahout0.9
- Xfce 终端

### 1.4 适合人群

本课程难度为一般，属于初级级别课程，适合具有 linux 基础的用户。

## 二、Mahout

### 2.1 Mahout 简介

Apache Mahout 起源于 2008 年，它是 Apache Software Foundation (ASF) 开发的一个全新的开源项目，其主要目标是创建一些可伸缩的 `机器学习算法`，供开发人员在 Apache 许可下免费使用。Mahout 包含许多实现，包括集群、分类、CP 和进化程序。此外，通过使用 Apache Hadoop 库，Mahout 可以有效地扩展到云中。

### 2.2 Mahout 历史

Mahout 项目是由 Apache Lucene（开源搜索）社区中对机器学习感兴趣的一些成员发起的，他们希望建立一个可靠、文档翔实、可伸缩的项目，在其中实现一些常见的用于集群和分类的机器学习算法。该社区最初基于 Ng et al. 的文章 “Map-Reduce for Machine Learning on Multicore”, 但此后在发展中又并入了更多广泛的机器学习方法。Mahout 的目标还包括：

- 建立一个用户和贡献者社区，使代码不必依赖于特定贡献者的参与或任何特定公司和大学的资金。

- 专注于实际用例，这与高新技术研究及未经验证的技巧相反。

- 提供高质量文章和示例。

### 2.3 Mahout 特性

虽然在开源领域中相对较为年轻，但 Mahout 已经提供了大量功能，特别是在集群和 CF（Collaborative Filtering, 协作筛选） 方面。Mahout 的主要特性包括：

- Taste CF. Taste 是 Sean Owen 在 SourceForge 上发起的一个针对 CF 的开源项目，并在 2008 年被赠予 Mahout.

- 一些支持 Map-Reduce 的集群实现包括 k-Means、模糊 k-Means、Canopy、Dirichlet 和 Mean-Shift.

- Distributed Naive Bayes 和 Complementary Naive Bayes 分类实现。

- 针对进化编程的分布式适用性功能。

- Matrix 和矢量库。

- 上述算法的示例。

### 2.4 Mahout 应用场景

Mahout 为推荐引擎提供了一些可扩展的机器学习领域的经典算法实现,
可以使开发人员更为快捷的创建智能应用程序。

例如最经典的就是各种推荐系统，像购物网站上的相关物品推荐，可以根据用户的浏览记录或者购物记录来推荐用户可能感兴趣的商品：

![图片描述信息](https://doc.shiyanlou.com/userid46108labid785time1427765633901/wm)

## 三、实验总结

> 本次实验是对 Mahout 的简介。介绍了什么是 Mahout, Mahout 的特性和使用场景等等。

## 四、参考文档

> - 《Hadoop 实战 第 2 版》陆嘉恒，机械工业出版社；
> - [Apache Mahout 简介](http://www.ibm.com/developerworks/cn/java/j-mahout/)
