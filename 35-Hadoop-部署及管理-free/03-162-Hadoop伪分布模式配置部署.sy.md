---
show: step
version: 1.0
enable_checker: true
---

# Hadoop 伪分布模式配置部署

## 一、实验介绍

#### 1.1 实验知识点

- hadoop 核心配置文件
- 文件系统的格式化
- 测试 WordCount 程序

#### 1.2 实验内容

- hadoop 配置文件介绍及修改
- hdfs 格式化
- 启动 hadoop 进程，验证安装

#### 1.3 实验环境

- hadoop2.6.0
- Xfce 终端

#### 1.4 适合人群

本课程难度为一般，属于初级级别课程，适合具有 Linux 基础的用户。

## 二、Hadoop 伪分布式模式配置

！！！注意：本实验需要按照上一节单机模式部署后继续进行操作，因此您必须先完成上一节实验。

本部分主要讲解相关配置文件修改（若文件中没有添加的配置项，则系统为默认值，不会对该实验产生影响）

后续步骤都将在 hadoop 用户的环境中执行。切换到 hadoop 用户，hadoop 用户时密码为 hadoop。

```bash
# 切换为 hadoop 用户
$ su hadoop

# 密码为 hadoop
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569207894347/wm)

### 2.1 修改 `.bashrc`

**再次提醒**：本实验需要按照上一节单机模式部署后继续进行操作，因此您必须先完成上一节实验。上一节实验主要包括了：安装 Hadoop、配置环境变量。

本部分讲解 `.bashrc` 的配置，这部分内容在上一节实验中已经讲解过，这里只是为了让实验文档的连贯性更强，用户无需进行操作。

由于平台环境与该实验 hadoop 版本不匹配问题，需要对`.bashrc`文件中末尾处的环境变量做修改

```bash
$ vim /home/hadoop/.bashrc
```

在 `/home/hadoop/.bashrc` 文件末尾添加下列内容：

> 下面的配置中以 `#` 开头的是注释，无需输入。

```bash
#HADOOP START
export HADOOP_HOME=/home/hadoop/hdfs
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
#HADOOP END
```

环境中的 hive 以及 hbase 等环境本次实验不会用到可以删去，在所在行前加 `#` 即可注释（需要注释的内容在下面的图片中给出了。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569208793422/wm)

在 `/home/hadoop/.bashrc` 文件中 PATH 路径更改 HADOOP 相关内容：

```bash
export PATH=/usr/local/sbin:/usr/local/bin/:/usr/bin:/usr/sbin:/sbin:/bin:/home/hadoop/hdfs/bin:/home/hadoop/hdfs/sbin
```

编辑完成后的文件内容如下：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190928-1569654365786/wm)

**提醒**：修改了配置文件后，需要使变量生效。

```bash
$ source ~/.bashrc
```

```checker
- name: Install Hadoop
  script: |
    #!/bin/bash
    ls /home/hadoop/hdfs/
  error: 您还没有复制 Hadoop 到 /home/hadoop/hdfs，请参考上一节的内容进行
```

```checker
- name: Hadoop Home
  script: |
    #!/bin/bash
    grep "HADOOP_HOME=/home/hadoop/hdfs" /home/hadoop/.bashrc
  error: 您还没有在 /home/hadoop/.bashrc 中配置 HADOOP_HOME
```

```checker
- name: Java Home
  script: |
    #!/bin/bash
    grep "JAVA_HOME=/usr/lib/jvm/java-8-oracle" /home/hadoop/.bashrc
  error: 您还没有在 /home/hadoop/.bashrc 中配置 JAVA_HOME
```

```checker
- name: Java Home
  script: |
    #!/bin/bash
    grep "/home/hadoop/hdfs/bin:/home/hadoop/hdfs/sbin" /home/hadoop/.bashrc
  error: 您还没有在 /home/hadoop/.bashrc 中配置 PATH
```

### 2.2 修改`core-site.xml`

这里我们要修改的是 `core-site.xml`，使用下面的命令调用 Vim 编辑器来编辑文件。

```bash
$ cd /home/hadoop/hdfs/etc/hadoop

$ vim ./core-site.xml
```

先说明一下常用配置项：

- `fs.defaultFS`

`fs.defaultFS` 是默认的 HDFS 路径。当有多个 HDFS 集群同时工作时，用户在这里指定默认 HDFS 集群，该值来自于 `hdfs-site.xml` 中的配置。

- `fs.default.name`

`fs.default.name` 是一个描述集群中 NameNode 结点的 URI(包括协议、主机名称、端口号)，集群里面的每一台机器都需要知道 NameNode 的地址。DataNode 结点会先在 NameNode 上注册，这样它们的数据才可以被使用。独立的客户端程序通过这个 URI 跟 DataNode 交互，以取得文件的块列表。

- `hadoop.tmp.dir`

