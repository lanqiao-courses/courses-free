---
show: step
version: 1.0
enable_checker: true
---

# 字典类型

## 回忆

- 上次学习了集合的运算
- 集合总共四种运算
  - 交集
  - 并集
  - 差集
  - 对称差集
- 集合可以
  - 添加 add
  - 清空 clear
  - 指定删除 remove
  - 丢弃 discard
  - 弹出 pop
- 集合之间可以判断
  - 是否有交集
  - 是否是子集
  - 是否是超集
- 除了集合之外
- 还有其他的容器吗？🤔

### 回忆

- 圆括号对应元组
  - 不能动
- 方括号对应列表
  - 能动
- 大括号对应集合
  - 不可重复

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630312802021)

- 但是一个空的大括号
- 他告诉我这是一个 dict 类型
- 什么意思？！

### 帮助手册

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630312993743)

- 看起来是一对数字
  - 有一个 key
  - 还有一个 value
- 构造如下
  - dict(one=1,two=2)

### 试试

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630318824993)

- 这就像是一个字典
- one 是一个要查的单词
- one 是什么意思
- one 的意思就是 1
- 这是一种 map 映射的关系
  - key 和 value 之间的映射关系

### 电话簿

- 我记得住人名
- 但我记不住电话
- 这些人名是没有前后顺序的

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631605238357)

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631605398894)

- 除了电话之外可能还包括地址
- 这个时候人名就可以映射到电话和地址元组
- 人名:(地址,电话)

### 通讯录

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631605765613)

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631605779836)

- 当然 dict 本意是字典 dictionary
- 备查的字就是 key
- key 怎么理解？

### key

- key 就是钥匙

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210925-1632541643680)

- 钥匙干嘛用的？

### 开锁

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210925-1632541652535)

- 钥匙是和锁一一配对的
- 得到这把钥匙就能开这把锁
- 一个钥匙能开两把锁么？
- 一个 key 可以对应两个不同的 value 么？
- 试试

### 一一对应

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210925-1632542127578)

- 只有后面的 key-value 对会留下
- key 和 value 是一一对应
- key 只有一个

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630318814436)

- 那我做个关于动物的英汉字典吧

### 字典

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631606052068)

### 增加元素

- 真的能插入字典么？

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631606460009)

- 翻遍了帮助文件
- 也没有找到插入字典项的方法
- 不过也别着急
- 我们会逐渐熟悉
- 先练练查字典

### 查询字典

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630318940523)

- 根据这个 key
- 就能查到这个 value
- 所用的运算符是索引运算符[]
- 注意中括号在这里并不是表示列表
  - 而是索引运算符
  - 根据谁索引？
- 根据这个 key
- 就能查到这个 value
- 或者说 map 映射到这个 value

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630318953375)

### 字典大小

- 字典里面有几个记录呢
- 用 len 看看

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630319104323)

- 字典里面总共两条记录
- 都有什么索引 keys？
- 都有什么值 values？
- 索引和值都是由列表构成的
- 但这个 keys 和 values 具体是什么类型呢？

### 列表

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630319195498)

- 字典的键和值都有相应数据类型
- 字典不像序列
  - 用数字来索引
- 但是可以用 key 键来找到值 value
- 当数字索引不好用的时候
  - 就可以使用字典
  - 用字符串来进行索引

### key 的含义

- list、tuple、dict 和 int、str 一样
- 都是最最基本的存储变量的数据结构类型
- 是 python 的基础！

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630319745741)

- 本质上来说
  - keys 是一个键的列表
  - values 是一个值的列表
  - 这俩列表是有对应关系的
- 可以通过两个有关系的列表构建字典么？

### 构建

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630319909754)

- 可以把有关列的两个列表
- 压制成一个字典

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630320024717)

- 确实可以通过两个列表 zip 出一个字典
- 如何理解 zip 呢？

### zip

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631606803665)

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631606793297)

- zip 把若干个元组的元素
- 按照所在位置
- 组合成新的元组
- 直到其中某个元组遍历完成
- zip 的结果可以理解为元组的序列
- 可以再构成
  - 序列类的列表、元组等
  - 或者序列类的字典
- 字典 dict 还有其他的构造方法
- 查询手册

### 构造方法

- 这次去 doc.python.org 去查
- 除了用构造函数
- 以下方法都可以
- 而且是等价的

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210830-1630319375973)

### 动手试试

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631607410468)

- 可以把列表的列表转化为字典

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631607489247)

- 可以把元组的元组转化为字典
- 那么 元组的列表 或者 列表的元组呢？

### 手动

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631607708608)

- 元组的列表和列表的元组 都没有问题

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631607736730)

- 只要满足内层序列元素是二维的映射关系就可以
- 序列类的列表和元组可以转化为字典
- 字典可以转化回来么？

### 尝试

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210914-1631607910268)

- 只能把 keys 或者 values 分别转化
- 然后就得再想办法了

### 总结

- 这次学习了字典
- 字典是用来查的
  - 根据一个 key
  - 可以查到相应的 value
- 字典中有很多 key-value 构成的键值对
  - 所有的键可以转化一个列表
  - 所有的值也可以转化一个列表
  - 键值对的关系是对应的
- 字典还可以怎么用呢？🤔
- 下次再说 👋
