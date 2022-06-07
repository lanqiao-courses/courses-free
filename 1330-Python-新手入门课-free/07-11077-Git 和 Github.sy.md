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

如果你之前没接触过 **「版本控制」** 的概念，看到这里一定是一脸蒙 x 的，别急，看了这篇文章你一定能明白：

[**什么是版本控制**](http://blog.a0z.me/2014/05/21/GitBeginning/)

简单复述一下文章中的例子：

大四毕业生 **小张** 在写 **毕业论文**，他经常删删改改，有时还会后悔“昨天那个思路那么好，我怎么就给删了”。

有了多次教训后，他决定每次写之前都先复制一份，在复制的那份里修改，这么一来，文件夹里有了：

```txt
毕业论文_初稿.doc
毕业论文_修改1.doc
毕业论文_修改2.doc
毕业论文_修改3.doc
毕业论文_完整版1.doc
毕业论文_完整版2.doc
毕业论文_完整版3.doc
毕业论文_最终版1.doc
毕业论文_最终版2.doc
毕业论文_确定版1.doc
毕业论文_确定版2.doc
……
```

小张想：“虽然很痛苦，但不至于丢掉以前的灵感了吧……等等，最终版和确定版哪个是昨天写的来着？？？”

同时，他还要把论文发给学霸女友求帮忙，第二天他的文件夹里又有了：

```txt
毕业论文_最终版3.doc
毕业论文_女友版1.doc
毕业论文_女友版2.doc
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

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558617168374)

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

Linux 之父林纳斯（Linus） 在 1991 年创建了开源的 Linux 系统，随着 Linux 代码量越来越大，合并志愿者提交的代码已经无法依靠人工完成，所以 林纳斯 选择了商业的管理软件 BitKeeper ，来管理 Linux 的代码版本。

在 2005 年，BitKeeper 公司发现，有 Linux 社区的人试图破解 BitKeeper 软件，他们决定收回 Linux 社区的免费使用权。Linus 对此事调解数周无果，决定自己搞一个。**他花了十天时间用 C 语言写好了一个开源的版本控制系统，就是著名的 Git。**

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558587393317)

（先写出一个操作系统，再用 10 天写出 Git，林纳斯已经不能用大神来形容，只能理解为外星人来技术扶贫的）

**2007 年，旧金山的三个年轻人觉得 Git 是个好东西，创建一个公司名字叫 GitHub，第二年上线了使用 Ruby 编写的同名网站。**这是一个基于 Git 的免费代码托管网站（有付费服务），十年间迅速蹿红，击败了实力雄厚的 Google Code，成为全世界最受欢迎的代码托管网站。根据 2018 年 10 月的 GitHub 年度报告显示，目前有 3100 万开发者创建了 9600 万个项目仓库，有 210 万企业入驻。

**值得一提的是，GitHub 的创始人、CEO —— Wanstrath 是一位大学肄业，自学成才的程序员。**

而他大学的专业是英语，相当于我们的“汉语言文学”专业，是一名地地道道的 “文科生”，如今，他的身家可能超过了 10 亿美元，也算是一位自学编程成才的标志人物吧。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558587496016)

### Git 的安装

Git 是一个版本控制系统，可以理解为一个工具，使用之前必须得先下载安装，所以第一步必须要安装。

- Windows：[**GitForWindows**](https://gitforwindows.org/index.html)

- Mac 系统安装：[**git-osx-installer**](https://sourceforge.net/projects/git-osx-installer/)

- Linux：在终端输入命令行安装

- Debian 系列：apt-get install git

- Fedora 上：yum install git-core

蓝桥云课的环境中预装了 `Git`，打开 `终端` ，输入 `git` 可以检测是否安装成功：

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558592988977)

如图，如果成功安装了 Git，会显示 Git 的常用命令，以后忘记命令时，也记得输入 `git` 查看一下～

### 在 GitHub 上创建仓库

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

仓库（ repository），可理解为储存代码的场所，点击个人主页的右上角的加号，再点击 `New repository`，即可创建新的仓库：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid7166timestamp1548755508075.png)

然后给你的仓库命名（比如说 Demo），然后点击 Create Repository，无需考虑本页面的其他选项。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558596226639)

创建后，页面跳转到新建仓库的主页面，第一步成功了！

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558596181235)

### 添加 SSH 关联授权

在 2021 年 8 月 GitHub 更新后，已经不再允许使用账户密码操作 GitHub，必须使用 SSH 密钥登陆。所以我们可以在系统中创建 SSH 公私钥，并将公钥放到 GitHub 指定位置。如此操作即可生成 GitHub 账户对于当前系统中的 Git 授权。

终端执行 `ssh-keygen` 命令按几次回车生成公私钥，公私钥存放在主目录下的隐藏目录 `.ssh` 中的两个文件中：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid9816timestamp1548756454421.png)

将 `~/.ssh/id_rsa.pub` 文件中的公钥内容复制出来，实验环境中可以使用右侧工具栏中的剪切板复制：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid9816timestamp1548756470163.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid9816timestamp1548756481375.png)

然后在 GitHub 网页上添加公钥：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid9816timestamp1548756492545.png)

Title 自定义，把剪切板中的内容粘贴到 Key 中，点击绿色按钮添加 SSH Key 即可：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid9816timestamp1548756503765.png)

使用 SSH 的好处主要有两点：

- 免密码推送，执行 `git push` 时不再需要输入用户名和密码了；
- 提高数据传输速度。它不是必须的，比如在课程中挑战环境是不可保存的，一次性的，这种环境就没有必要创建 SSH 了，因为相较好处来说，还是太麻烦了。

### 创建文件

下面，让我们在本地新建一个文件，最后上传到刚刚创建的仓库中。

现在的你在 Linux 中创建文件，应该很轻车熟路了吧：

```bash
mkdir Demo
cd Demo
gedit README.md
```

然后在打开的文件中输入 `#Demo`，保存文件后关闭 gedit 。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558597024894)

用 `cat` 命令查看一下文件内容：

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558597073053)

