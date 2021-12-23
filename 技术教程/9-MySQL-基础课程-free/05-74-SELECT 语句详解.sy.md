---
show: step
version: 1.0
enable_checker: true
---

# SELECT 语句详解

## 一、实验简介

#### 1.1 实验内容

SQL 中最常用的 SELECT 语句，用来在表中选取数据，本节实验中将通过一系列的动手操作详细学习 SELECT 语句的用法。

#### 1.2 实验知识点

- SELECT 基本语法
- 数学符号条件
- AND OR IN
- 通配符
- 排序
- SQL 内置函数和计算
- 子查询与连接查询

#### 1.3 实验环境

课程使用的实验环境为 Ubuntu Linux 16.04 64 位版本。实验中会用到程序：

- Mysql 5.7.22
- Xfce 终端

## 二、开发准备

> 注：如果你是从上一节直接进入本节进行学习的，请先删除上一节建立的数据库`mysql_shiyan`，删除语句为`DROP DATABASE mysql_shiyan;`。

在正式开始本实验内容之前，需要先下载相关数据库表，搭建好一个名为`mysql_shiyan` 的数据库(有三张表：department，employee，project)，并向其中插入数据。

具体操作如下,首先输入命令进入 `/home/shiyanlou/Desktop` 目录：

```bash
cd /home/shiyanlou/Desktop
```

然后再输入命令，下载代码：

```bash
wget https://labfile.oss.aliyuncs.com/courses/9/MySQL-04-01.sql
wget https://labfile.oss.aliyuncs.com/courses/9/MySQL-04-02.sql
```

下载完成后，输入命令开启 MySQL 服务并使用 root 用户登录：

```bash
# 打开 MySQL 服务
sudo service mysql start

# 使用 root 用户登录
mysql -u root
```

```checker
- name: check service
  script: |
    #!/bin/bash
    ps -ef | grep -v grep | grep mysql
  error:
    还没有启动 mysql
```

刚才下载的两个文件 `MySQL-04-01.sql` 和 `MySQL-04-02.sql`，其中第一个文件用于创建数据库，第二个文件向数据库中插入数据。

（**你可以用 gedit 查看两个文件的内容。**）

如果你是接着上一个实验操作的话，首先删除 mysql_shiyan 数据库：

```sql
drop database mysql_shiyan;
```

加载文件中的数据，需要在 MySQL 控制台中输入命令，搭建数据库：

```bash
source /home/shiyanlou/Desktop/MySQL-04-01.sql
```

向数据库插入数据：

```bash
source /home/shiyanlou/Desktop/MySQL-04-02.sql
```

## 三、实验步骤

在数据库操作语句中，使用最频繁，也被认为最重要的是 SELECT 查询语句。之前的实验中，我们已经在不少地方用到了 `SELECT * FROM table_name;` 这条语句用于查看一张表中的所有内容。
而 SELECT 与各种限制条件关键词搭配使用，具有各种丰富的功能，这次实验就进行详细介绍。

### 3.1 基本的 SELECT 语句

SELECT 语句的基本格式为：

```sql
SELECT 要查询的列名 FROM 表名字 WHERE 限制条件;
```

如果要查询表的所有内容，则把 **要查询的列名** 用一个星号 `*` 号表示(实验 2、3 中都已经使用过)，代表要查询表中所有的列。
而大多数情况，我们只需要查看某个表的指定的列，比如要查看 employee 表的 name 和 age：

```sql
SELECT name,age FROM employee;
```

