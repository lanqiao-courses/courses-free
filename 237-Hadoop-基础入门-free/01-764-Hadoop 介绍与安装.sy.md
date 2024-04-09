---
show: step
version: 1.0
enable_checker: true
---

# Hadoop 介绍及 2.X 伪分布式安装

## 实验介绍

本节实验将对 Apache Hadoop 进行介绍。

#### 知识点

- Hadoop 生态系统
- Hadoop 环境搭建

#### 交流群

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid1735817-20220112-1641985629701)

## Hadoop 介绍

Apache Hadoop 软件库是一个框架，允许在集群服务器上使用简单的编程模型对大数据集进行分布式处理。Hadoop 被设计成能够从单台服务器扩展到数以千计的服务器，每台服务器都有本地的计算和存储资源。Hadoop 的高可用性并不依赖硬件，其代码库自身就能在应用层侦测并处理硬件故障，因此能基于服务器集群提供高可用性的服务。

### Hadoop 生态系统

经过多年的发展形成了 Hadoop2.X 生态系统，其结构如下图所示：

![图片描述](https://doc.shiyanlou.com/courses/237/2505524/0f7a4078ea94465252c084905a96b0e4-0)

当涉及大数据处理时，以下是一些关键的大数据技术和工具的简要介绍：

1. HDFS（Hadoop Distributed File System）：HDFS 是一个分布式文件系统，用于存储和处理大规模数据集。它具有高容错性和可扩展性，并且被设计用于在廉价的硬件上运行。

2. MapReduce：MapReduce 是一种编程模型和处理框架，用于在大规模集群上进行并行数据处理。它将数据处理任务分为两个阶段：Map（映射）和 Reduce（归约）。Map 阶段将输入数据切分为独立的任务，Reduce 阶段将 Map 的结果进行汇总。

3. Hive：Hive 是一个建立在 Hadoop 之上的数据仓库基础设施，它提供了类似于传统数据库的查询和分析功能。Hive 使用类似 SQL 的查询语言（Hive QL）来操作和分析存储在 HDFS 上的结构化数据。

4. HBase：HBase 是建立在 Hadoop 上的分布式、可扩展的 NoSQL 数据库。它提供了对大规模结构化数据的实时读写访问，并具有高可靠性和高性能的特点。HBase 的数据模型类似于分布式的面向列（column-oriented）数据库。

5. Spark：Spark 是一个快速、通用的大数据处理框架。它支持在内存中进行数据处理，提供了比传统的 MapReduce 更高效的数据处理方式。Spark 提供了丰富的 API（如 Scala、Java、Python 和 R），可以进行批处理、流处理、机器学习和图处理等任务。

6. Flume：Flume 是一个分布式、可靠的日志收集和聚合系统。它用于在大规模集群上收集、聚合和移动数据，可以从多个来源（如日志文件、消息队列）接收数据，并将其传输到各种目的地（如 HDFS、HBase、Kafka）。

7. Sqoop：Sqoop 是用于在 Apache Hadoop 和关系型数据库之间进行数据传输的工具。它支持将结构化数据从关系数据库（如 MySQL、Oracle）导入到 Hadoop 生态系统中，也可以将数据从 Hadoop 导出到关系数据库中。

这些技术和工具在大数据生态系统中扮演着重要的角色，各自具有不同的功能和用途，可以帮助处理和分析大规模数据集。

### Apache 版本衍化

Hadoop 是一个开源的分布式计算框架，最初是由 Apache Software Foundation（ASF）的一个项目开发的。以下是自进入 Apache 之后 Hadoop 版本的演化：

1. Hadoop 0.1.x: Hadoop 的早期版本是在 2006 年进入 Apache 之前开发的。这些版本主要由 Doug Cutting 和 Mike Cafarella 等人开发，其目标是构建一个可靠、可扩展的分布式计算框架。

2. Hadoop 0.20.x: Hadoop 0.20.x 系列是 Hadoop 进入 Apache 之后的第一个主要版本。它引入了许多重要的特性，如 HDFS 的容错性改进、MapReduce 任务调度器的改进以及任务本地化等。

3. Hadoop 1.x: Hadoop 1.x 系列于 2009 年发布，是一个重要的里程碑。它引入了 Hadoop Common（包含 Hadoop 分布式文件系统和 MapReduce 框架）、Hadoop YARN（用于集群资源管理）和 Hadoop Distributed File System（HDFS）。

4. Hadoop 2.x: Hadoop 2.x 系列于 2012 年发布，是一个重要的升级版本。其中最显著的改变是引入了 YARN（Yet Another Resource Negotiator），它将资源管理与作业调度分离，使 Hadoop 能够支持除了 MapReduce 之外的其他计算模型，如 Spark、Tez 等。

5. Hadoop 3.x: Hadoop 3.x 系列是自 2017 年开始发布的最新版本。它引入了一些重要的改进，如 HDFS 的多命名空间、容器化支持、作业优先级调度、Erasure Coding（纠删码）等。此外，Hadoop 3.x 还改进了性能和可靠性，并提供了更好的扩展性。

随着时间的推移，Hadoop 不断演化和改进，以应对不断增长的大数据需求和技术挑战。每个版本都带来了新的功能和改进，使 Hadoop 成为处理和分析大规模数据的核心工具之一。

## Hadoop2.X 伪分布安装

Hadoop 安装有如下三种方式：

- `单机模式`：安装简单，几乎不用做任何配置，但仅限于调试用途。
- `伪分布模式`：在单节点上同时启动 NameNode、DataNode、Secondary Namenode、ResourceManager 和 NodeManager 等 5 个进程，模拟分布式运行的各个节点。
- `完全分布式模式`：正常的 Hadoop 集群，由多个各司其职的节点构成。

由于实验环境的限制，本节课程将讲解**伪分布模式安装**，并在随后的课程中以该环境为基础进行其他组件部署实验。以下为伪分布式环境下在 CentOS6 中配置 Hadoop-2.9.2，该配置可以作为其他 Linux 系统和其他版本的 Hadoop 部署参考。

### 软硬件环境说明

**实验楼环境已配置，除 Host 文件需要修改，其他无需操作。**

节点使用 Ubuntu 系统，防火墙和 SElinux 需要禁用，创建了一个 shiyanlou 用户，并在系统根目录下创建 `/moudle` 目录，用于存放 Hadoop 等组件运行包。由于该目录用于安装 Hadoop 等组件程序，用户对 shiyanlou 必须赋予 rwx 权限（一般做法是 root 用户在根目录下创建 `/opt` 目录，并修改该目录拥有者为 shiyanlou(`chown –R shiyanlou:shiyanlou /opt`)。

**Hadoop 搭建环境：**

- 虚拟机操作系统：Ubuntu 64 位，4 核，16G 内存
- JDK：1.8.0_292 64 位
- Hadoop：2.9.2

### 环境搭建

实验环境的虚拟机已经完成部分安装环境的配置，读者无需操作，当在其他机器部署时可以参考本节进行环境搭建。

> **注意：** shiyanlou 用户的密码点击桌面右边工具栏的环境信息查看。默认的用户名 / 密码为：shiyanlou / shiyanlou。

#### 配置本地环境

**设置机器名（实验楼环境已配置，无需操作）：**

使用 `sudo vi /etc/sysconfig/network`。

打开配置文件，根据实际情况设置该服务器的机器名，新机器名在重启后生效。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid214292labid764timestamp1488190165875.png)

