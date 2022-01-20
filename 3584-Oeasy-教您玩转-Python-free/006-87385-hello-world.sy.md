---
show: step
version: 1.0
enable_checker: true
---

# Hello World!

## 回忆上次内容

- `python3` 的程序是一个 5.3M 的可执行二进制文件
  - `python3` 里面全都是 cpu 指令
  - 可以执行的那种
  - 我们可以把机器语言指令反汇编
  - 以汇编语言的形式显示
  - `objdump -d ~/python3 > python3.asm`
- 汇编语句是和当前机器架构的指令集相关的
- `uname -a`可以查询当前架构的指令集
- 我们执行的过程其实就是
  - 系统执行`python3`这个可执行文件
  - 给了`python3`一个参数`hello.py`
  - `python3`对于`hello.py`一句句的解释执行
  - 在显示器输出了`hello world`
  - `python3`执行完毕，把控制权交回给 shell
- 这就是我们执行`hello world`的过程
- 可为什么我们学编程总是从`hello world`开始呢？🤔

### 为啥总是`Hello World`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210220-1613786932296)

### 奇怪

- 不论学习什么编程语言
- 总是从`Hello World`开始
- 为什么呢？

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210220-1613786792262)

### 起源

- 这一切都要从头说起
- 目前操作系统的老祖宗 `unix`
- 和他对应的编程语言 `c`
- 是一切的开始

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210220-1613786951642)

- 1969 年，由于所在的`Multics`项目失败有后
- 无所事事的两人
  - `Kenneth Thompson`
  - `Dennis Ritchie`
- 希望能在 PDP 机器上继续玩一个游戏
- 在尝试用 `c` 制作 `unix`
- 当时的 `c` 是他们为了开发 `unix` 制作的语言
- 只能运行在当时的机器上
  - 没有文档
  - 没有书籍
  - 甚至没有人知道

### hello world！

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210220-1613787540042)

- 与他们同在 bell 实验室的`Brian Wilson Kernighan`
  - 开始写 c 语言的类似于文档说明书的东西

### 手稿

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210220-1613787458918)

- 整体的函数叫做 `main`函数
  - 里面输出函数就叫做 `printf`函数
- 不管是`main`，还是`printf`都有`小括号`
- `小括号` 从那个时候就和函数相关
- 为什么输出字符要用`printf`来当做函数名呢?

### print 来历

- 这是 1974 年的手稿
  - 写在打字机用纸的上面
  - 打字机就是当时的显示器
  - 所以用`print`来表示输出
  - `f` 的意思是 `format` 格式
  - `printf` 是按格式输出
- `print函数`后面有`小括号`
  - `小括号`里面放的是`函数`的`参数`
  - `print("hello world")` 中 `函数print`的`参数`是`"hello world"`
  - `双引号`引起来意味着这是`字符串`
  - 输出的内容是 `"hello world"`
  - 最早的入门教学程序都做一个`hello world`
- 习惯成自然之后
  - 所有的编程语言第一个例子都是`hello world`
  - 是一种规矩或者文化

### 成书

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210220-1613787632360)

- 1978 年，`Kernighan`和`Ritchie`出版了这本书
- 不厚，很薄
- 轻松的语言风格
- 因为 `c` 的目的就是让人像玩一样编程
- 而不是记忆各种 cpu 指令
- 蓬勃发展的计算机技术
- 使得 `c` 语言成为系统语言的老大
- python 的源代码就是用纯 c 编的
- linux 内核 也是用纯 c 编的
- 所以`c`还是非常核心的啊
- 不过 python 一旦出现之后就可以简化好多东西

### 内置函数

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211211-1639232145671)

- 我们可以看到游乐场上来就自带一些函数
- 比如 dir()
- 这个函数可以知道当前载入了哪些模块
- 这里面有些什么呢？

### help(**builtins**)

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211211-1639232283926)

- 我们可以查询到各种各样的内建函数
- 他们都隶属于**builtin**这个模块
- 这是一个内建的模块
- 当让也可以引入模块

### 导入模块

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211211-1639232462283)

```python
import __hello__
```

- 可以输出经典的一句话
- 还可以把新模块导入到游乐场

## 总结

- `hello world` 不是从来就有的
- 来自于`unix`和`c`
- 虽然我们今天有各种先进的学习手段
- 最早的高级语言学习是从最早的那张打字机用纸的手写代码起源的
  - 所以输出用的是 `print`
  - 最早输出的是 `hello world`
  - 这就成了一个迷因
- 计算机里面不都是二进制的 0 和 1 吗
- 哪里来的`h`之类的字符呢？🤔
- 我们下次再说！👋
