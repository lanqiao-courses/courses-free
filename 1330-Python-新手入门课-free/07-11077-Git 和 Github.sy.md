---
show: step
version: 1.0
enable_checker: true
---

# Git 与 GitHub

## 实验介绍

你来了，快坐下，今天我们来讲讲编程界的两大神器： **Git** 和 **GitHub**。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558580810928)

**Git** 和 **GitHub** 都是程序员每天都要用到的东西 —— 前者是目前最先进的 **版本控制工具**，拥有最多的用户，且管理着地球上最庞大的代码仓库；而后者是全球最大 ~~同性交友~~ **代码托管平台、开源社区**。

如果你之前没接触过 **「版本控制」** 的概念，可以看看下面的例子：

大四毕业生 **小张** 在写 **毕业论文**，他经常删删改改，有时还会后悔“昨天那个思路那么好，我怎么就给删了”。

有了多次教训后，他决定每次写之前都先复制一份，在复制的那份里修改，这么一来，文件夹里有了：

```txt
毕业论文_初稿.doc
毕业论文_修改 1.doc
毕业论文_修改 2.doc
毕业论文_修改 3.doc
毕业论文_完整版 1.doc
毕业论文_完整版 2.doc
毕业论文_完整版 3.doc
毕业论文_最终版 1.doc
毕业论文_最终版 2.doc
毕业论文_确定版 1.doc
毕业论文_确定版 2.doc
……
```

小张想：“虽然很痛苦，但不至于丢掉以前的灵感了吧……等等，最终版和确定版哪个是昨天写的来着？？？”

同时，他还要把论文发给学霸女友求帮忙，第二天他的文件夹里又有了：

```txt
毕业论文_最终版 3.doc
毕业论文_女友版 1.doc
毕业论文_女友版 2.doc
```

几星期的煎熬下来，文件夹里多了几十份文件，小张的论文也快成型了，是时候把自己和女朋友的内容合并起来了。

这时又发生了一件喜闻乐见的事：**U 盘中病毒了**，而电脑里只有 1 个月前的版本……

如何拯救生无可恋的小张？其实，如果小张一早知道用「版本控制」工具就好了，他的文件可以整整齐齐地排列，就像这样：

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558583904136)

“哎呀，早知道能这样，就不用手动控制那么多版本啦！”

**但这还不够，如果能有一个支持「论文托管 + 论文版本控制」的网站就更好了。这样一来，小张不但能和女朋友合作编辑内容，还不用担心因电脑故障，导致之前论文版本的丢失。**

这时 —— **论文 Hub** 出现了，它可以帮你托管论文，而且和版本控制工具无缝连接。

越来越多人发现了 **论文 Hub** 的好处，相继把论文托管在论文 Hub 上，网站上的论文越来越多。一些优秀的作者还会把论文开源出来，让每个人都可以查阅、交流、学习……

慢慢的，**论文 Hub** 变成了全球最大的「交友社区」，并逐渐演化成了一种时尚 —— 找工作时，面试官会先问你有没有 **论文 hub** 的账号，有多少个赞、多少粉丝；而有优秀作品的人，会被大公司争抢录用……

这个 论文 hub，就是我们今天要学习的 **GitHub** ，只不过论文换成了程序代码。GitHub 大概长这样：

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20220819-1660889787761)

<center>图为 Python 之父的 GitHub 截图</center>

**在没有这两个工具时，编程可能是这样的：**

- 哪个同事修改了我的代码 🔪 我要杀了他
- 我把自己的代码改崩溃了 🙂️ 我选择自杀
- 电脑崩溃、硬盘损坏、中毒，几万行代码找不到了 😱

**但有了他们，一切都不一样了：**

- 同步代码到网络仓库，在家里写好代码上传，回到公司就可以继续写了，而且不怕丢失。
- 记录每次代码的修改，即使把程序写崩了，也能及时回溯到上一个版本，这在产品更新时也经常使用。
- 可以多人协作完成项目，每个人的提交都有清晰的记录。

在之后的学习中，你也会不断用到 **Git** 和 **GitHub**，把你完成的项目、学习记录，同步在 GitHub 的仓库中。这样做的结果是：你将有一份 **非常漂亮的 GitHub 主页**，能给你的简历加分很多。

