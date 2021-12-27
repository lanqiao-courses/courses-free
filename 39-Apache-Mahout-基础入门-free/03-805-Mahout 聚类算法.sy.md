# Mahout 聚类算法

## 一、实验介绍

### 1.1 实验内容

> - 本次课程学习 Mahout 的 K-Means 聚类算法

### 1.2 实验知识点

- K-Means 聚类算法的介绍与应用

### 1.3 实验环境

- Hadoop1.2.1
- Mahout0.9
- Xfce 终端

### 1.4 适合人群

本课程难度为一般，属于初级级别课程，适合具有 hadoop 基础的用户。

## 二、聚类算法

### 2.1 启动 Hadoop 服务

在上一个实验中主要介绍了 mahout 在 hadoop 单机模式下的安装配置，由于单机模式具体操作较简单，本实验主要介绍 mahout 在 hadoop 伪分布模式下的操作。

1. 在终端中输入`su - hadoop`命令切换到 hadoop 用户，密码为`hadoop`。
2. 切换至 hadoop 用户后，输入`ssh localhost`进行 SSH 登录（如有提示则输入 yes 进行确认）。
3. 修改 hadoop 配置文件`hdfs-site.xml`,`core-site.xml`,`mapred-site.xml`以及`hadoop-env.sh`(在`$HADOOP_HOME/conf`下)，具体修改可以参考[Hadoop 部署及管理](https://www.lanqiao.cn/courses/35)

- `hadoop-env.sh`仅修改 JAVA_HOME 的配置。
- `core-site.xml`这个文件的配置需要注意 1.2.1 版本和 2.4.1 版本的差别。`fs.default.name`与`fs.defaultFS`是不同的。

4. 输入命令`hadoop namenode -format`进行 NameNode 的格式化。
5. 输入命令`start-dfs.sh`以启动 hadoop 服务（如有提示则输入 yes 进行确认）。
6. 执行后续操作。

为了让大家明白什么是聚类算法，这里引用一个例子。

比如你是一个拥有众多藏书的图书馆馆长，但是图书馆里面的藏书全部都是混乱无序的。来到图书馆看书的读者如果要找一本书，则相当麻烦。如果所有的图书是按照书名首字母排序的，那么查找图书就会变得容易得多；或者你也可以按照图书的主题来分类。因此，你需要按照某种规则来把图书排成一列，当遇到与之前规则一样的图书，就可以把它们放在一起；当你遍历完所有读书时，众多的书籍已经被分成了若干类，一遍 `聚类` 也就完成了。

如果你觉得第一遍聚类的结果还不够精细，你还可以进行第二遍聚类，直到结果令人满意为止。

## 三、K-Means 聚类算法

K-Means 算法是 Mahout 中的一种聚类算法。它的应用很广泛，因此本次实验主要介绍 K-Means 聚类算法。其中，K 是聚类的数目，在算法中会要求用户输入。

K-Means 聚类算法主要可分为 3 步：

- 为 待聚类 的点寻找聚类中心；

- 计算每个点到聚类中心的距离，将每个点聚类到离改点最近的聚类中去；

- 计算聚类中所有点的坐标平均值，并将这个平均值作为新的聚类中心点。重复步骤 2，直到聚类中心不再进行大范围移动，或者聚类次数达到要求为止；

例如下图，解释了 K-Means 算法的第一轮：

![图片描述信息](https://doc.shiyanlou.com/userid46108labid805time1427859339111/wm)

> **1. 为 待聚类 的点寻找聚类中心；**
>
> _我们选择了 c1、c2、c3 作为聚类中心_

> **2. 计算每个点到聚类中心的距离，将每个点聚类到离该点最近的聚类中心去；**
>
> _在图 2，我们将每个点连线到了其距离最近的聚类中心，形成了 3 个新的聚类；_

> **3. 计算聚类中所有点的坐标平均值，并将这个平均值作为新的聚类中心点。重复步骤 2，直到聚类中心不再进行大范围移动，或者聚类次数达到要求为止；**
>
> _计算了每个聚类的平均值，即新的 c1、c2、c3，同时也被作为了新的聚类中心点；重复步骤 2 ..._

## 四、K-Means 算法应用实例

这里我们举一个简单的例子，来展现 K-Means 算法的应用。

本实验环境是伪分布模式或者分布模式，需在 hdfs 上新建一个目录:(如果 Hadoop 是单机模式的，现在只需在 `$HADOOP_HOME/bin` 下新建一个测试目录)

```bash
$ hadoop fs -mkdir -p /user/hadoop/testdata
```

（注意文件夹名只能是：`testdata`及文件夹路径不能变）

下载数据及上传数据（如果为单机模式，需要将该数据拷贝至 testdata 目录下）：

```bash
$ wget https://labfile.oss.aliyuncs.com/synthetic_control.data
$ hadoop fs -put synthetic_control.data /user/hadoop/testdata
```

运行 K-Means 算法（你必须先启动 Hadoop；注意根据你实际的 Mahout 路径来输入，`J` 为大写），可以看到类似下图的提示：

```bash
$ hadoop jar /usr/local/mahout-0.9/mahout-examples-0.9-job.jar org.apache.mahout.clustering.syntheticcontrol.kmeans.Job
```

![图片描述信息](https://doc.shiyanlou.com/userid46108labid805time1427882177723/wm)
_图示：K-Means 正在运行 Map-Reduce_

![图片描述信息](https://doc.shiyanlou.com/userid46108labid805time1427882241565/wm)
_图示：K-Means 正在运算中_

查看输出结果 output（它位于：hdfs 上的/user/hadoop/ ；单机模式位于`/usr/local/hadoop-1.2.1/bin/output`下）：

```bash
$ hadoop fs -ls /user/hadoop/output
```

> 单机模式为`ls /usr/local/hadoop-1.2.1/bin/output`

![图片描述信息](https://doc.shiyanlou.com/userid46108labid805time1427882379002/wm)

_图示：视图查看_

---

- ···/output/clusteredPoints 存放的是最后的聚类结果；查看 clusteredPoints 使用命令为：

```bash
$ mahout seqdumper -i /user/hadoop/output/clusteredPoints/part-m-00000
```

![图片描述信息](https://doc.shiyanlou.com/userid46108labid805time1427939325819/wm)
_图示：distance 为点到中心的距离；_

- ···/output/data 存放的是原始数据，这个文件夹下的数据可以用 Mahout Vectordump 来读取，因为原始数据都是向量形式的；查看 data 使用命令为：

```bash
$ mahout vectordump -i /user/hadoop/output/data/part-m-00000
```

![图片描述信息](https://doc.shiyanlou.com/userid46108labid805time1427882928611/wm)
_图示：原始数据_

- ···/output/clusters-n 为第 n 次迭代产生的结果集。可以看到 K-Means 算法进行了多次迭代（默认为 10 次迭代，所以最后应该是 clusters-10）。例如，查看 clusters-10：

```bash
$ mahout clusterdump -i /user/hadoop/output/clusters-10/part-r-00000
```

![图片描述信息](https://doc.shiyanlou.com/userid46108labid805time1427940754537/wm)
_图示：n=116，表示这一聚类有 116 个点；c 表示的是中心点的坐标；r 表示的是半径；_

## 五、课后习题

同学们不妨在单机模式下的 Hadoop 和 Mahout 中, 然后尝试运行 K-Means 算法加深对本实验的理解。

## 六、参考文档

> - 《Hadoop 实战 第 2 版》陆嘉恒，机械工业出版社；

- [Hadoop 家族安装系列(2)——安装 Mahout0.9 框架](http://gaoxianwei.iteye.com/blog/2028095)；

l
