---
show: step
version: 1.0
enable_checker: true
---

# Hadoop 介绍及 1.X 伪分布式安装

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

经过多年的发展形成了 Hadoop1.X 生态系统，其结构如下图所示：

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1427382197256)

- `HDFS`：Hadoop 生态圈的基本组成部分是 Hadoop 分布式文件系统（HDFS）。HDFS 是一种分布式文件系统，数据被保存在计算机集群上，HDFS 为 HBase 等工具提供了基础。
- `MapReduce`：Hadoop 的主要执行框架是 MapReduce，它是一个分布式、并行处理的编程模型，MapReduce 把任务分为 map（映射）阶段和 reduce（化简）阶段。由于 MapReduce 工作原理的特性，Hadoop 能以并行的方式访问数据，从而实现快速访问数据。
- `Hbase`：HBase 是一个建立在 HDFS 之上，面向列的 NoSQL 数据库，用于快速读 / 写大量数据，HBase 使用 Zookeeper 进行管理。
- `Zookeeper`：用于 Hadoop 的分布式协调服务。Hadoop 的许多组件依赖于 Zookeeper，它运行在计算机集群中，用于管理 Hadoop 集群。
- `Pig`：它是 MapReduce 编程的复杂性的抽象。Pig 平台包括运行环境和用于分析 Hadoop 数据集的脚本语言 (Pig Latin)，其编译器将 Pig Latin 翻译成 MapReduce 程序序列。
- `Hive`：类似于 SQL 高级语言，用于运行存储在 Hadoop 上的查询语句，Hive 让不熟悉 MapReduce 的开发人员也能编写数据查询语句，然后这些语句被翻译为 Hadoop 上面的 MapReduce 任务。像 Pig 一样，Hive 作为一个抽象层工具，吸引了很多熟悉 SQL 而不是 Java 编程的数据分析师。
- `Sqoop`：一个连接工具，用于在关系数据库、数据仓库和 Hadoop 之间转移数据。Sqoop 利用数据库技术描述架构，进行数据的导入 / 导出；利用 MapReduce 实现并行化运行和容错技术。
- `Flume`：提供了分布式、可靠、高效的服务，用于收集、汇总大数据，并将单台计算机的大量数据转移到 HDFS。它基于一个简单而灵活的架构，利用简单的可扩展的数据模型，将企业中多台计算机上的数据转移到 Hadoop 中。

### Apache 版本衍化

截止 2012年12月23日，Apache Hadoop 版本分为两代，我们将第一代 Hadoop 称为 Hadoop 1.0，第二代 Hadoop 称为 Hadoop 2.0。（注：2017 年 12 月 13 日，已发布 3.0 版本。）

第一代 Hadoop 包含三个大版本，分别是 0.20.x，0.21.x 和 0.22.x。其中，0.20.x 最后演化成 1.0.x，变成了稳定版，而 0.21.x 和 0.22.x 则包括 NameNode HA 等新的重大特性。

第二代 Hadoop 包含两个版本，分别是 0.23.x 和 2.x，它们完全不同于 Hadoop 1.0，是一套全新的架构，均包含 HDFS Federation 和 YARN 两个系统，相比于 0.23.x，2.x 增加了 NameNode HA 和 Wire-compatibility 两个重大特性。

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1427637657385)

## Hadoop1.X 伪分布安装

Hadoop 安装有如下三种方式：

- `单机模式`：安装简单，几乎不用做任何配置，但仅限于调试用途。
- `伪分布模式`：在单节点上同时启动 NameNode、DataNode、JobTracker、TaskTracker、Secondary Namenode 等 5 个进程，模拟分布式运行的各个节点。
- `完全分布式模式`：正常的 Hadoop 集群，由多个各司其职的节点构成。

由于实验环境的限制，本节课程将讲解伪分布模式安装，并在随后的课程中以该环境为基础进行其他组件部署实验。以下为伪分布式环境下在 CentOS6 中配置 Hadoop-1.1.2，该配置可以作为其他 Linux 系统和其他版本的 Hadoop 部署参考。

### 软硬件环境说明

**实验楼环境已配置，除 Host 文件需要修改，其他无需操作。**

节点使用 CentOS 系统，防火墙和 SElinux 需要禁用，创建了一个 shiyanlou 用户，并在系统根目录下创建 `/app` 目录，用于存放 Hadoop 等组件运行包。由于该目录用于安装 hadoop 等组件程序，用户对 shiyanlou 必须赋予 rwx 权限（一般做法是 root 用户在根目录下创建 `/app` 目录，并修改该目录拥有者为 shiyanlou(`chown –R shiyanlou:shiyanlou /app`)。

