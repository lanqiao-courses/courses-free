---
show: step
version: 1.0
enable_checker: true
---

# 部署 Nginx Web 服务器

## 部署一个 Web 服务器

#### 什么是 Web 服务器

我们使用浏览器访问网站的时候，网站所在的服务器上就需要运行一个称为 Web 服务器的软件。在浏览器中显示的各种页面，都是通过这个软件发送给我们的。

Nginx 是一种很流行的 Web 服务器软件，具备高性能、高扩展性、高可靠性、低内存消耗等优势。大家访问实验楼的网站（ `www.lanqiao.cn` ），实际上也是访问实验楼的服务器上运行的 Nginx 软件。

现在我们将从常用的配置来入门 Nginx 的使用，然后动手在 Linux 服务器中部署一个 Nginx Web 服务器吧。

#### 知识点

- Nginx 简介
- Nginx 的配置
- 搭建 Web 服务
- 使用 Nginx 模块

点击底部的 ![按钮](https://doc.shiyanlou.com/document-uid1labid6171timestamp1524190021794.png/wm) 开始实验之旅。

## Nginx 是什么

Nginx 是一个 **高性能的代理服务器**，能够反向代理 `HTTP`、 `HTTPS`、`SMTP`、 `POP3`、 `IMAP` ，也可以作为一个负载均衡器和 HTTP 缓存。同时，它还是一个免费的、开源的、高性能的 HTTP 服务器。

**Nginx 以其高性能、稳定性、丰富的特性、以及简单配置和低资源消耗而著称。** Nginx 是由 `Igor Sysoev` 开发设计来供俄罗斯的大型门户网站和搜索引擎 Rambler 的使用。此软件在 BSD-like 协议下发行，可以在 UNIX、GNU/Linux、BSD、Mac OS X、Solaris，和 Microsoft Windows 等操作系统中运行。

**与传统的服务器不同，`Nginx` 不依赖线程来处理请求。** 相反，它使用了一个更具可扩展性的事件驱动（异步）体系结构。这种体系结构使用较小的内存量，但更重要的是，内存的使用量在有负载的时候更加可预测。即使你不希望同时处理数千个请求，但仍然可以从 `Nginx` 的高性能和小内存占用中受益。`Nginx` 在所有方向都可以扩展：从最小的 `VPS（Virtual Private Servers）`到大型的服务器集群。

![nginx](https://doc.shiyanlou.com/courses/uid977658-20191010-1570690105221)

## 初试 Nginx

下面我们将会学习 Nginx 启动与配置。

#### 💡 启动 Nginx

在实验楼的环境中，我们已经为你安装好了 Nginx，你只需要使用下面的命令启动它就可以了~

```bash
sudo service nginx start
```

启动之后，我们看一下 Nginx 是否处于运行状态。

```bash
sudo service nginx status
```

看到下面的结果，就说明正在运行：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570604031212/wm)

#### 💡 查看版本信息

可以查看下本实验环境下的 Nginx 版本和配置参数信息等。

```bash
nginx -V
```

要注意，是大写的 V，不然你只能看到一行输出信息。

但是输出好像密密麻麻的，密集恐惧症都要犯了。别担心，使用重定向、管道和 `sed` 来处理一下输出。

```bash
nginx -V 2>&1 | sed 's/ /\n/g'
```

`2>&1` 的作用是把标准错误的输出重定向到标准输出（其文件描述符为 1），管道 `|` 将上一步命令 `nginx -V 2>&1` 传递给 `sed` 进行处理。处理的方式为 `s/ /\n/g`，它是一个正则表达式，其含义为将空白替换为换行输出。不是很明白？不必担心，这个命令不是我们课程的主线内容，我们只关心输出是否变得易读。

执行命令后的输出是下面这样，是不是好看多了。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570604801366/wm)

继续往下拉，非常的整齐。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570604878906/wm)

这里我们需要关心的是这一行：

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20200804-1596518778873)

它表示，Nginx 已经启用了 `stub_status` 模块，这个模块的使用将会在本实验的最后进行介绍。

## 配置文件

Nginx 及其模块的工作方式是在配置文件中确定的，默认的配置文件（`nginx.conf`）存放在目录 `/etc/nginx` 下。

可以使用下面的命令来查看默认配置文件。

```bash
cat /etc/nginx/nginx.conf
```

内容看上去比较多，但好像很多以 `#` 开头的行（其实是注释），为了看起来更舒服，可以采用下面的方式不显示注释和空白行。

```bash
cat /etc/nginx/nginx.conf | grep -vE "#|^$"
```

grep 去除了带 `#` 的行和 `^$` （即空白行）。同样的，我们不用关心 grep 命令的用法，只需要关心输出是否变得易读。

最后，只剩下了下面的内容：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570605638866/wm)

注意到倒数第二行和倒数第三行的 `include`，它表示将 `/etc/nginx/conf.d/` 目录下以 `.conf` 结尾的文件和目录 `/etc/nginx/site-enabled/` 下的所有文件直接包含进来。你可以理解为将文件的内容直接复制到这里（即 `/etc/nginx/nginx.conf` 中）。

