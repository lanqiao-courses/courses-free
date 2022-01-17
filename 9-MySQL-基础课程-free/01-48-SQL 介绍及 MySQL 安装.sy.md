---
show: step
version: 1.0
enable_checker: true
---

# SQL 的介绍及 MySQL 的安装

## 一、实验简介

本课程为实验楼提供的 MySQL 实验教程，所有的步骤都在实验楼在线实验环境中完成，学习中请按照实验步骤依次操作。

本课程为 SQL 基本语法及 MySQL 基本操作的实验，理论内容较少，动手实践多，可以快速上手 SQL 及 MySQL 服务。

#### 1.1 实验内容

本次课程对数据库、SQL、MySQL 做了简单介绍，并介绍了 Ubuntu Linux 下 MySQL 的安装。完成本实验，可以对这门课程和 MySQL 有了简单的了解，接下来的实验也将在此基础上进行。

#### 1.2 实验知识点

- 数据库的概念
- MySQL 的安装

#### 1.3 实验环境

课程使用的实验环境为 Ubuntu Linux 16.04 64 位版本。实验中会用到程序：

- Mysql 5.7.22
- Xfce 终端
- Gedit

#### 1.4 交流群

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid1735817-20220112-1641985643955) 


## 二、相关概念

#### 2.1 数据库和 SQL 概念

数据库（`Database`）是按照数据结构来组织、存储和管理数据的仓库，它的产生距今已有六十多年。随着信息技术和市场的发展，数据库变得无处不在：它在电子商务、银行系统等众多领域都被广泛使用，且成为其系统的重要组成部分。

数据库用于记录数据，使用数据库记录数据可以表现出各种数据间的联系，也可以很方便地对所记录的数据进行增、删、改、查等操作。

结构化查询语言(`Structured Query Language`)简称 SQL，是上世纪 70 年代由 IBM 公司开发，用于对数据库进行操作的语言。更详细地说，SQL 是一种数据库查询和程序设计语言，用于存取数据以及查询、更新和管理关系数据库系统，同时也是数据库脚本文件的扩展名。

#### 2.2 MySQL 介绍

MySQL 是一个 DBMS（数据库管理系统），由瑞典 MySQLAB 公司开发，目前属于 Oracle 公司，MySQL 是最流行的关系型数据库管理系统（关系数据库，是建立在关系数据库模型基础上的数据库，借助于集合代数等概念和方法来处理数据库中的数据）。由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，一般中小型网站的开发者都选择 MySQL 作为网站数据库。MySQL 使用 SQL 语言进行操作。

## 三、MySQL 安装

**注意：实验楼环境中已经安装好了 MySQL，可以直接使用，无需再次安装，以下安装仅用于大家学习使用。**

### 3.1 安装之前的检查

先要检查 Linux 系统中是否已经安装了 MySQL，输入命令尝试打开 MySQL 服务：

```bash
sudo service mysql start
```

输入密码后，如果出现以下提示，则说明系统中已经安装有 MySQL：

![1-01](https://doc.shiyanlou.com/MySQL/sql-01-01-.png/wm)

如果提示是这样的，则说明系统中没有 MySQL，需要继续安装：

```bash
mysql: unrecognized service
```

### 3.2 Ubuntu Linux 安装配置 MySQL

在 Ubuntu 上安装 MySQL，最简单的方式是在线安装。只需要几行简单的命令（ `#` 号后面是注释）：

```bash
#安装 MySQL 服务端、核心程序
sudo apt-get install mysql-server

#安装 MySQL 客户端
sudo apt-get install mysql-client
```

在安装过程中会提示确认输入 YES，设置 root 用户密码（之后也可以修改）等，稍等片刻便可安装成功。

安装结束后，用命令验证是否安装并启动成功：

```bash
sudo netstat -tap | grep mysql
```

如果出现如下提示，则安装成功：

![1-02](https://doc.shiyanlou.com/MySQL/sql-01-02.png/wm)

此时，可以根据自己的需求，用 gedit 修改 MySQL 的配置文件（my.cnf）,使用以下命令:

```bash
sudo gedit /etc/mysql/my.cnf
```

至此，MySQL 已经安装、配置完成，可以正常使用了。

### 3.3 尝试 MySQL

**1). 打开 MySQL：**

使用如下两条命令，打开 MySQL 服务并使用 root 用户登录：

```bash
# 启动 MySQL 服务
sudo service mysql start

# 使用 root 用户登录，实验楼环境的密码为空，直接回车就可以登录
mysql -u root
```

```checker
- name: check service
  script: |
    #!/bin/bash
    ps -ef|grep -v grep|grep mysql
  error:
    没有启动 mysql
```

执行成功会出现如下提示：

![1-03](https://doc.shiyanlou.com/MySQL/sql-01-03-.png/wm)

**2). 查看数据库：**

使用命令 `show databases;`，查看有哪些数据库（注意不要漏掉分号 `;`）：

![1-04](https://doc.shiyanlou.com/MySQL/sql-01-04.png/wm)

可见已有三个数据库，分别是 “information-schema”、“mysql”、“performance-schema”。

**3). 连接数据库：**

选择连接其中一个数据库，语句格式为 `use <数据库名>`，这里可以不用加分号，这里我们选择 `information_schema` 数据库：

```sql
use information_schema
```

![1-05](https://doc.shiyanlou.com/MySQL/sql-01-05.png/wm)

**4). 查看表：**

使用命令 `show tables;` 查看数据库中有哪些表（**注意不要漏掉“;”**）：

![1-06](https://doc.shiyanlou.com/MySQL/sql-01-06.png/wm)

**5). 退出：**

使用命令 `quit` 或者 `exit` 退出 MySQL。

## 四、实验总结

本节实验中我们初步接触了数据库，SQL 及 MySQL 的基本概念，实践了登录和退出 MySQL，使用和查看数据库等基本操作。

## 五、课后习题

1. 如果你的计算机操作系统或虚拟机中有 Ubuntu，尝试在 Ubuntu 中完成 MySQL 的安装、配置、试用。
2. 通过谷歌百度或其他方式，进一步了解数据库、SQL 和 MySQL。