**设置 Host 映射文件（需要您在实验楼环境操作）：**

1. 设置 IP 地址与机器名的映射，设置信息如下：

```bash
# 配置主机名对应的 IP 地址
$ sudo vi /etc/hosts
```

设置：`<IP 地址> <主机名>`

例如：`192.168.42.3 55a95997af1c hadoop`

注意：就是在打开的 `/etc/hosts` 文件的最后一行加上 hadoop，记得使用的是 `tab` 键而不是空格。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid214292labid764timestamp1488269552713.png)

2. 使用 `ping` 命令验证设置是否成功。

```bash
ping hadoop
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid214292labid764timestamp1488190512632.png)

#### 设置操作系统环境（以下内容实验楼环境已配置，无需操作）

**关闭防火墙：**

在 Hadoop 安装过程中需要关闭防火墙和 SElinux，否则会出现异常。

1. 使用 `sudo service iptables status`。

查看防火墙状态，如下所示表示 iptables 已经开启。

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141096136)

> 注意：若弹出权限不足，可能防火墙已经关闭，请输入命令：`chkconfig iptables --list` 查看防火墙的状态。

2. 使用如下命令关闭 iptables。

```bash
sudo chkconfig iptables off
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141135347)

**关闭 SElinux：**

1. 使用 `getenforce` 命令查看是否关闭。
2. 修改 `/etc/selinux/config` 文件。

将 `SELINUX=enforcing` 改为 `SELINUX=disabled`，执行该命令后重启机器生效。

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141171432)

**更新 OpenSSL（实验楼环境已配置，无需操作）：**

CentOS 自带的 OpenSSL 存在 bug，使用如下命令进行更新：

```bash
yum update openssl
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141340991)

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141352659)

**SSH 无密码验证配置（实验楼环境已配置，无需操作）：**

1. 使用 `sudo vi /etc/ssh/sshd_config`，打开 sshd_config 配置文件，开放三个配置，如下图所示：

```text
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141462396)

2. 配置后重启服务。

```bash
sudo service sshd restart
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141475180)

3. 使用 shiyanlou 用户登录使用如下命令生成私钥和公钥。

```bash
ssh-keygen -t rsa
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141507684)