`hadoop.tmp.dir` 是 hadoop 文件系统依赖的基础配置，很多路径都依赖它。如果`hdfs-site.xml` 中不配置 namenode 和 datanode 的存放位置，默认就放在 `/tmp/hadoop-${user.name}` 这个路径中。

更多说明请参考[core-default.xml](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/core-default.xml)，包含配置文件所有配置项的说明和默认值。

配置好以后的文件内容如下：

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!-- Put site-specific property overrides in this file. -->

<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/hadoop/tmp</value>
   </property>
</configuration>
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569216878692/wm)

```checker
- name: core-site conf
  script: |
    #!/bin/bash
    grep "/home/hadoop/tmp" /home/hadoop/hdfs/etc/hadoop/core-site.xml
  error: 您还没有在 core-site.xml 中进行配置
```

### 2.3 修改`hdfs-site.xml`

这里我们要修改的是 `hdfs-site.xml`，使用下面的命令调用 Vim 编辑器来编辑文件。

```bash
$ cd /home/hadoop/hdfs/etc/hadoop/

$ vim ./hdfs-site.xml
```

常用配置项说明：

- `dfs.replication`

`dfs.replication` 决定着系统里面的文件块的数据备份个数。对于一个实际的应用，它应该被设为 3（这个数字并没有上限，但更多的备份可能并没有作用，而且会占用更多的空间）。少于三个的备份，可能会影响到数据的可靠性（系统故障时，也许会造成数据丢失）

- `dfs.data.dir`

`dfs.data.dir` 这是 DataNode 结点被指定要存储数据的本地文件系统路径。DataNode 结点上的这个路径没有必要完全相同，因为每台机器的环境很可能是不一样的。但如果每台机器上的这个路径都是统一配置的话，会使工作变得简单一些。默认的情况下，它的值为`file://${hadoop.tmp.dir}/dfs/data`这个路径只能用于测试的目的，因为它很可能会丢失掉一些数据。所以这个值最好还是被覆盖。

- `dfs.name.dir`

`dfs.name.dir` 是 NameNode 结点存储 hadoop 文件系统信息的本地系统路径。这个值只对 NameNode 有效，DataNode 并不需要使用到它。上面对于 `/temp` 类型的警告，同样也适用于这里。在实际应用中，它最好被覆盖掉。

更多说明请参考[hdfs-default.xml](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml)，包含配置文件所有配置项的说明和默认值。

配置好以后的文件内容如下：

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!-- Put site-specific property overrides in this file. -->

<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569217071454/wm)

```checker
- name: hdfs-site conf
  script: |
    #!/bin/bash
    grep "dfs.replication" /home/hadoop/hdfs/etc/hadoop/hdfs-site.xml
  error: 您还没有在 hdfs-site.xml 中进行配置
```

### 2.4 修改`mapred-site.xml`

使用下面的命令先将默认文件复制一份过来，然后调用 Vim 编辑器进行编辑。

```bash
$ cd /home/hadoop/hdfs/etc/hadoop/

$ cp ./mapred-site.xml.template ./mapred-site.xml

$ vim ./mapred-site.xml
```

常用配置项说明：

- `mapred.job.tracker`：JobTracker 的主机（或者 IP）和端口。

更多说明请参考[mapred-default.xml](http://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml)，包含配置文件所有配置项的说明和默认值

配置好以后的文件内容如下：

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!-- Put site-specific property overrides in this file. -->

<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569217213654/wm)

```checker
- name: mapred-site conf
  script: |
    #!/bin/bash
    grep "mapreduce.framework.name" /home/hadoop/hdfs/etc/hadoop/mapred-site.xml
  error: 您还没有在 mapred-site.xml 中进行配置
```

### 2.5 修改`yarn-site.xml`

编辑 `yarn-site.xml` 文件。

```bash
$ cd /home/hadoop/hdfs/etc/hadoop

$ vim ./yarn-site.xml
```

常用配置项说明：

- `yarn.nodemanager.aux-services`通过该配置，用户可以自定义一些服务

更多说明请参考[yarn-default.xml](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-common/yarn-default.xml)，包含配置文件所有配置项的说明和默认值

配置好以后的文件内容如下：

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>

</configuration>
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569217329091/wm)

```checker
- name: yarn-site conf
  script: |
    #!/bin/bash
    grep "yarn.nodemanager.aux-services" /home/hadoop/hdfs/etc/hadoop/yarn-site.xml
  error: 您还没有在 yarn-site.xml 中进行配置
```

### 2.6 修改 `hadoop-env.sh`

```bash
$ cd /home/hadoop/hdfs/etc/hadoop/

$ sudo vim ./hadoop-env.sh
```

修改 JAVA_HOME 如下：

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
export HADOOP_CONF_DIR=/home/hadoop/hdfs/etc/hadoop
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569217490884/wm)

这样简单的伪分布式模式就配置好了。

```checker
- name: hadoop-env.sh conf
  script: |
    #!/bin/bash
    grep "/usr/lib/jvm/java-8-oracle" /home/hadoop/hdfs/etc/hadoop/hadoop-env.sh
  error: 您还没有在 hadoop-env.sh 中进行配置
```

