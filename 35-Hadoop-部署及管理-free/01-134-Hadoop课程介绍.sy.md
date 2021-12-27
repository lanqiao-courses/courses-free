---
show: step
version: 1.0
enable_checker: false
---

# Hadoop 课程介绍

## 一、实验介绍

#### 1.1 实验知识点

- Hadoop 简介
- Hadoop 历史
- Hadoop 相关项目
- Hadoop 应用场景

#### 1.2 实验内容

- 了解 Hadoop 的概念
- 了解 Hadoop 的相关项目和使用场景

#### 1.3 实验环境

- Hadoop2.6.0
- Xfce 终端

#### 1.4 适合人群

本课程难度为一般，属于初级级别课程，适合具有 Linux 基础的用户。

## 二、Hadoop

在本实验中，我们将一同了解什么是 Hadoop，它有哪些相关的项目以及它的使用场景。

<center><img src="https://doc.shiyanlou.com/courses/uid977658-20190923-1569206111311" style="width:300px"/></center>

本部分以理论为主，如果对 Hadoop 已有一定的了解，可以跳过此实验，进入下一个实验。

### 2.1 Hadoop 简介

- 开源

Apache Hadoop 是一款支持数据密集型分布式应用并以 Apache 2.0 许可协议发布的开源软件框架。它支持在商品硬件构建的大型集群上运行的应用程序。

- 源于 MapReduce

Hadoop 是根据 Google 公司发表的 MapReduce 和 Google 档案系统的论文自行实作而成。

- 提供可靠性和数据移动

Hadoop 框架透明地为应用提供可靠性和数据移动。它实现了名为 MapReduce 的编程范式：应用程序被分割成许多小部分，而每个部分都能在集群中的任意节点上执行或重新执行。

- 分布式文件系统

此外，Hadoop 还提供了分布式文件系统，用以存储所有计算节点的数据，这为整个集群带来了非常高的带宽。MapReduce 和分布式文件系统的设计，使得整个框架能够自动处理节点故障。它使应用程序与成千上万的独立计算的电脑和 PB 级的数据。

- Hadoop 平台

现在普遍认为整个 Apache Hadoop “平台” 包括 Hadoop 内核、MapReduce、Hadoop 分布式文件系统（HDFS）以及一些相关项目，有 Apache Hive 和 Apache HBase 等等。

Hadoop 的框架最核心的设计就是：HDFS 和 MapReduce。HDFS 为海量的数据提供了存储，则 MapReduce 为海量的数据提供了计算。

### 2.2 Hadoop 历史

Hadoop 由 Apache Software Foundation 公司于 2005 年秋天作为 Lucene 的子项目 Nutch 的一部分正式引入。它受到最先由 Google Lab 开发的 Map/Reduce 和 Google File System(GFS) 的启发。

2006 年 3 月份，Map/Reduce 和 Nutch Distributed File System (NDFS) 分别被纳入称为 Hadoop 的项目中。

Hadoop 是最受欢迎的在 Internet 上对搜索关键字进行内容分类的工具，但它也可以解决许多要求极大伸缩性的问题。例如，如果您要 grep 一个 10TB 的巨型文件，会出现什么情况？在传统的系统上，这将需要很长的时间。但是 Hadoop 在设计时就考虑到这些问题，采用并行执行机制，因此能大大提高效率。

目前有很多公司开始提供基于 Hadoop 的商业软件、支持、服务以及培训。

1. Cloudera 是一家美国的企业软件公司，该公司在 2008 年开始提供基于 Hadoop 的软件和服务。
2. GoGrid 是一家云计算基础设施公司，在 2012 年，该公司与 Cloudera 合作加速了企业采纳基于 Hadoop 应用的步伐。
3. Dataguise 公司是一家数据安全公司，同样在 2012 年该公司推出了一款针对 Hadoop 的数据保护和风险评估。

### 2.3 Hadoop 相关项目

- Hadoop Common

在 0.20 及以前的版本中，包含 HDFS、MapReduce 和其他项目公共内容，从 0.21 开始 HDFS 和 MapReduce 被分离为独立的子项目，其余内容为 Hadoop Common。

- HDFS