创建文件 OK 了，但现在，Demo 目录还只是一个普通的目录，我们如何用 `Git` 来控制这个目录？

你只需在 Demo 目录中，输入 `git init` 即可。

```bash
git init
```

这是 `Git` 的初始化操作，作用是**将一个已存在文件夹，置于 Git 的控制管理之下。**

再 `ls -la` 命令，会发现一个名叫 .git 的目录被创建了，这意味着仓库初始化成功。可以进入到 .git 目录查看下有哪些内容。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558598663328)

### 提交代码

Git 提交代码的基本流程是这样的：

- 创建或修改 **本地文件**
- 使用 **git add** 命令，将创建或修改的文件添加到本地的 **暂存区**，这里保存的是你的临时更改
- 使用 **git commit** 命令，提交文件到 **本地仓库**
- 使用 **git push** 命令，将本地代码库同步到 **远端仓库**

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558600871270)

目前为止，我们实现了第一步，创建了一个文件，我们的**最终目标是**：将本地的 Demo 仓库，同步到 GitHub 上的 Demo 仓库中。

#### 💡 git add

使用 `git add + 文件名/目录名` 命令，可以将你需要同步的文件，添加到本地的暂存区。我们先进入 Demo 目录，然后把 README.md 文件添加一下：

```bash
cd /home/shiyanlou/Demo
git add README.md
```

输入 `git status` ，可以检测当前目录和暂存区的状态，查看哪些修改被暂存了：

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558601543864)

可以看到我们刚刚 `add` 的文件已经被初始提交了。

#### 💡 git commit

`git commit` 提交是你工作的一个里程碑 —— **每当你完成一些工作，都可以创建一次提交，保存当前的版本。**

这样一来，无论你何时修改了文件，都创建一个新版本的文件，你可以很方便地查看以往所有版本的文件和内容。

**在提交之前，你必须先设置你的名字和 email**，这是你在提交 commit 时的签名，每次提交记录里都会包含这些信息。

使用 git config 命令进行配置：

```bash
git config --global user.name "YourName"
git config --global user.email "YourEmail@xxx.com"
```

完成配置后，我们可以创建提交了，请输入：

```bash
git commit -m "first commit"
```

`commit` 的语法结构是 `git commit -m "注释"`，通过上个命令，你创建了一条注释为 “first commit” 的 Git 提交。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558603780044)

**⚠️ 注意：**

每次提交，你都必须用 `-m + '注释'` 编辑注释信息 。它不仅能协助我们辨别不同的版本，而且能让你理解，自己当时对文件做了什么修改。

比如当你每次在文件中添加了新的代码后，你可以写一句提交信息：“添加了 XXX 代码” —— 当你一个月后回来看提交记录或者 Git 日志 时，你还能知道当时做了什么。

### 与 GitHub 仓库同步

终于到了激动人心的时刻，我们要把本地仓库提交到远端仓库（即 GitHub 仓库）中。

#### 💡 连接 GitHub 仓库

使用如下命令，将本地仓库连接到 GitHub 仓库中：

```bash
git remote add origin 仓库链接
```

仓库链接请在这里复制，并用剪切板功能粘贴进去：

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558606913216)

我们分析一下这个命令，首先 **remote** 的意思是**远程**：

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558607126461)

