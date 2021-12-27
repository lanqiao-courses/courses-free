---
show: step
version: 1.0
enable_checker: true
---

# Hadoop 单机模式安装

## 一、实验介绍

#### 1.1 实验知识点

- 下载解压 / 环境变量配置
- Linux/shell
- 测试 WordCount 程序

#### 1.2 实验内容

- hadoop 三种安装模式介绍
- hadoop 单机模式安装
- 测试安装

#### 1.3 实验环境

- hadoop2.6.0
- Xfce 终端

#### 1.4 适合人群

本课程难度为一般，属于初级级别课程，适合具有 Linux 基础的用户。

## 二、Hadoop 启动模式

Hadoop 集群有三种启动模式：

- 单机模式：默认情况下运行为一个单独机器上的独立 Java 进程，主要用于调试环境
- 伪分布模式：在单个机器上模拟成分布式多节点环境，每一个 Hadoop 守护进程都作为一个独立的 Java 进程运行
- 完全分布式模式：真实的生产环境，搭建在完全分布式的集群环境

## 三、用户及用户组

需要先添加用来运行 Hadoop 进程的用户组 hadoop 及用户 hadoop。

**注意：实验楼环境里已经配置好 hadoop 用户，此步骤可以跳过。**

可以使用下面的命令来查看已经创建好的 hadoop 用户的 uid 与 gid。

```bash
$ id hadoop
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569207207167/wm)

另外，在 `/etc/passwd` 文件中也记录了用户的信息，使用下面的命令查看：

```bash
$ tail -5 /etc/passwd
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569207277451/wm)

添加用户及用户组的步骤如下，这里只是演示，**用户无需进行操作**。

创建用户 Hadoop

```bash
$ sudo adduser hadoop
```

并按照提示输入 hadoop 用户的密码，例如密码设定为 `hadoop`。注意输入密码的时候是不显示的。

将 hadoop 用户添加进 sudo 用户组

```bash
$ sudo usermod -G sudo hadoop
```

## 四、安装及配置依赖的软件包

Hadoop 的运行需要 JDK，同时还应配置 SSH 免密码登录。

本实验中，我们提供了 JDK 1.8 和 SSH 免密码登录环境，用户无需进行配置，但在文档中我们给出了配置的过程。

### 4.1 安装 jdk

`注意：实验楼环境里已经配置jdk环境变量，此步骤可以跳过。`

由于实验环境中已经配置好了 JDK，所以在这里，我们只是查看一下环境中提供的 JDK 信息。

使用下面的命令可以查看 Java 安装的路径：

```bash
$ echo $JAVA_HOME
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569207788803/wm)

使用下面的命令可以查看 Java 的版本信息：

```bash
$ java -version
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190928-1569652880753/wm)

### 4.2 配置 SSH 免密码登录

切换到 hadoop 用户，hadoop 用户时密码为 hadoop。后续步骤都将在 hadoop 用户的环境中执行。

```bash
# 切换为 hadoop 用户
$ su hadoop

# 密码为 hadoop
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569207894347/wm)

配置 ssh 环境免密码登录。**注意：实验楼环境里已经配置 ssh 环境免密码登录，此步骤可以跳过。**

在 `/home/hadoop` 目录下执行下面的命令

```bash
# 切换到根目录
$ cd ~

# 生成密钥对
$ ssh-keygen -t rsa

# 一路回车保持默认配置即可
```

对于秘钥对的设置，保持默认，等待执行完成后，秘钥对就生成好了（一般放置于 `~/.ssh/` 目录中

```bash
# 将公钥写入到验证文件中
$ cat .ssh/id_rsa.pub >> .ssh/authorized_keys

# 修改文件的权限为 600
$ chmod 600 .ssh/authorized_keys
```

验证登录本机是否还需要密码，第一次需要密码以后不需要密码就可以登录。

```bash
# 仅需输入一次 hadoop 密码，以后不需要输入
$ ssh localhost
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569208224242/wm)

> 这里命令行的提示信息（命令光标所在行的内容）没有变化，但是这里是登录成功的，可以输入 `exit` 命令，如果用户没有切换回 `shiyanlou` 用户，说明刚刚是使用 ssh 登录的用户。再次输入 `exit` 就退回到 `shiyanlou` 用户了。

## 五、下载并安装 Hadoop

注意，本部分的操作都是在 hadoop 用户登录的环境中进行的。

切换用户使用下面的命令。

```bash
# 切换为 hadoop 用户
$ su hadoop

# 密码为 hadoop
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569207894347/wm)

#### （1）下载 Hadoop 2.6.0

Hadoop 的下载比较缓慢，为了方便大家进行实验，我们在实验楼的服务器中提供了下载的文件，方便大家快速下载。

```bash
# 进入到家目录 /home/hadoop
$ cd ~