HDFS 是指 Hadoop 分布式文件系统（Distributed File System）－HDFS（Hadoop Distributed File System）

- MapReduce

MapReduce 是一个并行计算框架，0.20 前使用 `org.apache.hadoop.mapred` 旧接口，0.20 版本开始引入 `org.apache.hadoop.mapreduce` 的新 API。

- Apache HBase

HBase 是一个分布式 NoSQL 列数据库，类似谷歌公司的 BigTable。

- Apache Hive

Hive 是构建于 Hadoop 之上的数据仓库，通过一种类 SQL 语言 HiveQL 为用户提供数据的归纳、查询和分析等功能。

- Apache Mahout

机器学习算法软件包。

- Apache Sqoop

结构化数据（如关系数据库）与 Apache Hadoop 之间的数据转换工具。

- Apache ZooKeeper

分布式锁设施，提供类似 Google Chubby 的功能。

- Apache Avro

新的数据序列化格式与传输工具，将逐步取代 Hadoop 原有的 IPC 机制。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569206419123/wm)

### 2.4 Hadoop 优点

- 高可靠性

Hadoop 按位存储和处理数据的能力值得人们信赖。

- 高扩展性

Hadoop 是在可用的计算机集簇间分配数据并完成计算任务的，这些集簇可以方便地扩展到数以千计的节点中。

- 高效性

Hadoop 能够在节点之间动态地移动数据，并保证各个节点的动态平衡，因此处理速度非常快。

- 高容错性

Hadoop 能够自动保存数据的多个副本，并且能够自动将失败的任务重新分配。

- 低成本

与一体机、商用数据仓库以及 QlikView、Yonghong Z-Suite 等数据集市相比，hadoop 是开源的，项目的软件成本因此会大大降低。

- 支持 Java、C/C++

Hadoop 带有用 Java 语言编写的框架，因此运行在 Linux 生产平台上是非常理想的。Hadoop 上的应用程序也可以使用其他语言编写，比如 C++。

### 2.5 Hadoop 应用场景

美国著名科技博客 GigaOM 的专栏作家 Derrick Harris 在一篇文章中总结了 10 个 Hadoop 的应用场景：

- 在线旅游

全球 80% 的在线旅游网站都是在使用 Cloudera 公司提供的 Hadoop 发行版，其中 SearchBI 网站曾经报道过的 Expedia 也在其中。

- 移动数据

Cloudera 运营总监称，美国有 70% 的智能手机数据服务背后都是由 Hadoop 来支撑的，也就是说，包括数据的存储以及无线运营商的数据处理等，都是在利用 Hadoop 技术。

- 电子商务

这一场景应该是非常确定的，eBay 就是最大的实践者之一。国内的电商在 Hadoop 技术上也是储备颇为雄厚的。

- 能源开采

美国 Chevron 公司是全美第二大石油公司，他们的 IT 部门主管介绍了 Chevron 使用 Hadoop 的经验，他们利用 Hadoop 进行数据的收集和处理，其中这些数据是海洋的地震数据，以便于他们找到油矿的位置。

- 节能

另外一家能源服务商 Opower 也在使用 Hadoop,为消费者提供节约电费的服务，其中对用户电费单进行了预测分析。

- 基础架构管理

这是一个非常基础的应用场景，用户可以用 Hadoop 从服务器、交换机以及其他的设备中收集并分析数据。

- 图像处理

创业公司 Skybox Imaging 使用 Hadoop 来存储并处理图片数据，从卫星中拍摄的高清图像中探测地理变化。

- 诈骗检测：这个场景用户接触的比较少，一般金融服务或者政府机构会用到。利用 Hadoop 来存储所有的客户交易数据，包括一些非结构化的数据，能够帮助机构发现客户的异常活动，预防欺诈行为。

- IT 安全

除企业 IT 基础机构的管理之外，Hadoop 还可以用来处理机器生成数据以便甄别来自恶意软件或者网络中的攻击。

- 医疗保健

医疗行业也会用到 Hadoop，像 IBM 的 Watson 就会使用 Hadoop 集群作为其服务的基础，包括语义分析等高级分析技术等。医疗机构可以利用语义分析为患者提供医护人员，并协助医生更好地为患者进行诊断。

