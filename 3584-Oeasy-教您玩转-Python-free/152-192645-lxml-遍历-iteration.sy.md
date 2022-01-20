---
show: step
version: 1.0
enable_checker: true
---

# 语法 html 遍历

## 回忆

- 了解 etree 中的元素的文本成员变量
  - text - 字符串类型
  - tail - 字符串类型
- 就像原来学的 etree 元素中的成员变量
  - tag - 字符串类型
  - attrib - 字典类型
- 还可以设置 etree 元素的成员变量 text
- tostring 函数有一些参数可以控制输出
  - method = "text" 可以控制输出结果只包含 text 和 tail
  - with_tail = False 可以控制输出结果不包含 tail
  - pretty_print = True 可以控制输出结果包含缩进信息
- 我们已经在内存构建了一棵树了
- 但是如何遍历这棵树呢？🤔

### xpath

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630463341760)

- "string()"可以得到根节点的文本
- 或者"//text()"文本列表

### 得到文本

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630463370460)

- 看起来
  - 一个得到的是拼接起来的字符串
  - 另一个得到的是字符串列表

### 遍历所有节点

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630463450183)

- etree.Element 类可以有很多 Element 子对象
- 可以用 for 语言来遍历 Element 的子对象

### iter()函数遍历所有子元素

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630463551258)

- 我们使用 iter 函数
  - 递归地遍历了一遍根元素和各子元素节点
    - 从根开始
    - 然后遍历两个根的子元素
    - 然后递归递遍历子元素里面的子元素
- 输出元素时
  - 输出标签名
  - 输出文本
- 这函数的返回值是什么类型的呢？

### iter()类型

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211106-1636195524938)

- 这个东西的类型是一个深度优先迭代器
- 什么是深度优先？

### 深度优先

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630463551258)

- 见到节点先往深里走
  - 而不是先往广里走
- 和深度优先相对的是广度优先

### 筛选标签

- iter("child")可以遍历所有标签是 child 的子元素

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630463660607)

### 增加筛选标签

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211106-1636195307288)

- root.iter("child","another")筛选这两个 tag 的所有元素
  - 然后遍历

### 动手

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630463683785)

- 你能够想象出 iter 方法的实现的具体代码吗？

### 实体和注释

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630463857945)

- 为 root 元素增加了两类对象
  - etree.Entity("#234") 实体
  - etree.Comment("some comment") 注释
- root 元素本身是 etree.Element 对象
- Entity 和 Comment 是两类对象
  - 作为 root 元素的子对象
    - 可以遍历
  - 但是不属于 etree.Element 元素类的对象
- iter()函数中用参数 tag=etree.Element 可以进行筛选
- Entity 元素本身只包含
  - text 成员
- Comment 元素包含
  - 头尾的注释标记
  - 具体的注释内容

### 添加实体

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630463996964)

### 遍历元素和实体

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630464157409)

- iter()函数的参数
  - tag=etree.Entity - 筛选出 Entity 实体对象
  - tag=etree.Element - 筛选出 Element 元素对象
  - tag=etree.Comment - 筛选出 Comment 注释对象
- 现在可以在内存里生成操作这颗 etree 树了
- 可是怎么通过网上爬到的 html 文件生成 etree 树呢？

## 总结

- 已经在内存构建了一棵 etree 树了
- 树是由节点 Element 构成的
- 除了 etree.Element 节点之外，还有
  - etree.Entity
  - etree.Comment
- Element 元素最重要，他的成员有:
  - attrib 属性字典
  - text 具体文本
  - tail 后跟文本
  - tag 标签
  - iter() 迭代器函数
    - 可以用 for 遍历迭代器函数
    - 参数 tag=etee.Element 可以类型进行筛选
- 但是网上爬到的网页源文件的 response 只有字节序列
  - 如何通过字节序列得到一棵 etree 树呢？🤔
- 下次再说
