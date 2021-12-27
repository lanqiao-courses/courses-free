---
show: step
version: 1.0
enable_checker: true
---

# Git 标签 tags 和 GitHub 版本 releases

## 实验介绍

#### 知识点

- Git 标签的作用
- 创建标签
- 查看标签
- 推送本地标签至远程仓库
- 删除标签
- 签出标签对应的提交版本
- GitHub releases 简介

## 一、Git 标签的作用

在一个项目中，我们可能需要阶段性地发布一个版本，比如 `V1.0`、`V1.0.2`、`V3.2 Beta` 之类的，Git 的标签可以满足这个需求。在一个长期大型项目中，可能会有数千个提交版本，我们可能需要对重要的节点性提交打个记号，这时也可以使用 Git 的标签功能。在一些项目相关的书籍中，我们会看到 “执行 xxx 命令签出这个版本以查看对应的代码” ，这也是使用 Git 的标签功能做到的。本节实验将详细讲解此功能的具体操作。

### 1.1 创建标签

前面的课程提到过 GitHub 的 issue 功能，issue 是仓库拥有者在 GitHub 上手动创建的，仓库被 Fork 时 issue 不会跟随。Tags 通常在本地使用 git 命令创建后推送到 GitHub 上，与 issue 相同的一点，它也只存在于项目仓库内，Fork 或提 PR 都不会带上它。在多人协作项目中，通常由组长对主仓库设置 Tags，单人项目自然就是自己说了算。

开始操作。首先，克隆仓库、配置信息、查看提交版本历史：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553929421917.png/wm)

重要的一点，我们创建标签是给具体的某次提交创建的，跟分支无关。创建标签使用 `git tag [标签名] -m [备注信息] [提交版本号]` 这个命令。其中 `-m [备注信息]` 可以省略不写，但建议不要省略。`[提交版本号]` 可以省略，如果是给当前分支最新的提交创建标签的话。

给当前分支当前版本创建一个标签：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553930253265.png/wm)

这样一个本地标签就创建完成了。

### 1.2 查看标签

执行 `git tag` 命令显示仓库中的全部标签列表，执行 `git show [标签名]` 查看标签详情：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553930588467.png/wm)

前文已提到，标签是在提交的基础上创建的，如果仓库的多个分支中都有这个提交版本，那么这些分支上就有关于这个提交的相同的标签。

### 1.3 删除本地标签

当我们执行 `git tag [标签名]` 创建本地标签后，在仓库主目录的 `.git/refs/tags` 目录下就会生成一个标签文件：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553931091763.png/wm)

执行 `git tag -d [标签名]` 删除本地标签，标签文件也会被删除：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553931276329.png/wm)

### 1.4 将本地标签推送到远程仓库

首先对两个提交版本创建对应的标签：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553931584680.png/wm)

执行 `git push origin [标签名]` 推送标签到远程仓库，注意前面的命令都只涉及本地操作不需要联网，此命令需要联网：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553931743455.png/wm)

我们到浏览器上打开仓库主目录，点击下图红色框可以查看 releases 和 tags ：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553931964773.png/wm)

点 Tags 按钮查看标签：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553932082005.png/wm)

关于 releases 是什么，下文会介绍。

如果你一口气创建了 6 个标签，当然啦，这种情况很少发生，可以使用 `git push origin --tags` 命令将全部本地标签推送至远程仓库：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553932318259.png/wm)

查看远程仓库情况：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553932396657.png/wm)

### 1.5 删除远程仓库标签

如果标签废弃不用或者写错了，可以使用 `git push origin :refs/tags/[标签名]` 删除远程仓库的标签，命令中的标签名其实也就是文件名：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553932627155.png/wm)

再次查看远程仓库：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553932723902.png/wm)

好，删除成功。以上就是关于 Git 标签的创建、查看、推送、删除的操作流程。

查看本地仓库的标签列表：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553932951532.png/wm)

咦，001 标签还在呢？是的，本地标签需要另外手动删除，上文已演示。

### 1.6 签出版本

现在介绍一下关于 “签出版本” 的操作，我们会见到类似这种说明：“如果你从 GitHub 上克隆了这个程序的仓库，那么可以在仓库主目录下执行 git checkout xxx 签出程序的这个版本。” 其实签出版本就是指定某个提交版本创建一个新的分支。

假定当前的 work 仓库就是一个程序，我们要签出 001 版本，执行以下步骤即可。

首先执行 `git checkout [标签名]` 切换到之前的某个提交版本，然后执行 `git checkout -b [新的分支名]` 将此提交版本固定到一个新分支上并切换到此分支：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid10349timestamp1553933619122.png/wm)

这样就利用标签完成了提交版本签出的工作。

## 二、GitHub 的 releases

GitHub 的 releases 是 2013 年发布的新功能，旨在协助软件开发者分发新版本给用户，关于这个功能这里仅作简单介绍。

当项目组织宣布发布一个软件产品的版本，发布过程就是一个将软件交付给最终用户的工作流。版本是具有修改日志和二进制文件的一类对象，它们提供了 Git 工作流之外的完整项目历史，它们也可以从存储库的主页上被访问。发布版 release 附带发布说明和下载软件或源代码的链接。按照许多 Git 项目的约定，发布版本与 Git 的标签 tag 绑定。您可以使用现有的标签，或者让 release 在发布时创建标签。这就是上面查看 GitHub 仓库中标签信息时出现的场景。

标签是 Git 中的概念，而 releases 则是 Github、码云等源码托管商所提供的更高层的概念。Git 本身是没有 releases 这个概念，只有 tag。两者之间的关系则是，release 基于 tag，为 tag 添加更丰富的信息，一般是编译好的文件。

## 三、总结

本节所讲知识点：

- Git 标签的作用
- 创建标签
- 查看标签
- 推送本地标签至远程仓库
- 删除标签
- 签出标签对应的提交版本
- GitHub releases 简介

以上就是 Git 标签的全部操作和 GitHub releases 的介绍，相对前面的课程而言比较简单，依照文档的步骤操作一遍即可掌握。有任何问题请随时到讨论区里留言。