比如我在 `/etc/nginx/conf.d/` 目录中有一个 `test.conf`，它的内容是：

```nginx
server {
    #...
}
```

那么这些内容将替换 `include /etc/nginx/conf.d/*.conf`。`nginx.conf` 的内容，相当于：

```nginx
#...
http {
    #...
    gzip on;
    gzip_disable "msie6"

    server {
        #...
    }

    include /etc/nginx/sites-enabled/*;
}

```

一般来说，Nginx 的配置文件的结构可以抽象成如下示意图：

![图片描述](https://dn-simplecloud.shiyanlou.com/uid/276733/1517307303225.png)

`Main` 就是我们的配置文件，配置文件中的 `events{...}` 对应 `Events`，`http{...}` 对应 `Http`。

在 `nginx.conf` 中是不是没有发现定义的 `server {}`？

原因是：为了方便维护我们 server 相关配置，不会让某一个配置文件过于庞大。通常是将所有的虚拟主机配置文件（也就是 server 配置块的内容）存放在 `/etc/nginx/conf.d/` 或者 `/etc/nginx/sites-enabled/` 目录中，在主配置文件中已经默认声明了会读取这两个文件夹下所有 `*.conf` 文件。

在我们实际的使用中，主要也是配置 server 块的内容，接下来，让我们通过例子来学习它吧~

### server 和 location

#### 💡 server 配置块

一个典型、完整的静态 Web 服务器还会包含多个 server 配置块，例如 `/etc/nginx/sites-enabled/default`。

我们查看它的方式可以参考之前查看 nginx.conf 的方式。

```bash
cd /etc/nginx/sites-enabled/

cat ./default | grep -vE "#|^$"
```

文件的内容如下：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570606515666/wm)

文件中的配置含义在下面的注释中（以 `#` 开头）。

```nginx
# 虚拟主机的配置
server {
    # 侦听 80 端口，分别配置了 IPv4 和 IPv6
	listen 80 default_server;
	listen [::]:80 default_server ipv6only=on;

    # 定义服务器的默认网站根目录位置
	root /usr/share/nginx/html;

    # 定义主页的文件名
	index index.html index.htm;

    # 定义虚拟服务器的名称
	server_name localhost;

    # location 块
    location / {
		try_files $uri $uri/ =404;
	}
}
```

在配置文件中可以看到，如果我们想修改 Server 的端口为 8080，那么就可以修改 `listen 80` 为 `listen 8080`。访问网站的时候应该是 `网站:8080`，其中 `:8080` 表示访问 8080 端口。如果是 80 端口，可以省略不写。

如果我们想更改网站文件存放的位置，修改 `root` 就可以了。

**要注意：各个指令都是以分号结尾的！！！记住这一点可以让你快速找出 “让实验楼网站恢复” 挑战中的错误。**

#### 💡 location 配置块

其中 location 用于匹配请求的 URI。

URI 表示的是访问路径，除域名和协议以外的内容，比如说我访问了 `https://www.lanqiao.cn/louplus/linux`，`https://` 是协议，`www.lanqiao.cn` 是域名，`/louplus/linux` 是 URI。

location 匹配的方式有多种：

- 精准匹配
- 忽略大小写的正则匹配
- 大小写敏感的正则匹配
- 前半部分匹配

其语法如下：

```nginx
location [ = | ~ | ~* | ^~ ] pattern {
#    ......
#    ......
}
```

其中各个符号的含义：

- `=`：用于精准匹配，想请求的 URI 与 pattern 表达式完全匹配的时候才会执行 location 中的操作
- `~`：用于区分大小写的正则匹配；
- `~*`：用于不区分大小写的正则匹配；
- `^~`：用于匹配 URI 的前半部分；

我们以这样的实例来进一步理解：

```bash
location = / {
    # [ 配置 A ]
}

location / {
    # [ 配置 B ]
}

location /documents/ {
    # [ 配置 C ]
}

location ^~ /images/ {
    # [ 配置 D ]
}

location ~* \.(gif|jpg|jpeg)$ {
    # [ 配置 E ]
}
```

- 当访问 `www.shiyanlou.com` 时，请求访问的是 `/`，所以将与配置 A 匹配；
- 当访问 `www.shiyanlou.com/test.html` 时，请求将与配置 B 匹配；
- 当访问 `www.shiyanlou.com/documents/document.html` 时，请求将匹配配置 C;
- 当访问 `www.shiyanlou.com/images/1.gif` 请求将匹配配置 D；
- 当访问 `www.shiyanlou.com/docs/1.jpg` 请求将匹配配置 E。

当一个 URI 能够同时配被多 location 匹配的时候，则按顺序被第一个 location 所匹配。

在 location 中处理请求的方式有很多，如上文中的 `try_files $uri $uri/ =404;`，它是一种特别常用的写法。

我们来分析一下 `try_files $uri $uri/ =404;`。这里假设我定义的 `root` 为 `/usr/share/nginx/html/`，访问的 URI 是 `/hello/shiyanlou`。

