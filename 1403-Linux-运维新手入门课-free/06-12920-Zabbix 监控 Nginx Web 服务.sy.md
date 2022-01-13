---
show: step
version: 1.0
enable_checker: true
---

# Zabbix 监控 Nginx

## 使用 Zabbix 监控 Nginx

通过前面的学习，我们基本掌握了 Zabbix 监控的搭建，那么这节实验就来进行一下实战。

让我们一起来尝试用 Zabbix 监控 Nginx 服务，通过实战学习 Zabbix 的基本使用。

> 请确保已经配置好了 zabbix 服务。

#### 实验知识点

- Nginx 状态页面配置
- Zabbix 监控 Nginx

## 配置 Nginx 状态页面

在第二个实验部署 Nginx Web 服务器实验中，我们已经学会了 Nginx 的基本使用方式和开启状态页面的方式，本部分是对之前内容的回顾。

Nginx 的配置主要包括两个部分，一是修改默认端口，二是配置状态页。

#### 💡 修改默认端口

由于 Zabbix 使用的 Apache2 已经占用了 80 端口，而 Nginx 的默认端口也是 80，所以我们需要将 Nginx 的 default 配置修改为 80 端口以外的其他端口。

首先，实验楼环境下已经安装了 Nginx，可以查看下本实验环境下的 Nginx 版本和配置参数信息等。

```bash
sudo vim /etc/nginx/conf.d/default.conf
```

将里面的 listen 的那个 80 改成 8090 这个端口或者其他没有用的端口即可。

注意：8080 端口需要配置开启状态页，所以不能使用。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609831852862)

#### 💡 配置状态页

新建一个 conf 文件

```bash
cd /etc/nginx/conf.d/
sudo touch status.conf
```

使用 Vim 编辑器进行编辑。

```bash
sudo vim status.conf
```

键入下面的内容：

```nginx
server {
	listen 8080 default_server;

	server_name localhost;

    location /nginx_status {
        stub_status on;
    }
}
```

它表示监听 8080 端口，对于 URI `/nginx_status`，我们启用模块 `stub_status` 进行响应。

#### 💡 重新启动 nginx

先检查一下是否有语法错误：

```bash
sudo nginx -t
```

当看到 OK 字样后，再重新启动 Nginx

```bash
sudo service nginx start
```

#### 💡 查看状态页

通过 `curl` 工具读取状态页的内容。

```bash
curl localhost:8080/nginx_status
```

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609832016855)

## Shell 脚本入门

因为接下来对 Agent 的配置需要一定的 Shell 知识，所以在这里补充一下。但作为入门课程，我们不会详细地讲解 Shell 脚本的方方面面。

我们可以把 Shell 脚本看做很多命令的集合，它会按照我们规定的顺序执行这些命令。

#### 💡 简单的尝试

首先，我们使用 touch 命令来创建一个 Shell 脚本。

```bash
cd ~
touch test1.sh
```

然后使用 Vim 来编辑它：

```bash
vim test1.sh
```

写入下面的内容

```bash
#!/bin/bash
echo "hello,shiyanlou1"

echo "My Fist Script"
```

在终端中输入下面的命令来执行我们编写的脚本

```bash
bash test1.sh
```

完整的演示：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191011-1570774780869/wm)

#### 💡 变量

和第一个脚本采用的方式相同，先 `touch test2.sh`，再调用 `vim test2.sh` 进行编辑。输入的脚本内容如下：

```bash
#!/bin/bash
hello="hello,shiyanlou2"
echo $hello
```

它定义了一个变量 `hello`，它的值为 `hello,shiyanlou2` ，最后我们打印了这个变量的值。当一个相同的值被多次使用时，定义变量是最好的方式，因为只需修改变量的值，所有引用变量值的地方就都被修改啦~

**注意：`=` 的前后不能有空格。**

使用下面的命令进行执行。

```bash
bash test2.sh
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191011-1570775011824/wm)

#### 💡 传入参数

变量的值是被写入到文件中，如果我们想根据命令的不同来让脚本有不同的行为，就必须采用传入参数的方式。

让我们通过实战来了解一下如何传入参数。新建一个 `test3.sh`，写入下面的内容。

```bash
#!/bin/bash
echo $1
```

这里 `$1` 就是我们传入的第一个参数。

输入下面的命令执行脚本。

```bash
bash test3.sh "hello,shiyanlou3"
```

这里 `"hello,shiyanlou3"` 就是我们传入的参数，注意 `"` （引号）是不会被打印的。