**Hadoop 搭建环境：**

- 虚拟机操作系统：CentOS6.6 64 位，单核，1G 内存
- JDK：1.7.0_55 64 位
- Hadoop：1.1.2

### 环境搭建

实验环境的虚拟机已经完成部分安装环境的配置只需要你完成少量环境配置内容即可，当在其他机器部署时可以参考本节进行环境搭建。

> **注意：** shiyanlou 用户的密码点击桌面右边工具栏的环境信息查看。

#### 配置本地环境

**设置机器名（实验楼环境已配置，无需操作）：**

使用 `sudo vi /etc/sysconfig/network`。

打开配置文件，根据实际情况设置该服务器的机器名，新机器名在重启后生效。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid214292labid764timestamp1488190165875.png/wm)

**设置 Host 映射文件（需要您在实验楼环境操作）：**

1. 设置 IP 地址与机器名的映射，设置信息如下：

```bash
# 配置主机名对应的IP地址
$ sudo vi /etc/hosts
```

设置：`<IP 地址> <主机名>`

例如：`192.168.42.3 55a95997af1c hadoop`

注意：就是在打开的 `/etc/hosts` 文件的最后一行加上 hadoop，记得使用的是 `tab` 键而不是空格。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid214292labid764timestamp1488269552713.png/wm)

2. 使用 `ping` 命令验证设置是否成功。

```bash
ping hadoop
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid214292labid764timestamp1488190512632.png/wm)

#### 设置操作系统环境（以下内容实验楼环境已配置，无需操作）

**关闭防火墙：**

在 Hadoop 安装过程中需要关闭防火墙和 SElinux，否则会出现异常。

1. 使用 `sudo service iptables status`。

查看防火墙状态，如下所示表示 iptables 已经开启。

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141096136/wm)

> 注意：若弹出权限不足，可能防火墙已经关闭，请输入命令：`chkconfig iptables --list` 查看防火墙的状态。

2. 使用如下命令关闭 iptables。

```bash
sudo chkconfig iptables off
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141135347/wm)

**关闭 SElinux：**

1. 使用 `getenforce` 命令查看是否关闭。
2. 修改 `/etc/selinux/config` 文件。

将 `SELINUX=enforcing` 改为 `SELINUX=disabled`，执行该命令后重启机器生效。

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141171432/wm)

**JDK 安装及配置：**

1. 下载 JDK 64bit 安装包。

打开 JDK 64bit 安装包下载链接，注意需要登录 Oracle 账户才能下载：

https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html

打开界面之后，选择下载对应平台版本的 .tar.gz 包：

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20201021-1603245067947)

2. 创建 `/app` 目录，把该目录的所有者修改为 shiyanlou。

```bash
sudo mkdir /app
sudo chown -R shiyanlou:shiyanlou /app
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141273685/wm)

3. 创建 `/app/lib` 目录，使用命令如下：

```bash
mkdir /app/lib
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141283780/wm)

4. 把下载的安装包解压并迁移到 `/app/lib` 目录下。

```bash
cd /home/shiyanlou/install-pack
tar -zxf jdk-7u55-linux-x64.tar.gz
mv jdk1.7.0_55/ /app/lib
ll /app/lib
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141297095/wm)

5. 使用 `sudo vi /etc/profile` 命令打开配置文件，设置 JDK 路径。

```bash
export JAVA_HOME=/app/lib/jdk1.7.0_55
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141308466/wm)

6. 编译并验证。

```bash
source /etc/profile
java -version
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141316786/wm)

> 需要注意的是：由于实验楼虚拟机原因使用 `java -version` 显示 JDK 版本为 1.5，该版本并不影响后续实验。

**更新 OpenSSL（实验楼环境已配置，无需操作）：**

CentOS 自带的 OpenSSL 存在 bug，使用如下命令进行更新：

```bash
yum update openssl
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141340991/wm)

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141352659/wm)

**SSH 无密码验证配置（实验楼环境已配置，无需操作）：**

1. 使用 `sudo vi /etc/ssh/sshd_config`，打开 sshd_config 配置文件，开放三个配置，如下图所示：

```text
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141462396/wm)

2. 配置后重启服务。

```bash
sudo service sshd restart
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141475180/wm)

3. 使用 shiyanlou 用户登录使用如下命令生成私钥和公钥。

