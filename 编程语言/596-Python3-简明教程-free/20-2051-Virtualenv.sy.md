---
show: step
version: 1.0
enable_checker: true
---

# Virtualenv

## 一、实验介绍

Virtualenv 是一个创建隔离 Python 环境的工具，可以帮助你在本地目录安装不同版本 Python 模块的 Python 环境，你可以不再需要在你系统中安装所有东西就能开发并测试你的代码。

#### 实验知识点

- virtualenv 的安装
- 创建虚拟环境
- 激活虚拟环境
- 使用多个虚拟环境
- 关闭虚拟环境

## 二、安装 virtualenv

首先安装 pip3，打开 xfce 终端输入下面的命令：

```sh
sudo apt-get update
sudo apt-get install python3-pip
```

用如下命令安装 virtualenv：

```sh
sudo pip3 install virtualenv
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid212737labid2051timestamp1471425612347.png/wm)

## 三、用法

我们会创建一个叫做 `virtual` 的目录，在里面我们会创建两个不同的虚拟环境。

```sh
cd /home/shiyanlou
mkdir virtual
```

下面的命令创建一个叫做 virt1 的环境。

```sh
cd virtual
virtualenv virt1
```

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20211222-1640144800690)

现在我们激活这个 virt1 环境。

```sh
source virt1/bin/activate
```

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20211222-1640144808494)

提示符的第一部分是当前虚拟环境的名字，当你有多个环境的时候它会帮助你识别你在哪个环境里面。

现在我们将安装 `redis` 这个 Python 模块。

```sh
pip install redis
```

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20211222-1640144821059)

使用 `deactivate` 命令关闭虚拟环境。

```sh
deactivate
```

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20211222-1640144972989)

现在我们将创建另一个虚拟环境 virt2，我们会在里面同样安装 `redis` 模块，但版本是 2.8 的旧版本。

```sh
virtualenv virt2
source virt2/bin/activate
pip install redis==2.8
```

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20211222-1640144830682)

这样可以为你的所有开发需求拥有许多不同的环境。

```checker
- name: 检查是否创建目录 virtual
  script: |
    #!/bin/bash
    ls -l /home/shiyanlou/virtual
  error: |
    我们发现您还没有创建 /home/shiyanlou/virtual
- name: 检查是否创建虚拟环境 virt2
  script: |
    #!/bin/bash
    ls -l /home/shiyanlou/virtual/virt2
  error: |
    我们发现您还没有在 /home/shiyanlou/virtual/ 下创建虚拟环境 virt2
```

## 四、总结

本节知识点回顾：

- virtualenv 的安装
- 创建虚拟环境
- 激活虚拟环境
- 使用多个虚拟环境
- 关闭虚拟环境

永远记住当开发新应用时创建虚拟环境，这会帮助你的系统模块保持干净。