执行的结果如下：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191011-1570775283723/wm)

#### 💡 函数

为了使相同的工作不再重复，我们通常定义函数来将相同的操作写到一个函数中，比如下面的代码中就定义了一个名为 `hello` 的函数。

```bash
#!/bin/bash
function hello {
  echo "hello,shiyanlou4"
}

$1
```

只定义一个函数是不会进行任何操作的，我们必须要调用函数，这样它才会执行。当我们调用函数时，它就会执行函数体中的内容，这里的函数体是 `echo "hello,shiyanlou4"`。

为什么最后有 `$1` 呢？这其实是我们调用函数的方式，不过这里我们从命令行传入要调用的函数。如果我们执行的命令为 `/bin/bash test4.sh hello`，那么脚本就相当于：

```bash
#!/bin/bash
function hello {
  echo "hello,shiyanlou4"
}

hello
```

这里直接写 `hello` 就意味着调用 `hello` 函数。

创建一个 `test4.sh` 来看一下结果吧：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191011-1570775621753/wm)

### 配置 Agent

#### 💡 添加自定义参数

在前面的学习中我们知道对服务的监控需要通过 Agent 去采集数据，所以下面就该对 Agent 进行配置。

在 `/etc/zabbix/zabbix_agentd.conf` 配置文件中添加自定义参数，让 Agent 使用脚本来采集 Nginx 的数据，然后发送给 server。

```bash
sudo vim /etc/zabbix/zabbix_agentd.conf
```

需要在末尾添加的内容如下：

```bash
UserParameter=nginx.status[*],/bin/bash /etc/zabbix/zabbix_agentd.d/nginx_status.sh $1
```

这里我们添加一个自定义的 `UserParameter`。我们详细讲解一下自定义参数的含义。

有时候我们想让被监控端执行一个 Zabbix 没有预定义的检测，Zabbix 的用户自定义参数功能提供了这个方法。

其语法规范为：

```bash
UserParameter=key,command
```

`key` 使我们定义的参数，它必须是唯一的，`[*]` 表示里面可以传递多个参数。

如果 Zabbix 调用 key，就会执行对应的 command（命令）。比如这里我们调用的 key 为 `nginx.status['active']`，那么执行的命令为：`/bin/bash /etc/zabbix/zabbix_agentd.d/nginx_status.sh active`，这里的 `$1` 会被替换为 key 中传入的第一个参数。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570615584478/wm)

#### 💡 采集脚本

接着我们来编写下用于采集数据的信息的脚本，新打开一个终端，输入如下命令：

```bash
sudo vim /etc/zabbix/zabbix_agentd.d/nginx_status.sh
```

脚本的内容如下，它定义了许多 Shell 函数，是用来采集信息的关键。脚本的结构非常简单，定义了一些变量用来记录一些函数公用的值。

每个函数的内容都是相似的，比如 `/usr/bin/curl "http://$HOST:$PORT/nginx_status" 2>/dev/null| grep 'Active' | awk '{print $NF}'`。看起来非常长，但是仔细一分析你会发现这个命令并不复杂。

`curl` 从一个链接 `"http://$HOST:$PORT/nginx_status"`（即我们的 Nginx 状态页）获取数据，然后传递给 grep 筛选，而我们最终只需要它的值（即对应的项的数值），所以又交给 `awk` 进行处理。

`awk` 是一个常用的文本处理器，它的用法已经多到可以单独出一本书。限于篇幅，这里我们不进行展开讲解，只需要知道它的功能就是获取对应监控项的值就可以了。

在函数的最后，我们使用 `$1` 来获取要执行的函数（在 Shell 脚本入门一小节中我们已经讲解过了，这里就不再重复它的作用）。