接下来，我们将学习 Git 的基本操作，并注册 GitHub 账户，建立你的第一个代码仓库！

#### 知识点

- 版本控制
- Git 和 GitHub 的历史
- 在 GitHub 创建仓库
- 添加修改到暂存区
- 提交代码
- 同步远程仓库

## Git 与 GitHub 的历史

Linux 之父林纳斯（Linus）在 1991 年创建了开源的 Linux 系统，随着 Linux 代码量越来越大，合并志愿者提交的代码已经无法依靠人工完成，所以 林纳斯 选择了商业的管理软件 BitKeeper，来管理 Linux 的代码版本。

在 2005 年，BitKeeper 公司发现，有 Linux 社区的人试图破解 BitKeeper 软件，他们决定收回 Linux 社区的免费使用权。Linus 对此事调解数周无果，决定自己搞一个。**他花了十天时间用 C 语言写好了一个开源的版本控制系统，就是著名的 Git。**

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558587393317)

（先写出一个操作系统，再用 10 天写出 Git，林纳斯已经不能用大神来形容，只能理解为外星人来技术扶贫的）

**2007 年，旧金山的三个年轻人觉得 Git 是个好东西，创建一个公司名字叫 GitHub，第二年上线了使用 Ruby 编写的同名网站。**这是一个基于 Git 的免费代码托管网站（有付费服务），十年间迅速蹿红，击败了实力雄厚的 Google Code，成为全世界最受欢迎的代码托管网站。根据 2018 年 10 月的 GitHub 年度报告显示，目前有 3100 万开发者创建了 9600 万个项目仓库，有 210 万企业入驻。

**值得一提的是，GitHub 的创始人、CEO —— Wanstrath 是一位大学肄业，自学成才的程序员。**

而他大学的专业是英语，相当于我们的“汉语言文学”专业，是一名地地道道的“文科生”，如今，他的身家可能超过了 10 亿美元，也算是一位自学编程成才的标志人物吧。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558587496016)

## Git 的安装

Git 是一个版本控制系统，可以理解为一个工具，使用之前必须得先下载安装，所以第一步必须要安装。

- Windows：[**GitForWindows**](https://gitforwindows.org/index.html)

- Mac 系统安装：[**git-osx-installer**](https://sourceforge.net/projects/git-osx-installer/)

- Linux：在终端输入命令行安装

- Debian 系列：apt-get install git

- Fedora 上：yum install git-core

蓝桥云课的环境中预装了 `Git`，打开 `终端` ，输入 `git` 可以检测是否安装成功：

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558592988977)

如图，如果成功安装了 Git，会显示 Git 的常用命令，以后忘记命令时，也记得输入 `git` 查看一下～

## 在 GitHub 上创建仓库

下面，我们通过一次上传 GitHub 的完整操作，实践学习 Git 的常用功能。

**首先，申请一个 GitHub 账户。**

打开 [GitHub](https://github.com/)，你可以在主页的 banner 上快速注册，或者点击右上角的 `Sign up` 注册。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558594190554)

下一步，我们选择创建一个免费账户。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558594464315)

下一步，回答两个问题，也可以在最下面选择跳过：

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558594762889)

最后，去注册邮箱中接收验证邮件，验证后，自动用新注册账户登录，进入 GitHub 主页。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558595244685)

**第二步，我们来新建一个代码仓库。**

仓库（repository），可理解为储存代码的场所，点击个人主页的右上角的加号，再点击 `New repository`，即可创建新的仓库：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid7166timestamp1548755508075.png)

然后给你的仓库命名（比如说 Demo），然后点击 Create Repository，无需考虑本页面的其他选项。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558596226639)

创建后，页面跳转到新建仓库的主页面，第一步成功了！

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558596181235)

## 添加 SSH 关联授权

2021 年 8 月 GitHub 不再允许使用账户密码操作 GitHub，必须使用 SSH 密钥登陆。

2022 年 3 月 15 日，GitHub 又通过删除较旧的不安全密钥类型提高了安全性。