![01](https://doc.shiyanlou.com/MySQL/sql-04-01.png)

### 3.2 数学符号条件

SELECT 语句常常会有 WHERE 限制条件，用于达到更加精确的查询。WHERE 限制条件可以有数学符号 (`=,<,>,>=,<=`) ，刚才我们查询了 name 和 age，现在稍作修改：

```sql
SELECT name,age FROM employee WHERE age>25;
```

筛选出 age 大于 25 的结果：

![02](https://doc.shiyanlou.com/MySQL/sql-04-02.png)

或者查找一个名字为 Mary 的员工的 name,age 和 phone：

```sql
SELECT name,age,phone FROM employee WHERE name='Mary';
```

结果当然是：

![03](https://doc.shiyanlou.com/MySQL/sql-04-03.png)

### 3.3 “AND”与“OR”

从这两个单词就能够理解它们的作用。WHERE 后面可以有不止一条限制，而根据条件之间的逻辑关系，可以用 [`条件一 OR 条件二`] 和 [`条件一 AND 条件二`] 连接：

例如，筛选出 age 小于 25，或 age 大于 30

```sql
SELECT name,age FROM employee WHERE age<25 OR age>30;
```

![04](https://doc.shiyanlou.com/MySQL/sql-04-04.png)

```sql
#筛选出 age 大于 25，且 age 小于 30
SELECT name,age FROM employee WHERE age>25 AND age<30;
```

![05](https://doc.shiyanlou.com/MySQL/sql-04-05.png)

而刚才的限制条件 **age>25 AND age<30** ，如果需要包含 25 和 30 这两个数字的话，可以替换为 **age BETWEEN 25 AND 30** ：

![06](https://doc.shiyanlou.com/MySQL/sql-04-06.png)

### 3.4 IN 和 NOT IN

关键词 **IN** 和 **NOT IN** 的作用和它们的名字一样明显，用于筛选**“在”**或**“不在”**某个范围内的结果，比如说我们要查询在 **dpt3** 或 **dpt4** 的人:

```sql
SELECT name,age,phone,in_dpt FROM employee WHERE in_dpt IN ('dpt3','dpt4');
```

![07](https://doc.shiyanlou.com/MySQL/sql-04-07.png)

而 **NOT IN** 的效果则是，如下面这条命令，查询出了不在 **dpt1** 也不在 **dpt3** 的人：

```sql
SELECT name,age,phone,in_dpt FROM employee WHERE in_dpt NOT IN ('dpt1','dpt3');
```

![08](https://doc.shiyanlou.com/MySQL/sql-04-08.png)

### 3.5 通配符

关键字 **LIKE** 可用于实现模糊查询，常见于搜索功能中。

和 LIKE 联用的通常还有通配符，代表未知字符。SQL 中的通配符是 `_` 和 `%` 。其中 `_` 代表一个未指定字符，`%` 代表**不定个**未指定字符

比如，要只记得电话号码前四位数为 1101，而后两位忘记了，则可以用两个 `_` 通配符代替：

```sql
SELECT name,age,phone FROM employee WHERE phone LIKE '1101__';
```

这样就查找出了 **1101 开头的 6 位数电话号码**：

![09](https://doc.shiyanlou.com/MySQL/sql-04-09.png)

另一种情况，比如只记名字的首字母，又不知道名字长度，则用 `%` 通配符代替不定个字符：

```sql
SELECT name,age,phone FROM employee WHERE name LIKE 'J%';
```

这样就查找出了首字母为 **J** 的人：

![10](https://doc.shiyanlou.com/MySQL/sql-04-10.png)

### 3.6 对结果排序

为了使查询结果看起来更顺眼，我们可能需要对结果按某一列来排序，这就要用到 **ORDER BY** 排序关键词。默认情况下，**ORDER BY** 的结果是**升序**排列，而使用关键词 **ASC** 和 **DESC** 可指定**升序**或**降序**排序。
比如，我们**按 salary 降序排列**，SQL 语句为：

```sql
SELECT name,age,salary,phone FROM employee ORDER BY salary DESC;
```

![11](https://doc.shiyanlou.com/MySQL/sql-04-11.png)

如果后面不加 DESC 或 ASC 将默认按照升序排列。应用场景：博客系统中按时间先后顺序显示博文。

### 3.7 SQL 内置函数和计算

SQL 允许对表中的数据进行计算。对此，SQL 有 5 个内置函数，这些函数都对 SELECT 的结果做操作：

| 函数名： | COUNT | SUM  | AVG      | MAX    | MIN    |
| -------- | ----- | ---- | -------- | ------ | ------ |
| 作用：   | 计数  | 求和 | 求平均值 | 最大值 | 最小值 |

> 其中 COUNT 函数可用于任何数据类型(因为它只是计数)，而 SUM 、AVG 函数都只能对数字类数据类型做计算，MAX 和 MIN 可用于数值、字符串或是日期时间数据类型。

具体举例，比如计算出 salary 的最大、最小值，用这样的一条语句：

```sql
SELECT MAX(salary) AS max_salary,MIN(salary) FROM employee;
```

有一个细节你或许注意到了，**使用 AS 关键词可以给值重命名**，比如最大值被命名为了 max_salary：

![12](https://doc.shiyanlou.com/MySQL/sql-04-12.png)

### 3.8 子查询

上面讨论的 SELECT 语句都仅涉及一个表中的数据，然而有时必须处理多个表才能获得所需的信息。例如：想要知道名为 "Tom" 的员工所在部门做了几个工程。员工信息储存在 employee 表中，但工程信息储存在 project 表中。

对于这样的情况，我们可以用子查询：

```sql
SELECT of_dpt,COUNT(proj_name) AS count_project FROM project GROUP BY of_dpt
HAVING of_dpt IN
(SELECT in_dpt FROM employee WHERE name='Tom');
```

上面代码包含两个 SELECT 语句，第二个 SELECT 语句将返回一个集合的数据形式，然后被第一个 SELECT 语句用 **in** 进行判断。

HAVING 关键字可以的作用和 WHERE 是一样的，都是说明接下来要进行条件筛选操作。

区别在于 HAVING 用于对分组后的数据进行筛选

![13](https://doc.shiyanlou.com/document-uid600404labid74timestamp1529491223362.png)

> 子查询还可以扩展到 3 层、4 层或更多层。

### 3.9 连接查询

在处理多个表时，子查询只有在结果来自一个表时才有用。但如果需要显示两个表或多个表中的数据，这时就必须使用连接 **(join)** 操作。
连接的基本思想是把两个或多个表当作一个新的表来操作，如下：

```sql
SELECT id,name,people_num
FROM employee,department
WHERE employee.in_dpt = department.dpt_name
ORDER BY id;
```

这条语句查询出的是，各员工所在部门的人数，其中员工的 id 和 name 来自 employee 表，people_num 来自 department 表：

![14](https://doc.shiyanlou.com/MySQL/sql-04-14.png)

另一个连接语句格式是使用 JOIN ON 语法，刚才的语句等同于：

```sql
SELECT id,name,people_num
FROM employee JOIN department
ON employee.in_dpt = department.dpt_name
ORDER BY id;
```

结果也与刚才的语句相同。

## 四、实验总结

本节实验中学习了 SELECT 语句的常用方法：

- 基本语法
- 数学符号条件
- AND OR IN
- 模糊查询
- 对查询结果排序
- SQL 内置函数和计算
- 子查询与连接查询

## 五、课后习题

使用连接查询的方式，查询出各员工所在部门的人数与工程数，工程数命名为 `count_project`。（连接 3 个表，并使用 `COUNT` 内置函数）

### 练习题参考答案

以下提示仅供参考，你可以按自己的想法来完成：

```sql
SELECT name, people_num, COUNT(proj_name) AS count_project
  FROM employee, department, project
  WHERE in_dpt = dpt_name AND of_dpt = dpt_name
  GROUP BY name, people_num;
```

```bash
+------+------------+---------------+
| name | people_num | count_project |
+------+------------+---------------+
| Alex |         11 |             1 |
| Jack |         12 |             2 |
| Jim  |         11 |             1 |
| Jobs |         12 |             2 |
| Joe  |         12 |             2 |
| Ken  |         11 |             1 |
| Mary |         12 |             2 |
| Mike |         15 |             2 |
| Rick |         10 |             1 |
| Rose |         10 |             1 |
| Tom  |         15 |             2 |
| Tony |         10 |             1 |
+------+------------+---------------+
12 rows in set (0.01 sec)
```

前两行不必说了，从三张表选择三列数据；第三行设置三列数据的关系，`dpt_name` 是唯一值，`of_dpt` 和 `in_dpt` 的值必取自 `dpt_name` 这列，它们应该有外键关系，但不是必须的；第四行分组，分组不可或缺，否则会出现无意义的数据（一般来说连接查询语句中有 `COUNT` 就会有 `GROUP BY`），报一个 `sql_mode` 引起的错误，至于为什么选择 `name` 和 `people_num` 分组，大家可以试一下同时去掉 `COUNT` 和 `GROUP BY` 语句，看看其中的差异即可理解。
