---
show: step
version: 1.0
enable_checker: true
---

# Linux 简介

## 一、实验介绍

#### 1.1 实验内容

本节主要介绍 Linux 的历史，Linux 与 Windows 的区别等入门知识。如果你已经有过充分的了解，可以跳过本节，直接进入下一个实验。

#### 1.2 知识点

- linux 为何物
- linux 历史简介
- linux 重要人物
- linux 与 windows 的不同
- 如何学习 linux

## 二、实验内容

#### linux 为何物

Linux 就是一个操作系统，就像你多少已经了解的 Windows（xp，7，8）和 Mac OS 。至于操作系统是什么，就不用过多解释了，如果你学习过前面的入门课程，应该会有个基本概念了，这里简单介绍一下操作系统在整个计算机系统中的角色。

![图1-1](https://doc.shiyanlou.com/linux_base/1-1.png/wm)

我们的 Linux 主要是系统调用和内核那两层。当然直观地看，我们使用的操作系统还包含一些在其上运行的应用程序，比如文本编辑器、浏览器、电子邮件等。

### 2.1 Linux 历史简介

操作系统始于二十世纪五十年代，当时的操作系统能运行批处理程序。批处理程序不需要用户的交互，它从文件或者穿孔卡片读取数据，然后输出到另外一个文件或者打印机。

二十世纪六十年代初，交互式操作系统开始流行。它不仅仅可以交互，还能使多个用户从不同的终端同时操作主机。这样的操作系统被称作分时操作系统，它的出现对批处理操作系统是个极大的挑战。许多人尝试开发分时操作系统， 其中包括一些大学的研究项目和商业项目。当时有个项目叫做 Multics ，它的技术在当时很具有创新性。 Multics 项目的开发并不顺利，它花费了远超过预计的资金，却没有在操作系统市场上占到多少份额。而参加该项目的一个开发团体——贝尔实验室退出了这个项目。他们在退出后开发了他们自己的一个操作系统—— UNIX 。

UNIX 最初免费发布并因此在大学里受到欢迎。后来，UNIX 实现了 TCP/IP 协议栈，成为了早期工作站的操作系统的一个流行选择。

1990 年，UNIX 在服务器市场上尤其是大学校园中成为主流操作系统，许多校园都有 UNIX 主机，当然还包括一些研究它的计算机系的学生。这些学生都渴望能在自己的电脑上运行 UNIX 。不幸的是，从那时候开始，UNIX 开始变得商业化，它的价格也变得非常昂贵。而唯一低廉的选择就是 MINIX，这是一个功能有限的类似 UNIX 的操作系统，作者 Andrew Tanenbaum 开发它的目的是用于教学。

1991 年 10 月，Linus Torvalds（Linux 之父）在赫尔辛基大学接触 UNIX，他希望能在自己的电脑上运行一个类似的操作系统。可是 UNIX 的商业版本非常昂贵，于是他从 MINIX 开始入手，计划开发一个比 MINIX 性能更好的操作系统。很快他就开始了自己的开发工作。他第一次发行的版本迅速吸引了一些黑客。尽管最初的 Linux 并没有多少用处，但由于一些黑客的加入使它很快就具有了许多吸引人的特性，甚至一些对操作系统开发不感兴趣的人也开始关注它。

Linux 本身只是操作系统的内核。内核是使其它程序能够运行的基础。它实现了多任务和硬件管理，用户或者系统管理员交互运行的所有程序实际上都运行在内核之上。其中有些程序是必需的，比如说，命令行解释器（shell），它用于用户交互和编写 shell 脚本。 Linus 没有自己去开发这些应用程序，而是使用已有的自由软件。这减少了搭建开发环境所需花费的工作量。实际上，他经常改写内核，使得那些程序能够更容易地在 Linux 上运行。许多重要的软件，包括 C 编译器，都来自于自由软件基金 GNU 项目。GNU 项目开始于 1984 年，目的是为了开发一个完全类似于 UNIX 的免费操作系统。为了表扬 GNU 对 Linux 的贡献，许多人把 Linux 称为 GNU/Linux（GNU 有自己的内核）。

1992－1993 年，Linux 内核具备了挑战 UNIX 的所有本质特性，包括 TCP/IP 网络，图形界面系统（X window )，Linux 同样也吸引了许多行业的关注。一些小的公司开始开发和发行 Linux，有几十个 Linux 用户社区成立。1994 年，Linux 杂志也开始发行。