`add` 很容易明白 —— 添加。`git remote add` 表示通知 Git 去添加一个远程仓库，后面接上的 `origin` 是这个仓库的小名，方便以后沟通，通常默认用 `origin` 来表示；最后再接上远程仓库的地址，即你刚刚创建的 GitHub 仓库链接。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558607911486)

#### 💡 push 命令

**`push`** 顾名思义，就是推送， 使用 **`push`** 可以把本地仓库推送到远端仓库中。

具体命令如下：

```bash
git push origin master
```

执行后，GitHub 服务器会验证你的身份，完成 push 同步。因为我们已经配置了 SSH，该过程会自动完成。

**目前国内网络访问 GitHub 不太稳定，如果遇到连接失败，可以多尝试几次。**

接下来就是见证奇迹的时刻 —— 再刷新你的 GitHub 仓库，就会发现多了这些东西：

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558609348514)

💡**分支的概念**：分支在多人协作中经常会被用到，但前期我们用不到这个功能，为了不给你增加认知负担，这里就先不讲了。你只需知道 Git 管理的项目进程中，有一条默认的主分支 - `master` 即可。（想象 Git 是一棵树，`master` 就是树干，树干上还可以生出很多分支来，如 master 2.0、master 3.0 等）

### 克隆 GitHub 上的仓库

本节实验最后一个知识点是 `git clone` 命令，它可以帮你拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558610914428)

如果你想要复制一个项目，看看代码，或者把自己的远程仓库复制到本地，可以执行命令：

```bash
git clone [url]
```

[url] 指的就是你想复制的仓库，我们在 `github.com` 上提供了一个名字为 `gitproject` 的公开仓库， 供大家测试，现在你要把这个仓库复制到实验环境中，只需输入：

```bash
cd /home/shiyanlou/
git clone https://github.com/shiyanlou/gitproject
```

操作完成后，会发现 /home/shiyanlou 目录下多了一个 gitproject 文件夹，这个文件夹里的内容就是我们刚刚 clone 下来的代码。

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558611764793)

关于 `Git` 的常用操作我们就学到这里，如果你想学习 `Git` 的更多操作，可以学习以下课程：

[**Git 与 GitHub 入门实践（免费）**](https://www.lanqiao.cn/courses/1035)

[**Git 实战教程 （免费）**](https://www.lanqiao.cn/courses/4)

```checker
- name: 检查是否已经clone
  script: |
    #!/bin/bash
    ls /home/shiyanlou/gitproject
  error: |
    没有发现 git clone 后创建的目录 /home/shiyanlou/gitproject
```

## 总结

回顾一下本节实验学到的内容：

- 版本控制
- Git 和 GitHub 的历史
- 在 GitHub 创建仓库
- 添加修改到暂存区
- 提交代码
- 同步远程仓库

**之后，请你把这些操作整理在脑图中，类似这样：**

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190523-1558613911512)

**🔥 综合练习**

现在有 GitHub 账户，为何不把这几天的学习记录下来呢？

你可以在 GitHub 中新开一个仓库，起名为「Louplus」，然后把这几天的笔记或脑图都放进去，这样做的好处是：

- 方便之后查阅
- 漂亮的学习记录，可以激励你以后的学习
- 这将是你未来求职最好的证明

我们为你准备了四张从 Linux 到 Python 到 Git 的学习脑图，请你试着用今天学到的内容，上传到 `Louplus` 的仓库中:

**💡 步骤:**

1. 在 `GitHub` 的个人主页中新建一个仓库，命名为 `Louplus`

2. 复制仓库链接，在实验环境中，将该仓库克隆到本地
   ![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190524-1558684946974)

3. 进入本地的仓库目录，输入以下命令，下载我们之前的学习脑图（可以用剪切板一口气把四行命令粘贴进去）：

```bash
wget https://labfile.oss.aliyuncs.com/courses/1330/linux.png
wget https://labfile.oss.aliyuncs.com/courses/1330/python1.png
wget https://labfile.oss.aliyuncs.com/courses/1330/python2.png
wget https://labfile.oss.aliyuncs.com/courses/1330/git.png
```

4. 使用 `git add --all` 命令，添加仓库内的所有文件

5. 使用 `git commit`、`git remote`、`git push` 命令，将本地仓库同步到 GitHub 中

完成后，再次刷新你的 `GitHub` 仓库，4 张图片就这么被上传了～

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190524-1558685949102)

**最后，不要忘了做我们的最终挑战～**

**最终挑战很简单**：请你把上个挑战完成的代码文件，传到自己的 `GitHub` 上。

**最终挑战也很重要**：从现在开始，就请养成良好的**代码上传习惯**，不管是学习中的代码、挑战中的代码，都可以上传到 GitHub 上，你的代码仓库将是你未来的财富。

胜利就在前方，加油 ～

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20190514-1557810582507)