4. 进入 `/home/shiyanlou/.ssh` 目录把公钥命名为 `authorized_keys`，使用命令如下：

```bash
cp id_rsa.pub authorized_keys
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141519844)

5. 使用如下设置 authorized_keys 读写权限。

```bash
sudo chmod 400 authorized_keys
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141529173)

6. 测试 ssh 免密码登录是否生效。

### Hadoop 环境搭建（以下内容只有部分需在实验楼环境操作）

#### 下载并解压 hadoop 安装包（无需操作）

```bash
wget https://labfile.oss-cn-hangzhou.aliyuncs.com/courses/21472/hadoop-2.9.2.tar.gz
tar -xzf hadoop-2.9.2-bin.tar.gz
rm -rf hadoop-2.9.2
mv hadoop-2.9.2 /opt
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147689247)

#### 在 Hadoop-2.9.2 目录下创建子目录（需要操作）

```bash
cd /opt/hadoop-2.9.2
mkdir -p tmp hdfs hdfs/name hdfs/data
ls
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147658777)

进入 hdfs 目录中，使用 `chmod -R 755 data` 命令把 hdfs/data 设置为 755，否则 DataNode 会启动失败。

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147759048)

**后续配置均已配置完毕，无需再次操作，可直接操作格式化 namenode 和启动 hadoop 的步骤。**

#### 配置 hadoop-env.sh

1. 进入 `hadoop-2.9.2/etc/hadoop` 目录，打开配置文件 `hadoop-env.sh`。

```bash
cd /opt/hadoop-2.9.2/etc/hadoop
vi hadoop-env.sh
```

2. 加入配置内容，设置 Hadoop 中 jdk 和 `hadoop/bin` 路径。

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$PATH:/opt/hadoop-2.9.2/bin
```

3. 编译配置文件 `hadoop-env.sh`，并确认生效。

```bash
source hadoop-env.sh
hadoop version
```

#### 配置 core-site.xml

```bash
# 进入配置目录，下面的配置文件均在该目录下
cd /opt/hadoop-2.9.2/etc/hadoop
```

1. 使用如下命令打开 `core-site.xml` 配置文件。

```bash
sudo vi core-site.xml   # 如果 sudo 需要密码，可以点击桌面右侧工具栏的 ssh 直连，其中的密码就是这里需要输入的密码
```

2. 在配置文件中，按照如下内容进行配置。

```xml
<configuration>
  <property>
    <name>fs.default.name</name>
    <value>hdfs://localhost:9000</value>
  </property>
  <property>
    <name>hadoop.tmp.dir</name>
    <value>/opt/hadoop-2.9.2/tmp</value>
  </property>
</configuration>
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147866409)

#### 配置 hdfs-site.xml

1. 使用如下命令打开 `hdfs-site.xml` 配置文件。

```bash
sudo vi hdfs-site.xml
```

2. 在配置文件中，按照如下内容进行配置。

```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
  <property>
    <name>dfs.name.dir</name>
    <value>/opt/hadoop-2.9.2/hdfs/name</value>
  </property>
  <property>
    <name>dfs.data.dir</name>
    <value>/opt/hadoop-2.9.2/hdfs/data</value>
  </property>
</configuration>
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147890390)

#### 配置 mapred-site.xml

1. 使用如下命令打开 `mapred-site.xml` 配置文件。

```bash
sudo vi mapred-site.xml
```

2. 在配置文件中，按照如下内容进行配置。

```xml
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
```

![图片描述](https://doc.shiyanlou.com/courses/237/2505524/675cec2cebebff61d36f0a0f51b28183-0)

#### 配置 masters 和 slaves 文件

1. 设子主节点。

```bash
vi masters
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433148013243)

2. 设置从节点。

```bash
vi worker
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433148127917)

#### 格式化 namenode

在 hadoop 机器上使用如下命令进行格式化 namenode。

```bash
cd /opt/hadoop-2.9.2/bin
./hadoop namenode -format
```

![图片描述](https://doc.shiyanlou.com/courses/237/2505524/fcad19350adae61283c6de90d6b349df-0)

#### 启动 hadoop

```bash
cd /opt/hadoop-2.9.2/
sbin/start-all.sh
```

![图片描述](https://doc.shiyanlou.com/courses/237/2505524/b920d9c0ab490165e1a589c5a258c4b6-0/wm)


#### 用 jps 检验各后台进程是否成功启动

使用 `jps` 命令查看 hadoop 相关进程是否启动。

![图片描述](https://doc.shiyanlou.com/courses/237/2505524/69f07c9a2fdd0cd83eeefaeda0cd983c-0/wm)

> 注意：如果是会员，推荐把实验环境保存下来，后续的实验都会依赖于现在的环境；如果是非会员进行后续的实验，则每次开始实验都需要对环境进行配置。