```bash
#!/bin/bash

# 设置变量
BKUP_DATE=`/bin/date +%Y%m%d`
LOG="/data/log/zabbix/webstatus.log"
HOST=127.0.0.1  # 确保 CURL 能访问这个主机的 IP 地址
PORT="8080"    # 端口号

# 编写函数用于获取 nginx 的统计信息
function active {
  /usr/bin/curl "http://$HOST:$PORT/nginx_status" 2>/dev/null| grep 'Active' | awk '{print $NF}'
  }
function reading {
  /usr/bin/curl "http://$HOST:$PORT/nginx_status" 2>/dev/null| grep 'Reading' | awk '{print $2}'
  }
function writing {
  /usr/bin/curl "http://$HOST:$PORT/nginx_status" 2>/dev/null| grep 'Writing' | awk '{print $4}'
}

function waiting {
  /usr/bin/curl "http://$HOST:$PORT/nginx_status" 2>/dev/null| grep 'Waiting' | awk '{print $6}'
}

function accepts {
  /usr/bin/curl "http://$HOST:$PORT/nginx_status" 2>/dev/null| awk NR==3 | awk '{print $1}'
}

function handled {
  /usr/bin/curl "http://$HOST:$PORT/nginx_status" 2>/dev/null| awk NR==3 | awk '{print $2}'
}

function requests {
  /usr/bin/curl "http://$HOST:$PORT/nginx_status" 2>/dev/null| awk NR==3 | awk '{print $3}'
}

$1
```

添加脚本的过程和 Shell 脚本入门一小节中是相似的。可以参考下面的演示 ↓

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570616037249/wm)

添加好采集脚本之后，启动 Zabbix Agent 服务。

```bash
sudo /usr/sbin/zabbix_agentd --foreground -c /etc/zabbix/zabbix_agentd.conf
```

### 测试数据采集

Zabbix Agent 监控代理获取（采集）数据可以通过 `zabbix_get` 进程来取得。可以用 `zabbix-get` 来测试一下数据的采集是否成功。

这里我们需要安装一下 `zabbix-get` 这个软件包。

```bash
sudo apt-get install zabbix-get
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570616238687/wm)

测试的命令如下：

```bash
sudo zabbix_get -s 127.0.0.1 -k 'nginx.status[accepts]'

sudo zabbix_get -s 127.0.0.1 -k 'nginx.status[handled]'

sudo zabbix_get -s 127.0.0.1 -k 'nginx.status[requests]'

sudo zabbix_get -s 127.0.0.1 -k 'nginx.status[active]'

sudo zabbix_get -s 127.0.0.1 -k 'nginx.status[reading]'

sudo zabbix_get -s 127.0.0.1 -k 'nginx.status[writing]'