Linux 内核 1.0 在 1994 年 3 月发布，内核的发布要经历许多开发周期，直至达到一个稳定的版本。

下面列举一些 Linux 诞生大事件：

- 1965 年，Bell 实验室、MIT、GE（通用电气公司）准备开发 Multics 系统，为了同时支持 300 个终端访问主机，但是 1969 年失败了；

  > 那时候并没有鼠标、键盘，输入设备，只有卡片机。因此，如果要测试某个程序，则需要将读卡纸插入卡片机，如果有错误，还需要重新来过；Multics：Multiplexed Information and Computing Service；

- 1969 年，Ken Thompson（C 语言之父）利用汇编语言开发了 File Server System（Unics，即 UNIX 的原型）；

  > 因为汇编语言对于硬件的依赖性，因此只能针对特定硬件；
  > 只是为了移植一款“太空旅游”的游戏；

- 1973 年，Dennis Ritchie 和 Ken Thompson 发明了 C 语言，而后写出了 UNIX 的内核；

  > 将 B 语言改成 C 语言，由此产生了 C 语言之父；90% 的代码是 C 语言写的，10% 的代码用汇编语言写的，因此移植时只要修改那 10% 的代码即可；

- 1977 年，Berkeley 大学的 Bill Joy 针对他的机器修改了 UNIX 源码，称为 BSD（Berkeley Software Distribution）；

  > Bill Joy 是 Sun 公司的创始人；

- 1979 年，UNIX 发布 System V，用于个人计算机；

- 1984 年，因为 UNIX 规定“不能对学生提供源码”，Tanenbaum 老师自己编写兼容于 UNIX 的 Minix，用于教学；

- 1984 年，Stallman 开始 GNU（GNU's Not Unix）项目，创办 FSF（Free Software Foundation）基金会；

  > 产品：GCC、Emacs、Bash Shell、GLIBC；倡导“自由软件”；GNU 的软件缺乏一个开放的平台运行，只能在 UNIX 上运行；自由软件指用户可以对软件做任何修改，甚至再发行，但是始终要挂着 GPL 的版权；自由软件是可以卖的，但是不能只卖软件，而是卖服务、手册等；

- 1985 年，为了避免 GNU 开发的自由软件被其他人用作专利软件，因此创建 GPL（General Public License）版权声明；

- 1988 年，MIT 为了开发 GUI，成立了研发 XFree86 的组织；

- 1991 年，芬兰赫尔辛基大学的研究生 Linus Torvalds 基于 gcc、bash 开发了针对 386 机器的 Linux 内核；

- 1994 年，Torvalds 发布 Linux-v1.0；

- 1996 年，Torvalds 发布 Linux-v2.0，确定了 Linux 的吉祥物：企鹅。

UNIX 进化史（UNIX 大家族族谱 1969-2013）：