- 第一步：当 URI 被匹配后，会先查找 `/usr/share/nginx/html//hello/shiyanlou` 这个文件是否存在，如果存在则返回，不存在进入下一步。
- 第二步：查找 `/usr/share/nginx/html//hello/shiyanlou/` 目录是否存在，如果存在，按 `index` 指定的文件名进行查找，比如 `index.html`，如果存在则返回，不存在就进入下一步。
- 第三步：返回 404 Not Found。

### 尝试创建虚拟服务器

看了这么多，我们也来尝试创建一个虚拟服务器。

#### 💡 准备一下网站文件

首先，我们确定一下网站文件存放在哪里，想来想去，最终决定放在 `/var/myweb/`。

先使用下面命令创建网站根目录：

```bash
sudo mkdir /var/myweb/
```

然后我们需要创建一个 `index.html`

```bash
cd /var/myweb/

sudo touch index.html
```

使用 Vim 编辑器编辑文件。

```bash
sudo vim index.html
```

按 `i` 键进入插入模式。

键入下面的内容：

```html
<html>
  <head>
    <title>my website</title>
  </head>
  <body>
    <h1>Hello, Shiyanlou!</h1>
  </body>
</html>
```

编辑完成后，先按 ECS 键（一般在键盘的左上角），然后在按 `:` 键（键盘上对应为 Shift + `;`）进入到末行模式，再输入 `wq` 即保存并退出。编辑工作完成。

下面给出了整个过程的演示：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570608299290/wm)

#### 💡 开始配置

准备工作就完成了，接下来需要编辑 Nginx 配置文件，这里我们为了不影响原来的配置文件，所以新创建一个。

```bash
cd /etc/nginx/sites-enabled/

sudo touch myweb.conf
```

使用 Vim 编辑器编辑配置文件。

```bash
sudo vim myweb.conf
```

这里，我们监听本地 8070 端口，root 为 `/var/myweb/`，所以需要写入的内容如下：

```bash
server {
	listen 8070 default_server;

	root /var/myweb/;

	index index.html index.htm;

	server_name localhost;

    location / {
		try_files $uri $uri/ =404;
	}
}
```

可以参考下图进行操作：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570609416507/wm)

在重启 Nginx 使配置文件生效之前，我们还应检查一下是否有语法错误：

```bash
sudo nginx -t
```

当看到 OK 字样后，再重新启动 Nginx

```bash
sudo service nginx restart
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570609556998/wm)

最后，打开 Web 浏览器输入 `localhost:8070` 看一下结果吧，是不是有点小激动！

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570609696164/wm)

## 真正的工作者

Nginx 的架构是以高度模块化为设计的基础，除了非常少量的核心代码，其他的一切皆是模块，高度抽象的模块接口，结构的设计简单，使得 Nginx 十分的灵活与高效，默认情况下只会加载默认、必须的模块，其他的一些功能实现需要加载一些第三方的模块。

而配置文件中的各个指令配置项其实便是对模块的一个功能配置。

这里我们以 Nginx 中的 stub_status 模块为例子，它主要用于查看 Nginx 的一些状态信息。它能显示一个状态页，对于想了解 Nginx 的状态以及监控 Nginx 非常有帮助。

为了后续的 zabbix 监控，我们需要学习一下如何对它进行配置。

#### 💡 启用状态页

同样的新建一个 conf 文件

```bash
cd /etc/nginx/sites-enabled/

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

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570610231760/wm)

#### 💡 重启 Nginx

在重启 Nginx 使配置文件生效之前，我们还应检查一下是否有语法错误：

```bash
sudo nginx -t
```

当看到 OK 字样后，再重新启动 Nginx

```bash
sudo service nginx restart
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570610302226/wm)

#### 💡 查看 status

打开浏览器查看 `localhost:8080/nginx_status` 页面。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570610333608/wm)

状态页面中一共提供了七个指标：

- `active connections` – 活跃的连接数量
- `server accepts handled requests` — `accepts` 表示总共处理了 1 个连接，`handled` 表示成功创建 1 次握手，`requests` 表示总共处理了 1 个请求
- `reading`— 读取客户端的连接数
- `writing` — 响应数据到客户端的数量
- `waiting` — 开启 keep-alive 的情况下，这个值等于 active – (reading+writing)，意思就是 Nginx 已经处理完正在等候下一次请求指令的驻留连接

除了浏览器查看，我们还可以通过 `curl` 工具读取。

```bash
curl localhost:8080/nginx_status
```

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20191009-1570610431327/wm)

Nginx 的学习就到此结束啦~

其实 Nginx 还是优秀的代理服务器，想深入学习 Nginx 的同学可以了解一下课程：[Nginx 运维基础入门](https://www.lanqiao.cn/courses/95)。

## 总结

在本次的实验中，我们通过两个配置实战对 Nginx 的使用有了一个初步的认识。

首先我们配置了一个静态服务器，它指向了我们自己的网页文件。在实际使用中，我们的网站通常都是动态的，比如可能是 PHP 的，所以我们还需要使用 PHP 与 Nginx 进行通信，但是限于篇幅，这里没有做讲解。

然后我们使用了 Nginx 中的模块，这为我们后面搭建监控做了准备。

实验到这里就结束啦 ~