```bash
ssh-keygen -t rsa
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141507684/wm)

4. 进入 `/home/shiyanlou/.ssh` 目录把公钥命名为 `authorized_keys`，使用命令如下：

```bash
cp id_rsa.pub authorized_keys
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141519844/wm)

5. 使用如下设置 authorized_keys 读写权限。

```bash
sudo chmod 400 authorized_keys
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433141529173/wm)

6. 测试 ssh 免密码登录是否生效。

### Hadoop 环境搭建（以下内容只有部分需在实验楼环境操作）

#### 下载并解压 hadoop 安装包（无需操作）

在 Apache 的归档目录中下载 `hadoop-1.1.2-bin.tar.gz` 安装包，也可以在 `/home/shiyanlou/install-pack` 目录中找到该安装包，解压该安装包并把该安装包复制到 `/app` 目录中。

```bash
cd /home/shiyanlou/install-pack
tar -xzf hadoop-1.1.2-bin.tar.gz
rm -rf /app/hadoop-1.1.2
mv hadoop-1.1.2 /app
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147689247/wm)

#### 在 Hadoop-1.1.2 目录下创建子目录（需要操作）

```bash
cd /app/hadoop-1.1.2
mkdir -p tmp hdfs hdfs/name hdfs/data
ls
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147658777/wm)

进入 hdfs 目录中，使用 `chmod -R 755 data` 命令把 hdfs/data 设置为 755，否则 DataNode 会启动失败。

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147759048/wm)

**后续配置均已配置完毕，无需再次操作，可直接操作格式化 namenode 和启动 hadoop 的步骤。**

#### 配置 hadoop-env.sh

1. 进入 `hadoop-1.1.2/conf` 目录，打开配置文件 `hadoop-env.sh`。

```bash
cd /app/hadoop-1.1.2/conf
vi hadoop-env.sh
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147774618/wm)

2. 加入配置内容，设置 Hadoop 中 jdk 和 `hadoop/bin` 路径。

```bash
export JAVA_HOME=/app/lib/jdk1.7.0_55
export PATH=$PATH:/app/hadoop-1.1.2/bin
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147786848/wm)

3. 编译配置文件 `hadoop-env.sh`，并确认生效。

```bash
source hadoop-env.sh
hadoop version
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147797748/wm)

#### 配置 core-site.xml

1. 使用如下命令打开 `core-site.xml` 配置文件。

```bash
sudo vi core-site.xml   # 如果 sudo 需要密码，可以点击桌面右侧工具栏的ssh直连，其中的密码就是这里需要输入的密码
```

2. 在配置文件中，按照如下内容进行配置。

```xml
<configuration>
  <property>
    <name>fs.default.name</name>
    <value>hdfs://hadoop:9000</value>
  </property>
  <property>
    <name>hadoop.tmp.dir</name>
    <value>/app/hadoop-1.1.2/tmp</value>
  </property>
</configuration>
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147866409/wm)

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
    <value>/app/hadoop-1.1.2/hdfs/name</value>
  </property>
  <property>
    <name>dfs.data.dir</name>
    <value>/app/hadoop-1.1.2/hdfs/data</value>
  </property>
</configuration>
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433147890390/wm)

#### 配置 mapred-site.xml

1. 使用如下命令打开 `mapred-site.xml` 配置文件。

```bash
sudo vi mapred-site.xml
```

2. 在配置文件中，按照如下内容进行配置。

```xml
<configuration>
  <property>
    <name>mapred.job.tracker</name>
    <value>hadoop:9001</value>
  </property>
</configuration>
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433148000192/wm)

#### 配置 masters 和 slaves 文件

1. 设子主节点。

```bash
vi masters
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433148013243/wm)

2. 设置从节点。

```bash
vi slaves
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433148127917/wm)

#### 格式化 namenode

在 hadoop 机器上使用如下命令进行格式化 namenode。

```bash
cd /app/hadoop-1.1.2/bin
./hadoop namenode -format
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433148120053/wm)

#### 启动 hadoop

```bash
./start-all.sh
```

![图片描述信息](https://doc.shiyanlou.com/userid29778labid764time1433148138993/wm)

#### 用 jps 检验各后台进程是否成功启动

使用 `jps` 命令查看 hadoop 相关进程是否启动。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid600404labid764timestamp1552011393120.png/wm)

> 注意：如果是会员，推荐把实验环境保存下来，后续的实验都会依赖于现在的环境；如果是非会员进行后续的实验，则每次开始实验都需要对环境进行配置。