$ wget https://labfile.oss.aliyuncs.com/hadoop-2.6.0.tar.gz
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569208531191/wm)

```checker
- name: Download Hadoop
  script: |
    #!/bin/bash
    ls /home/hadoop/hadoop-2.6.0.tar.gz
  error: 您还没有下载 Hadoop 到 /home/hadoop/
```

#### （2）解压并安装

使用下面的命令解压下载的文件

```bash
$ tar zxvf hadoop-2.6.0.tar.gz
```

耐心等待解压完成：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569208608731/wm)

安装之前还需要删除之前的遗留文件，如果出现无此文件夹的提示，说明没有遗留文件。

```bash
# 删除原本目录中的 hdfs 文件夹
$ rm -r /home/hadoop/hdfs
```

然后再进行安装工作。

```bash
# 复制所需文件
$ mv hadoop-2.6.0 /home/hadoop/hdfs

# 将文件夹权限设置为 777
$ chmod 777 /home/hadoop/hdfs
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569208681675/wm)

```checker
- name: mkdir HDFS
  script: |
    #!/bin/bash
    ls -d /home/hadoop/hdfs/
  error: 您还没有创建文件夹 /home/hadoop/hdfs
```

#### （3）配置 Hadoop

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

环境中的 hive 以及 hbase 等环境本次实验不会用到可以删去，在所在行前加 `#` 即可注释（需要注释的内容在下面的图片中给出了。

在 `/home/hadoop/.bashrc` 文件中 PATH 路径更改 HADOOP 相关内容：

```bash
export PATH=/usr/local/sbin:/usr/local/bin/:/usr/bin:/usr/sbin:/sbin:/bin:/home/hadoop/hdfs/bin:/home/hadoop/hdfs/sbin
```

```checker
- name: Java Home
  script: |
    #!/bin/bash
    grep "/home/hadoop/hdfs/bin:/home/hadoop/hdfs/sbin" /home/hadoop/.bashrc
  error: 您还没有在 /home/hadoop/.bashrc 中配置 PATH
```

编辑完成后的文件内容如下：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190928-1569654365786/wm)

保存退出后，激活新加的环境变量。

```bash
$ source ~/.bashrc
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569208985638/wm)

至此，Hadoop 单机模式安装完成，可以通过下述步骤的测试来验证安装是否成功。

## 六、测试验证

创建输入的数据，暂时采用 `/etc/protocols` 文件作为测试

```bash
# 进入到 Hadoop 的目录
$ cd /home/hadoop/hdfs

# 新建一个 input 文件夹
$ mkdir input

# 复制文件到 input 中
$ cp /etc/protocols ./input/
```

```checker
- name: cp protocols
  script: |
    #!/bin/bash
    ls -l /home/hadoop/hdfs/input/protocols
  error: 您还没有复制测试文件 /home/hadoop/hdfs/input/protocols
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569209041541/wm)

执行 Hadoop WordCount 应用（词频统计）

```bash
$ hadoop jar \
  /home/hadoop/hdfs/share/hadoop/mapreduce/sources/hadoop-mapreduce-examples-2.6.0-sources.jar   \
  org.apache.hadoop.examples.WordCount input output
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569209558532/wm)

若以上语执行错误，可以尝试应用以下语句执行：

```bash
$ hadoop jar \
  /home/hadoop/hdfs/share/hadoop/mapreduce/sources/hadoop-mapreduce-examples-2.6.0-sources.jar   \
  wordcount input output
```

执行完成后：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569209485272/wm)

查看生成的单词统计数据

```bash
$ cat output/*
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190923-1569209470415/wm)

**注意**：如果要继续下一节 “伪分布式部署” 实验，请勿停止本实验环境，因为伪分布式部署模式需要在单机模式基础上进行配置。在实验的最后一步，直接点击右下角的 “进入下一章节” 即可。

## 七、小结

本实验中介绍了 Hadoop 单机模式的安装方法，并运行 wordcount 进行基本测试。

## 八、课后作业

请使用 hadoop 的 wordcount 对日志文件 `/var/log/dpkg.log` 进行词频统计。

实验中有任何问题欢迎到[实验楼问答](https://www.lanqiao.cn/questions)提问。

## 九、参考文档

本实验参考下列文档内容制作：

- http://hadoop.apache.org/docs/r2.6.0/hadoop-project-dist/hadoop-common/SingleCluster.html
- http://www.cnblogs.com/kinglau/p/3794433.html