sudo zabbix_get -s 127.0.0.1 -k 'nginx.status[waiting]'
```

> `-s`: 指定客户端主机名或者 IP 。由于是采集本机的数据，可以使用回环地址 `127.0.0.1` 或者 `localhost`。
>
> `-k`: 你想获取的 key 。这里的 Key 是之前在 `UserParameter` 中定义的。

返回一个数据则表示配置成功，如下是测试结果，你得到的值不一定与图片中的相同。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191010-1570684183786)

## Zabbix 监控配置

#### 💡 配置流程

下面就是 Zabbix 监控系统的配置步骤。

大体的流程是：

- （1）添加要监控的 Agent（这一步骤在上一个实验中已经完成，如果你没有使用保存的环境，就需要重新进行配置）。
- （2）给主机添加监控项，即我们想要监控的指标。
- （3）将我们监控项按组划分为两个图形。
- （4）将图形汇总到一个聚合图形上。

#### 💡 指标分组

在开始进行配置之前，我们先将七个指标进行分组。

回顾一下，之前 `nginx_status` 的七个指标：

- `active connections` – 活跃的连接数量
- `server accepts handled requests` — `accepts` 表示总共处理的连接数，`handled` 表示成功创建握手的次数，`requests` 表示总共处理的请求数
- `reading` — 读取客户端的连接数
- `writing` — 响应数据到客户端的数量
- `waiting` — Nginx 已经处理完正在等候下一次请求指令的驻留连接数

我们将其分为两个组

- 与连接相关的：`active`、`reading`、`writing`、`waiting`
- 与处理请求相关的：`accepts`、`handled`、`requests`

### 创建监控项

首先打开浏览器，在地址栏输入 `localhost/zabbix` 进入 Zabbix 的前端。

依次点击菜单栏的 “配置” → “主机”，然后选择主机 localhost 的“监控项”。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609832952752)

点击之后进入到监控项页面，点击右上角的 “创建监控项” 按钮。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833013743)

接下来，我们要分别为 7 个指标添加监控项。创建监控项的方式都是大同小异的，我们只会详细的讲解 `nginx.accepts` 的配置，其他的配置与其是相似的。

#### 💡 创建与连接相关的监控项

- `nginx.accepts`

我们需要填写监控项的名字：`nginx.accepts`。然后要填写键值 `nginx.status[accepts]`。更新时间改为 `1s`。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833107986)

填写好以后，下拉页面到底部，点击添加即可。

- nginx.handled

按照同样步骤添加监控项 `nginx.handled`：填写监控项的名字：`nginx.handled`。然后要填写键值 `nginx.status[handled]`。更新时间改为 `1s`。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833302307)

- nginx.requests

添加监控项 `nginx.requests`：填写监控项的名字：`nginx.requests`。然后要填写键值 `nginx.status[requests]`。更新时间改为 `1s`。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833359675)

#### 💡 创建与处理请求相关的监控项

- nginx.active

添加监控项 `nginx.active`：填写监控项的名字：`nginx.active`。然后要填写键值 `nginx.status[active]`。更新时间改为 `1s`。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833436164)

- nginx.reading

添加监控项 `nginx.reading`：填写监控项的名字：`nginx.reading`。然后要填写键值 `nginx.status[reading]`。更新时间改为 `1s`。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833501341)

- nginx.writing

添加监控项 `nginx.writing`：填写监控项的名字：`nginx.writing`。然后要填写键值 `nginx.status[writing]`。更新时间改为 `1s`。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833565964)

- nginx.waiting

添加监控项 `nginx.waiting`：填写监控项的名字：`nginx.waiting`。然后要填写键值 `nginx.status[waiting]`。更新时间改为 `1s`。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833620821)

配置完成后，状态栏显示是 `已启动`，如下图：

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833653436)

这表明我们添加的监控项已经成功启用。

### 创建图形

接下来，我们需要为两组监控项分别创建图形。

如下图，点击主机 localhost 的“图形”，再点击右上角“创建图形”。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833727270)

我们需要依次创建两个图形：`nginx_connect` 和 `nginx_interact`。

- 💡 nginx_connect

填写图形的名称为 `nginx_connect`，然后将监控项添加到图形中。添加监控项的方式如下。注意我们要添加的监控项为 `nginx.accepts`、`nginx.handled`、`nginx.requests`

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833798550)

添加完成后，点击底部的 “添加” 按钮。

- 💡 nginx_interact

nginx_interact 的配置方式与 nginx_connect 是相似的。要注意：图形命名为 `nginx_interact`；需要添加的监控项为：`nginx.active`、`nginx.reading`、`nginx.writing`、`nginx.waiting`。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833935033)

添加完成后，可以在图形列表中查看。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609833964704)

### 创建聚合图形

依次点击顶部的 “监测” → “聚合图形”，再点击 “创建聚合图形”。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609834021626)

创建一个名为 `Nginx Status` 的聚合图形，配置为 1 列 2 行。配置完成后，点击 “添加” 按钮。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609834085121)

选择 “构造函数” 就会进入到配置界面，将之前的两个图形添加到聚合图形中。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609834125266)

点击更改，然后在图形里选择要添加的图形 nginx_connect。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609834292339)

同样的方式添加另外一个图形，注意要点击第二个更改。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609834383389)

再次回到聚合图形的界面，点击 `Nginx Status`（即聚合图形的名字）。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609834434628)

点击之后就能看到我们的监控数据。

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20210105-1609834467309)

配置 Zabbix 监控 Nginx 的实验到此就结束了。

## 总结

本节实验中，我们通过实战 Zabbix 监控 Nginx Web 服务器来学习 Zabbix 添加服务监控的基本步骤。

回顾本节实验的知识点：

- Nginx 服务配置
- 配置 Zabbix 监控 Nginx

本课程到此就结束了。如果你想学习更多 DevOps 知识，可以关注 [楼+ 之 Linux 运维与 DevOps 实战](https://www.lanqiao.cn/louplus/linux)。

**💡 一个人孤零零地学习编程，很容易“从入门到放弃”，更好的方法是和朋友一起学习～欢迎加入我们的「Linux 学习交流群」，一群志同道合的小伙伴在等着你～扫码添加班主任微信 （sylmm003） 即可进群 😉。**

![图片描述](https://doc.shiyanlou.com/courses/uid8504-20191023-1571815193664)