## 三、格式化 HDFS 文件系统

在使用 hadoop 前，必须格式化一个全新的 HDFS 安装，通过创建存储目录和 NameNode 持久化数据结构的初始版本，格式化过程创建了一个空的文件系统。

由于 NameNode 管理文件系统的元数据，而 DataNode 可以动态的加入或离开集群，因此这个格式化过程并不涉及 DataNode。同理，用户也无需关注文件系统的规模。

集群中 DataNode 的数量决定着文件系统的规模。DataNode 可以在文件系统格式化之后的很长一段时间内按需增加。

使用下面的命令进行给格式化。

```bash
$ cd ~

$ hadoop namenode -format
```

会输出如下信息，则表格式化 HDFS 成功：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569217694370/wm)

命令的末尾出现下图的结果，说明格式化成功：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569217771818/wm)

## 四、Hadoop 集群启动

#### （1）启动 hdfs 守护进程

启动 HDFS 守护进程：分别启动 NameNode 和 DataNode

```bash
$ start-dfs.sh
```

输出如下（可以看出分别启动了 namenode, datanode, secondarynamenode，因为我们没有配置 secondarynamenode，所以地址为 0.0.0.0）：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569218077964/wm)

#### （2）启动 yarn

使用如下命令启动 ResourceManager 和 NodeManager:

```bash
$ start-yarn.sh
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569218129823/wm)

#### （3）检查是否运行成功

打开浏览器

- 输入：`http://localhost:8088`进入 ResourceManager 管理页面

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569218240150/wm)

- 输入：`http://localhost:50070`进入 HDFS 页面

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569218284811/wm)

可能出现的问题及调试方法：

启动伪分布后，如果活跃节点显示为零，说明伪分布没有真正的启动。

原因是有的时候数据结构出现问题会造成无法启动 datanode。如果使用`hadoop namenode -format`重新格式化仍然无法正常启动，原因是`/tmp`中的文件没有清除，则需要先清除`/tmp/hadoop/*`再执行格式化，即可解决 hadoop datanode 无法启动的问题。

具体步骤如下所示：

```bash
# 删除 hadoop:/tmp
$ hadoop fs -rmr /tmp

# 停止 hadoop
$ stop-all.sh

# 删除 /tmp/hadoop*
$ rm -rf /tmp/hadoop*

# 格式化
$ hadoop namenode -format

# 启动 hadoop
$ start-all.sh
```

## 五、测试验证

测试验证还是使用上一节的 WordCount。

不同的是，这次是伪分布模式，使用到了 hdfs，因此我们需要把文件拷贝到 hdfs 上去。

首先创建相关文件夹：

```bash
$ hdfs dfs -mkdir -p /user/hadoop/input
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569218458551/wm)

#### （1）创建输入的数据

这里我们采用 `/etc/protocols` 文件作为输入的数据进行测试。先将文件拷贝到 hdfs 上：

```bash
# 上传
$ hdfs dfs -put /etc/protocols /user/hadoop/input

# 查看是否上传成功
$ hdfs dfs -ls /user/hadoop/input
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569218577999/wm)

#### （2）执行 Hadoop WordCount 应用（词频统计）

如果存在上一次测试生成的 output，由于 hadoop 的安全机制，直接运行可能会报错，所以请手动删除上一次生成的 output 文件夹。

```bash
$ rm -rf /home/hadoop/hdfs/output/
```

然后运行词频统计程序。

```bash
$ hadoop jar   \
  /home/hadoop/hdfs/share/hadoop/mapreduce/sources/hadoop-mapreduce-examples-2.6.0-sources.jar   \
  org.apache.hadoop.examples.WordCount   \
  /user/hadoop/input output
```

执行过程截图（部分）：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569218769021/wm)

#### （3）查看生成的单词统计数据

耐心等待前面的词频统计命令结束后，输入下面的命令查看结果：

```bash
$ hdfs dfs -cat /user/hadoop/output/*
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569218872531/wm)

## 六、关闭服务

- 关闭 HDFS 守护进程

```bash
$ stop-dfs.sh
```

- 关闭 yarn

```bash
$ stop-yarn.sh
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569218972695/wm)

最后一步：点击屏幕上方的 “实验截图”将上述命令执行后的截图保存并分享给朋友们吧，这是你学习 Hadoop 安装的证明。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569219020294/wm)

## 七、小结

本实验讲解如何在单机模式下继续部署 Hadoop 伪分布模式。

## 八、思考题

伪分布模式和单机模式配置上的区别主要是哪些？是否可以推论出如何部署真实的分布式 Hadoop 环境？

实验中有任何问题欢迎到[实验楼问答](https://www.lanqiao.cn/questions)提问。

## 九、参考文档

本实验参考下列文档内容制作：

- http://www.cnblogs.com/kinglau/p/3796164.html