接下来，我们可以在系统中创建 SSH 公私钥，并将公钥放到 GitHub 指定位置。如此操作即可生成 GitHub 账户对于当前系统中的 Git 授权。

在终端执行下面命令，每次询问按回车即可。

```bash
ssh-keygen -t ed25519 -C "你的邮箱地址"
```

终端执行 `ssh-keygen` 命令按几次回车生成公私钥，公私钥存放在主目录下的隐藏目录 `.ssh` 中的两个文件中：

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20220829-1661768801543)

将 `~/.ssh/id_ed25519.pub` 文件中的公钥内容复制出来，实验环境中可以使用右侧工具栏中的剪切板复制：

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20220829-1661769181333)

然后在 GitHub 网页上添加公钥：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid9816timestamp1548756492545.png/wm)

Title 自定义，把剪切板中的内容粘贴到 Key 中，点击绿色按钮添加 SSH Key 即可：

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20220829-1661769320451)

使用 SSH 的好处主要有两点：

- 免密码推送，执行 `git push` 时不再需要输入用户名和密码了；
- 提高数据传输速度。它不是必须的，比如在课程中挑战环境是不可保存的，一次性的，这种环境就没有必要创建 SSH 了，因为相较好处来说，还是太麻烦了。

## 克隆 GitHub 上的仓库到本地

现在克隆前面我们在 GitHub 上创建的仓库，使用 `git clone + [仓库地址]` 命令即可，这是标准的克隆仓库命令。

回到仓库主目录，点击下图所示的绿色按钮，点击紫色框中的“Use SSH”，然后复制这个链接。

**重要的一点：只有使用这种 git 开头的地址克隆仓库，SSH 关联才会起作用。**

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid600404labid9816timestamp1549876495736.png/wm)

在实验环境里删除原仓库，使用此链接重新克隆仓库。克隆仓库是需要确认连接，输入 yes 即可：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid9816timestamp1548756521858.png/wm)

进入仓库主目录，如下图所示，仓库主目录中有个 `.git` 隐藏目录，它里面包含了仓库的全部信息，删掉这个目录，仓库就变成普通的目录了。进入到仓库目录中，命令行前缀发生了一些变化，出现了红色的 master，它就是当前所在的分支名：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid7166timestamp1548755685917.png/wm)

当我们在 GitHub 上创建一个仓库时，同时生成了仓库的默认主机名 origin，并创建了默认分支 master。GitHub 可以看成是免费的 Git 服务器，在 GitHub 上创建仓库，会自动生成一个仓库地址，主机就是指代这个仓库，主机名就等于这个仓库地址。克隆一个 GitHub 仓库（也叫远程仓库）到本地，本地仓库则会自动关联到这个远程仓库，执行 `git remote -v` 命令可以查看本地仓库所关联的远程仓库信息：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid7166timestamp1548755698081.png/wm)

Git 要求对本地仓库关联的每个远程主机都必须指定一个主机名（默认为 origin），用于本地仓库识别自己关联的主机，`git remote` 命令就用于管理本地仓库所关联的主机，一个本地仓库可以关联任意多个主机（即远程仓库）。

克隆远程仓库到本地时，还可以使用 `-o` 选项修改主机名，在地址后面加上一个字段作为本地仓库的主目录名，举例如下：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid7166timestamp1548755710736.png/wm)

另一个在其它 Git 教程中常见的命令 `git init` ，它会把当前所在目录变成一个本地仓库，因为有 GitHub 的存在，这个命令在我们的生产生活中用到的次数应该是零，除非你想费时费力自己搭建服务器。操作截图如下：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid7166timestamp1548755742879.png/wm)

## 实验总结

本节实验主要介绍了以下知识点：

- Git 和 GitHub 的来历
- 使用 GitHub 创建仓库
- 安装 / 更新 Git
- 使用 Git 克隆远程仓库到本地

本节主要介绍了 Git 与 GitHub 概述，以及创建仓库、克隆仓库到本地的基本操作，相对比较简单。大家在学习时重复操作几次即可熟练掌握。下一节将学习 Git 的使用流程，完成一次修改、提交、推送操作。
