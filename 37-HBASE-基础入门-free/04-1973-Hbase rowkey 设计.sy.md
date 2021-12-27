---
show: step
version: 1.0
enable_checker: true
---

# HBase rowkey 设计

## 实验介绍

学习 HBase shell。

#### 知识点

- HBase 原理
- 热点问题
- rowkey 设计

## HBase 原理详解

本章首先简单介绍 HBase 的架构和它对数据的写入交互。

本节实验包括以下 4 小节实验：

1. HBase 的架构

2. HBase 的写入和交互

3. 连接 HBase

4. 查看元数据表

### HBase 的架构

首先我们介绍 HBase 的架构。

![5-2-1](https://doc.shiyanlou.com/document-uid702660labid6674timestamp1528277980567.png)

如图所示，HBase 的最底层结构是基于 hdfs 的，它将自己的日志文件 `hlog`，以及数据表 `Region` 存储在 hdfs 的 datanode 当中。

而管理 HBase 的主要是 zookeeper 和 master。其中，HBase 主要依靠 zookeeper 管理，master 主要负责启动 HBase 时分配区域到指定区域服务器，监听来自 Zookeeper 的通知和提供管理表的接口。

同学肯定对刚刚提到的 regionServer（区域服务器），和区域（region）感到陌生。下面我们来具体解释一下概念。regionServer 即是一台 HBase 服务器，一个进程。我们配置的伪分布式 HBase 就只有一台 regionServer，而真正的分布式集群每一台部署了 HBase 的节点都是一个 regionServer。

我们使用的 HBase 是一个列式数据库，一张表可以达到十亿行，这就需要将表拆分成多个部分储备起来。即是分别存入 region 中，由 regionserver 管理。

### HBase 写入与交互

![5-2-1](https://doc.shiyanlou.com/document-uid702660labid6674timestamp1528277980567.png)

仍然由图可知，RegionServer 主要管理两种文件，一是 Hlog（预写日志 write-ahead log WAL），二是 region（实际的数据文件）。当用户向 HBase 请求写入 put 数据时，首先就要写入 Hlog 实现的预写日志当中，当 HBase 服务器崩溃时，hlog 可以回滚还没有持久化的数据，以便后续从 WAL 中恢复数据。
当数据写入预写日志后，数据便会存放当 region 中的 memStore 中，同时还会检查 memStore 是否写满，如果写满，就会被请求刷写到磁盘中。

当我们用客户端与 HBase 交互时，一般要经历如下步骤：

1. 联系 zookeeper，找出 meta 表（元数据表）所在 rs(regionserver) `/hbase/meta-region-server`；
2. 定位 row key, 找到对应 region server；
3. 缓存信息在本地（再次查找数据时，不需要经过 1,2 步骤）；
4. 联系 RegionServer；
5. HRegionServer 负责 open HRegion 对象，为每个列族创建 Store 对象，Store 包含多个 StoreFile 实例，他们是对 HFile 的轻量级封装。每个 Store 还对应了一个 MemStore，用于内存存储数据。

### 连接 HBase

首先，先打开终端切换到 hadoop 用户下：

```bash
su -l hadoop #密码为hadoop
```

![3-1](https://doc.shiyanlou.com/document-uid702660labid6145timestamp1524117265963.png/wm)

然后，我们需要进入 hadoop-2.7.3 目录下的 `sbin` 文件夹中。

```bash
 cd /opt/hadoop-2.7.3/sbin/
```

![2-4.2-1](https://doc.shiyanlou.com/document-uid702660labid6604timestamp1527751480885.png/wm)

启动 hdfs：

```bash
./start-all.sh
```

![2-4.2-3](https://doc.shiyanlou.com/document-uid702660labid6604timestamp1527751880858.png/wm)

使用 `hbase shell` 命令来连接正在运行的 Hbase 实例，该命令位于 HBase 安装包下的 `bin/` 目录。HBase Shell 提示符以 `>` 符号结束。

```bash
cd /opt/hbase-1.2.6/bin/
./start-hbase.sh
./hbase shell
```

![3-1.1-1](https://doc.shiyanlou.com/document-uid702660labid6605timestamp1527755583975.png/wm)

### 查看元数据表

输入命令：

```bash
list_namespace
```

![5-2.4-1](https://doc.shiyanlou.com/document-uid702660labid6674timestamp1528443198920.png/wm)

namespace 即是 HBase 当中的数据库，我们可以看到初始的 HBase 有两个 HBase 数据库，其一是 default，其二是 HBase。

我们自己创建的表都在 default 这个 namespace 下，而元数据表在 HBase 下。

下面，我们查看 HBase 下面的表。

输入命令：

```bash
list_namespace_tables 'hbase'
```

![5-2.4-2](https://doc.shiyanlou.com/document-uid702660labid6674timestamp1528443219182.png/wm)

我们可以发现，hbase 这个 namespace 下存在两张表，其一是 meta 表，其二是 namespace 表，它们都是系统表（system table）除此之外都是用户表（user table）。

为了更深入的观察，我们先创建一个自己的 namespace，并且在该 namespace 当中创建一张表，插入一些数据。

创建 namespace 和 table：

```bash
create_namespace 'ns1'
create 'ns1:t1','f1'
```

![5-2.4-3](https://doc.shiyanlou.com/document-uid702660labid6674timestamp1528443263224.png/wm)

插入一些数据：

```bash
put 'ns1:t1','row1','f1:id',100
put 'ns1:t1','row1','f1:name','tom'
put 'ns1:t1','row1','f1:age',22
```

![5-2.4-4](https://doc.shiyanlou.com/document-uid702660labid6674timestamp1528443286336.png/wm)

再次查看 namespace：

```bash
list_namespace
```

![5-2.4-5](https://doc.shiyanlou.com/document-uid702660labid6674timestamp1528443317146.png/wm)

我们可以看到我们已经创建 `ns1` namespace，我们查看下面的 `t1` 表。

```bash
scan 'ns1:t1'
```

![5-2.4-6](https://doc.shiyanlou.com/document-uid702660labid6674timestamp1528443340789.png/wm)

现在我们查看该 meta 表：

```bash
scan 'hbase:meta'
```

![5-2.4-7](https://doc.shiyanlou.com/document-uid702660labid6674timestamp1528443367924.png/wm)

我们观察元数据表的 row 设计，可以看到其中包括了表所在的 namespace，表名，以及在该 region 的 startkey，时间戳，以及对前面信息的计算产生的码。

> 注意：因为我们是伪分布模式，只有一台虚拟机，也就只有一台 regionServer，所以 startkey 默认为空，图中两个逗号便是代表 startkey。

重新打开一个终端，切换到 hadoop 用户下，我们查看 hdfs 文件系统中的存储情况：

```bash
su -l hadoop
hdfs dfs -lsr /
```

![5-2.4-8](https://doc.shiyanlou.com/document-uid702660labid6674timestamp1528443391214.png/wm)

图中圈出的即是 region 目录信息，我们创建的表被分区以后进行存储。

## 热点问题

#### 原因

在设计 rowkey 时，有大量连续编号的 rowkey。这就会导致大量 rowkey 相近的记录集中在个别 region 里，也就是集中在一台或几台 regionServer 当中。当 client 检索记录时，对个别 region 访问过多，这时此 region 所在的主机过载，就导致热点问题的出现。

#### 影响

对于 HBase 来说，一旦存在热点问题，便会导致 HBase 的读写大量集中与一台或少数的几台 regionServer 当中。这时，会造成 HBase 集群的负载不均衡，大幅度降低 HBase 的响应速度。

## rowkey 设计原则和方法

rowkey 设计首先应当遵循三大原则。

#### rowkey 长度原则

rowkey 是一个二进制码流，可以为任意字符串，最大长度为 64kb，实际应用中一般为 10-100bytes，它以 byte[] 形式保存，一般设定成定长。

一般越短越好，不要超过 16 个字节，注意原因如下：

1、目前操作系统都是 64 位系统，内存 8 字节对齐，控制在 16 字节，8 字节的整数倍利用了操作系统的最佳特性。

2、HBase 将部分数据加载到内存当中，如果 rowkey 过长，内存的有效利用率就会下降。

#### rowkey 散列原则

如果 rowkey 按照时间戳的方式递增，不要将时间放在二进制码的前面，建议将 rowkey 的高位字节采用散列字段处理，由程序随即生成。低位放时间字段，这样将提高数据均衡分布，各个 regionServer 负载均衡的几率。

如果不进行散列处理，首字段直接使用时间信息，所有该时段的数据都将集中到一个 regionServer 当中，这样当检索数据时，负载会集中到个别 regionServer 上，造成热点问题，会降低查询效率。

#### rowkey 唯一原则

必须在设计上保证其唯一性，rowkey 是按照字典顺序排序存储的，因此，设计 rowkey 的时候，要充分利用这个排序的特点，将经常读取的数据存储到一块，将最近可能会被访问的数据放到一块。但是这里的量不能太大，如果太大需要拆分到多个节点上去。

所以良好的 rowkey 设计，应当遵循三大原则，并且能让数据分散，从而避免热点问题。本节介绍几种常用的 rowkey 设计方法，以供同学们学习。

> 注意：本节理论知识较多，不过都是大数据岗位面试中常见问题，希望同学们认真研读。

### 加盐

这里所说的加盐并非密码学中的加盐，而是在 rowkey 的前面分配随机数，当给 rowkey 随机前缀后，它就能分布到不同的 region 中，这里的前缀应该和你想要数据分散的不同的 region 的数量有关。

为了让同学们更好的理解加盐（salting）这个 rowkey 设计方法。我们以电信公司为例。

当我们去电信公司打印电话详单也就是通话记录。对于通话记录来说，每个人每月可能都有很多通话记录，而使用电信的用户也是亿计。这种信息，我们就能存入 HBase 当中。

对于通话记录，我们有什么信息需要保存呢？首先，肯定应该有主叫和被叫，然后有主叫被叫之间的通话时长，以及通话时间。除此之外，还应该有主叫的位置信息，和被叫的位置信息。

由此，我们的通话记录表需要记录的信息就出来了：主叫、被叫、时长、时间、主叫位置、被叫位置。

我们该如何来设计一张 HBase 表呢？

首先，HBase 表是依靠 rowkey 来定位的，我们应该将尽可能多的将查询的信息编入 rowkey 当中。HBase 的元数据表 mate 表就给我们了一个很好的示例。它包括了 namespace，表名，startKey，时间戳，计算出来的码（用于分散数据）。

所以，当我们设计通话记录的 rowkey 时，需要将能唯一确定该条记录的数据编入 rowkey 当中。即是需要将主叫、被叫、时间编入。

如下所示：

```txt
17765657979 18688887777 201806121502 #主叫，被叫，时间
```

但是我们能否将我们设计的 rowkey 真正应用呢？

当然是可以的，但是热点问题便会随之而来。

例如你的电话是以 177 开头，电信的 HBase 集群有 500 台，你的数据就只可能被存入一台或者两台机器的 region 当中，当你需要打印自己的通话记录时，就只有一台机器为你服务。而若是你的数据均匀分散到 500 机器中，就是整个集群为你服务。两者之间效率速度差了不止一个数量级。

> 注意：由于我们的 regionServer 就只有一台，没有集群环境，所以我们只介绍方法和理论操作，不提供实际结果。

因为我们设定整个 HBase 集群有 500 台，所以我们随机在 0-499 之间中随机数字，添加到 rowkey 首部。

如下所示：

```txt
12 17765657979 18688887777 201806121502 #随机数，主叫，被叫，时间
```

在插入数据时，判断首部随机数字，选择对应的 region 存入，由于 rowkey 首部数字随机，所以数据也将随机分布到不同的 regionServer 中。这样就能很好的避免热点问题了。

### 预分区

通常 HBase 会自动处理 region 拆分，当 region 的大小到达一定阈值后，region 将被拆分成两个，之后在两个 region 都能继续增长数据。

然而在这个过程当中，会出现两个问题：

第一点，就是我们所说的热点问题，数据会继续往一个 region 中写，出现写热点问题；

第二点，则是 `拆分合并风暴`，当用户的 region 大小以恒定的速度增长，region 的拆分会在同一时间发生，因为同时需要 `压缩`region 中的存储文件，这个过程会重写拆分后的 region，这将会引起磁盘 I/O 上升 。

**压缩：HBase 支持大量的压缩算法，而且通常开启压缩，因为 cpu 压缩和解压的时间比从磁盘读写数据的时间消耗的更短，所以压缩会带来性能的提升。**

> 对于拆分合并风暴，通常我们需要关闭 HBase 的自动管理拆分。然后手动调用 HBase 的 split（拆分）和 major_compact（压缩），对其进行时间控制，来分散 I/O 负载。但是其中的 split 操作同样是高 I/O 的操作。

为了解决这些问题，预分区就是一种很好的方法，通常它和 `加盐`结合起来使用。

所谓预分区，就是预先创建 HBase 表分区。这需要我们明确 rowkey 的取值范围和构成逻辑。

比如前面我们所列举的电信电话详单表。通过加盐我们得到的 rowkey 构成是：`随机数+主叫+被叫+时间`，如果我们现在并没有 500 台机器，只有 10 台，但是按照我们的计划，未来将扩展到 500 台的规模。所以我们仍然设计 0 到 499 的随机数，但是将以主叫 177 开头的通话记录分配到十个 region 当中，所以我们将随机数均分成十个区域，范围如下：

```txt
-50,50-100,100-150,150-200,200-250,250-300,300-350,350-400,400-450,450-
```

然后我们将我们的预分区存入数组当中，当插入数据时，先根据插入数据的首部随机数，判断分区位置，再进行插入数据。同样，这样也能使得各台节点负载均衡。

### 哈希

细心的同学可能会发现，在我们刚刚提出的 **加盐** 与 **预分区** rowkey 设计方法中，并没有完整运用到 rowkey 设计的散列原则。

更一步思考下，我们会发现如果只运用 **加盐** 与 **预分区** rowkey 设计方法，数据会真正无序随即分布在 HBase 集群当中，这并没有让我们利用到 HBase 根据字典顺序排序的这一特点。

由此，哈希这一设计理念便顺理成章的出现在我们眼前。

同样以电信通话记录为例，我们想将某一天的通话记录存入同一 region 当中，所以我们利用哈希函数算出哈希值，再模以我们需要存入 region 数量，我们就能将相同输入的数据，存入同一 region 当中。

在 **主叫，被叫，时间** rowkey 当中，我们将 callerID（主叫）与 20180612（某一天的时间）作为参数，传入哈希函数当中，将得到的哈希值模以 500，余数添加到 rowkey 首部中，再结合 **预分区** 设计方法，就能将数据均匀分布到 regionServer 当中。

同时，我们还能将相同 rowkey 的数据收集到一台节点上，在避免热点问题的情况下，充分利用 HBase 字典排序的优点。

### 反转

对于以手机号码这样比较固定开头的 rowkey（例如开头 177,159,138），但是它的后几位都是随机的，没有规律的。我们可以将手机号反转之后作为 rowkey，这样就避免了热点问题。

这就是 rowkey 设计的另一种方法 **反转**，通过反转固定长度或者数字格式的 rowkey。这样可以使得 rowkey 中经常改变的部分（最没有意义的部分）放在前面。这样可以有效的随机 rowkey，但是牺牲了 rowkey 的有序性。

## 实验总结

本章介绍了 HBase 原理，详细介绍了 HBase 的架构与写入交互流程。同时介绍了 HBase 中的热点问题，以及如何通过 rowkey 设计来解决热点问题。

#### 参考资料

- http://hbase.apache.org/book.html