![1](https://dn-simplecloud.shiyanlou.com/uid/c4ca4238a0b923820dcc509a6f75849b/1467262784463.png-wm)

### 2.2 Linux 重要人物

#### 1. Ken Thompson：C 语言之父和 UNIX 之父

![1](https://doc.shiyanlou.com/linux_base/1-2.png/wm)

#### 2. Dennis Ritchie：C 语言之父和 UNIX 之父

![1](https://doc.shiyanlou.com/linux_base/1-3.jpg/wm)

#### 3. Stallman：著名黑客，GNU 创始人，开发了 Emacs、gcc、bash shell

![1](https://doc.shiyanlou.com/linux_base/1-4.jpg/wm)

#### 4. Bill Joy：BSD 开发者

![1](https://doc.shiyanlou.com/linux_base/1-5.jpg/wm)

#### 5. Tanenbaum：Minix 开发者

![1](https://doc.shiyanlou.com/linux_base/1-6.jpg/wm)

#### 6. Linus Torvalds：Linux 之父，芬兰赫尔辛基大学

![1](https://doc.shiyanlou.com/linux_base/1-7.jpg/wm)

### 2.3 Linux 与 Windows 到底有哪些不同

#### 1. 免费与收费

- 最新正版 Windows 10，需要付费购买；
- Linux 免费或少许费用。

#### 2. 软件与支持

- Windows 平台：数量和质量的优势，不过大部分为收费软件；由微软官方提供重要支持和服务；
- Linux 平台：大都为开源自由软件，用户可以修改定制和再发布，由于基本免费没有资金支持，部分软件质量和体验欠缺；由全球所有的 Linux 开发者和自由软件社区提供支持。

#### 3. 安全性

- Windows 平台：三天两头打补丁安装系统安全更新，还是会中病毒木马；
- Linux 平台：要说 Linux 没有安全问题，那当然是不可能的，这一点仁者见仁智者见智，相对来说肯定比 Windows 平台要更加安全，使用 Linux 你也不用装某杀毒、某毒霸。

#### 4. 使用习惯

- Windows：普通用户基本都是纯图形界面下操作使用，依靠鼠标和键盘完成一切操作，用户上手容易，入门简单；
- Linux：兼具图形界面操作（需要使用带有桌面环境的发行版）和完全的命令行操作，可以只用键盘完成一切操作，新手入门较困难，需要一些学习和指导（这正是我们要做的事情），一旦熟练之后效率极高。

#### 5. 可定制性

- Windows：这些年之前算是全封闭的，系统可定制性很差；
- Linux：你想怎么做就怎么做，Windows 能做到得它都能，Windows 做不到的，它也能。

#### 6. 应用范畴

或许你之前不知道 Linux ，要知道，你之前在 Windows 使用百度、谷歌，上淘宝，聊 QQ 时，支撑这些软件和服务的，是后台成千上万的 Linux 服务器主机，它们时时刻刻都在忙碌地进行着数据处理和运算，可以说世界上大部分软件和服务都是运行在 Linux 之上的。

#### 7. Windows 没有的

- 稳定的系统
- 安全性和漏洞的快速修补
- 多用户
- 用户和用户组的规划
- 相对较少的系统资源占用
- 可定制裁剪，移植到嵌入式平台（如安卓设备）
- 可选择的多种图形用户界面（如 GNOME，KDE）

#### 8. Linux 没有的

- 特定的支持厂商
- 足够的游戏娱乐支持度
- 足够的专业软件支持度

### 2.4 如何学习 Linux

#### 1. `学习心态`

- 明确目的：你是要用 Linux 来干什么，搭建服务器、做程序开发、日常办公，还是娱乐游戏；

- 面对现实：Linux 大都在命令行下操作，能否接受不用或少用图形界面；

- 是学习 Linux 操作系统本身还是某一个 Linux 发行版（[Ubuntu](http://www.ubuntu.com/)，[CentOS](http://www.centos.org/)，[Fedora](http://fedoraproject.org/)，[OpenSUSE](http://www.opensuse.org/)，[Debian](http://www.debian.org/)，[Mint](http://linuxmint.com/) 等等），如果你对发行版的概念或者它们之间的关系不明确的话可以参看 [Linux 发行版](http://baike.baidu.com/view/897468.htm)。

#### 2. `注重基础，从头开始`

大致的学习路径如下：

![1](https://doc.shiyanlou.com/linux_base/1-8.png/wm)
