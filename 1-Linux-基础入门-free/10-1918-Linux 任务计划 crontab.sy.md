---
show: step
version: 1.0
enable_checker: true
---

# Linux 任务计划 crontab

## 一、实验介绍

#### 1.1 实验内容

我们时常会有一些定期定时的任务，如周期性的清理一下 `/tmp`，周期性的去备份一次数据库，周期性的分析日志等等。而且有时候因为某些因素的限制，执行该任务的时间会很尴尬。本课程将带你很好的利用 Linux 系统的计划工具。

#### 1.2 实验知识点

- crontab 语法

#### 1.3 参考资料

本节部分内容参考以下文档制作：

- [例行性工作排程(crontab) - 鸟哥私房菜](http://linux.vbird.org/linux_basic/0430cron.php)
- [Linux Crontab 百度百科](http://baike.baidu.com/link?url=h_n27RWno87VR4N0xoTGQfcnhoWXH_5JtLNEjuTqisXIasgLiUbWWjJwIkc3vnEIJrCrlZ1oQ4q3xrTz8VRtd_)

## 二、crontab 的使用

crontab 命令常见于 Unix 和类 Unix 的操作系统之中（Linux 就属于类 Unix 操作系统），用于设置周期性被执行的指令。

### 2.1 crontab 简介

crontab 命令从输入设备读取指令，并将其存放于 crontab 文件中，以供之后读取和执行。通常，crontab 储存的指令被守护进程激活，crond 为其守护进程，crond 常常在后台运行，每一分钟会检查一次是否有预定的作业需要执行。

通过 crontab 命令，我们可以在固定的间隔时间执行指定的系统指令或 shell 脚本。时间间隔的单位可以是分钟、小时、日、月、周的任意组合。

这里我们看一看 crontab 的格式：

```bash
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
```

### 2.2 crontab 准备

crontab 在本实验环境中需要做一些特殊的准备，首先我们会启动 `rsyslog`，以便我们可以通过日志中的信息来了解我们的任务是否真正的被执行了（在本实验环境中需要手动启动，而在自己本地中 Ubuntu 会默认自行启动不需要手动启动）。

```bash
sudo apt-get install -y rsyslog
sudo service rsyslog start
```

![service-rsyslog-start](https://dn-simplecloud.shiyanlou.com/1135081468201394787-wm)

在本实验环境中 crontab 也是不被默认启动的，同时不能在后台由 upstart 来管理，所以需要我们来启动它:

```bash
sudo cron －f &
```

```checker
- name: check cron
  script: |
    #!/bin/bash
    ps -ef |grep -v grep|grep cron
  error: 没有启动 crontab
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid600404labid6124timestamp1523941816405.png/wm)

### 2.3 crontab 使用

下面将开始 crontab 的使用了，我们通过下面一个命令来添加一个计划任务：

```bash
crontab -e
```

第一次启动会出现这样一个画面，这是让我们选择编辑的工具，选择第二个基本的 vim 就可以了。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid600404labid6124timestamp1523941985569.png/wm)

而选择后我们会进入这样一个画面，这就是添加计划的地方了，与一般的配置文档相同，以#号开头的都是注释，通过文档的最后一排我们可以猜猜 crontab 的格式是什么样的呢？

![实验楼](https://dn-simplecloud.shiyanlou.com/1135081468202029108-wm)

详细的格式可以使用上一节中学习到的 man 命令查看：

```bash
man crontab
```

在了解命令格式之后，我们通过这样的一个例子来完成一个任务的添加，在文档的最后一排加上这样一排命令，该任务是每分钟我们会在/home/shiyanlou 目录下创建一个以当前的年月日时分秒为名字的空白文件

```bash
*/1 * * * * touch /home/shiyanlou/$(date +\%Y\%m\%d\%H\%M\%S)
```

**注意：**

> “ % ” 在 crontab 文件中，有结束命令行、换行、重定向的作用，前面加 ” \ ” 符号转义，否则，“ % ” 符号将执行其结束命令行或者换行的作用，并且其后的内容会被做为标准输入发送给前面的命令。

添加成功后我们会得到最后一排 installing new crontab 的一个提示：

![实验楼](https://dn-simplecloud.shiyanlou.com/1135081468203483143-wm)

当然我们也可以通过这样的一个指令来查看我们添加了哪些任务：

```bash
crontab -l
```

通过图中的显示，我们也可以看出，我们正确的保存并且添加成功了该任务的：

![实验楼](https://dn-simplecloud.shiyanlou.com/1135081468204230683-wm)

虽然我们添加了任务，但是如果 `cron` 的守护进程并没有启动，它根本都不会监测到有任务，当然也就不会帮我们执行，我们可以通过以下 2 种方式来确定我们的 `cron` 是否成功的在后台启动，默默的帮我们做事，若是没有就得执行上文准备中的第二步了。

```bash
ps aux | grep cron

# or

pgrep cron
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid600404labid6124timestamp1523942683532.png/wm)

通过下图可以看到任务在创建之后，执行了几次，生成了一些文件，且每分钟生成一个：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid600404labid6124timestamp1523943532369.png/wm)

我们通过这样一个命令可以查看到执行任务命令之后在日志中的信息反馈：

```bash
sudo tail -f /var/log/syslog
```

从图中我们可以看到分别在 13 点 28、29、30 分的 01 秒为我们在 shiyanlou 用户的家目录下创建了文件。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid600404labid6124timestamp1523943327065.png/wm)

当我们并不需要这个任务的时候我们可以使用这么一个命令去删除任务：

```bash
crontab -r
```

通过图中我们可以看出我们删除之后再查看任务列表，系统已经显示该用户并没有任务哦。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid600404labid6124timestamp1523943647348.png/wm)

### 三、crontab 的深入

每个用户使用 `crontab -e` 添加计划任务，都会在 `/var/spool/cron/crontabs` 中添加一个该用户自己的任务文档，这样目的是为了隔离。

![实验楼](https://dn-simplecloud.shiyanlou.com/1135081468206283987-wm)

如果是系统级别的定时任务，需要 root 权限执行的任务应该怎么处理？

只需要使用 `sudo` 编辑 `/etc/crontab` 文件就可以。

`cron` 服务监测时间最小单位是分钟，所以 `cron` 会每分钟去读取一次 `/etc/crontab` 与 `/var/spool/cron/crontabs` 里面的內容。

在 `/etc` 目录下，`cron` 相关的目录有下面几个：

![实验楼](https://dn-simplecloud.shiyanlou.com/1135081468206856712)

每个目录的作用：

1. `/etc/cron.daily`，目录下的脚本会每天执行一次，在每天的 6 点 25 分时运行；
2. `/etc/cron.hourly`，目录下的脚本会每个小时执行一次，在每小时的 17 分钟时运行；
3. `/etc/cron.monthly`，目录下的脚本会每月执行一次，在每月 1 号的 6 点 52 分时运行；
4. `/etc/cron.weekly`，目录下的脚本会每周执行一次，在每周第七天的 6 点 47 分时运行；

系统默认执行时间可以根据需求进行修改。

### 进一步学习参考

本章部分描述参考下面的文档实现，想进一步的学习该知识或者相关知识推荐查看：

- [例行性工作排程(crontab) - 鸟哥私房菜](http://linux.vbird.org/linux_basic/0430cron.php)

## 四、实验总结

本节我们讲解了 crontab 的一些简单的应用和一些简单的概念。crontab 是 Linux 系统中添加计划任务，定时执行一些必要的脚本所必不可少的工具。