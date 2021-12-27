# Mahout 安装配置

## 一、实验介绍

### 1.1 实验内容

> - Mahout 的安装与配置

### 1.2 实验知识点

- 下载 Mahout 并配置
- Hadoop 与 Mahout 的启动

### 1.3 实验环境

- Hadoop1.2.1
- Mahout0.9
- Xfce 终端

### 1.4 适合人群

本课程难度为一般，属于初级级别课程，适合具有 hadoop 基础的用户。

## 二、Mahout 与 Hadoop

> - 说明：Mahout 官网目前最新的已编译版本为 0.9，此版本只支持 Hadoop V1.x.y 版本；如果要使用 Hadoop V2.0.0 及其以上版本，需下载最新的 Mahout 源码自行编译。这里为了大家实验的方便，我们已经为大家重新安装了 Hadoop V1.2.1 版本，你只需要下载 Mahout 0.9 解压安装即可。

从 Mahout 0.9 源码中的 pom.xml 文件中，可以发现其支持的 Hadoop 版本：

![图片描述信息](https://doc.shiyanlou.com/userid46108labid786time1427783521921/wm)

最新的 Mahout 源码地址，请点击 [这里](https://github.com/apache/mahout)。同样可以看到，已经支持 Hadoop V2.0.0 以上版本了。

![图片描述信息](https://doc.shiyanlou.com/userid46108labid786time1427783899235/wm)

## 2.1 启动 Hadoop 服务

实验需要在 hadoop 用户的环境下执行，按照下面的步骤切换到 hadoop 用户并启动 hadoop 服务。

1. 在终端中输入`su - hadoop`命令切换到 hadoop 用户，密码为`hadoop`。
2. 切换至 hadoop 用户后，输入`ssh localhost`进行 SSH 登录（如有提示则输入 yes 进行确认）。
3. 配置 hadoop1.2.1 环境变量

```bash
$ vi ~/.bashrc
```

原环境末尾内容为：

```bash
export HADOOP_HOME=/opt/hadoop-2.4.1
export HBASE_HOME=/opt/hbase-1.1.5
export HBASE_HOME=/opt/hbase-1.1.5
export HIVE_HOME=/opt/apache-hive-2.0.0-bin/
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/hadoop-2.4.1/bin:/opt/hadoop-2.4.1/sbin:/opt/hbase-1.1.5/bin:/opt/apache-hive-2.0.0-bin/bin
```

更改为：

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
export HADOOP_HOME=/opt/hadoop-1.2.1
export HADOOP_HOME_WARN_SUPPRESS=1
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```

更改完后执行`source ~/.bashrc`使得环境变量生效。

压缩包在/opt 目录下（已解压）

## 三、下载 Mahout

为了方便大家实验，本实验环境集成 Hadoop V1.2.1 （单机模式），且将其设为了普通权限，无需切换到 Hadoop 用户目录。

**注意：**非实验楼会员无法连接网络，所以不需要下载，实验楼已经将 mahout 和 hadoop 等组件内置在实验环境中的 `/opt` 目录下，可以切换过去拷贝文件夹到 `/usr/local` 目录。

```bash
$ sudo cp -a /opt/mahout-distribution-0.9 /usr/local/mahout-0.9
```

**注意：** 如果提示权限不够，请 sudo

## 四、配置环境变量

修改`.bashrc`文件，这个文件我们在前面的实验中已经修改过多次了，相信大家已经很熟悉了。如果你不熟悉 Vim 的使用，可以点击[Vim 编辑器](https://www.lanqiao.cn/courses/2) 课程回顾。

```bash
$ vim ~/.bashrc
```

添加如下环境变量（HADOOP_HOME 是实验环境集成的，大家路径都一致；但 MAHOUT_HOME 需要根据你自己实际解压之后存放的路径来填写）：

```bash
export MAHOUT_HOME=/usr/local/mahout-0.9
export CLASSPATH=.:JAVA_HOME/lib/dt.jar:JAVA_HOME/lib/tools.jar:$CLASSPATH
export PATH=$MAHOUT_HOME/bin    # 在 PATH 末尾处添加即可
```

保存刚刚的修改，并使之生效：

```bash
$ source ~/.bashrc
```

## 五、启动 Mahout

确认上面的步骤我们都没有问题后，就可以验证 Mahout 是否安装成功，在 `$MAHOUT_HOME/bin` 路径下输入以下代码，就会输出 Mahout 所支持的命令：

```bash
$ mahout -help
```

![图片描述信息](https://doc.shiyanlou.com/userid46108labid786time1427858658194/wm)

看到类似上图的信息，就说明 Mahout 安装成功了。

## 六、课后习题

根据我们提供的信息和地址，同学们可以试试自行下载 Mahout 最新的源码来编译安装。

## 七、参考文档

> - 《Hadoop 实战 第 2 版》陆嘉恒，机械工业出版社；

- [Mahout 运行在 Hadoop 2.x 上兼容性问题的解决](http://f.dataguru.cn/thread-381702-1-1.html)；
