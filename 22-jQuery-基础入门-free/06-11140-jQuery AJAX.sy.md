---
show: step
version: 1.0
enable_checker: true
---

# jQuery AJAX

## 一、实验介绍

#### 1.1 实验内容

本节实验，我们将来介绍 jQuery 中的 ajax。

#### 1.2 实验知识点

- 原生 AJAX
- jQuery AJAX

#### 1.3 实验环境

- `Theia`: 一款前后端分离的、基于 web 的 云 IDE。
- `Preview 或 Mini Browser`：浏览器，右键点击目标 html 文件，选择 open with 下的 Preview 或 Mini Browser 即可在 IDE 中打开浏览器。

#### 1.4 参考链接

[w3c jQuery ajax](http://www.w3school.com.cn/jquery/jquery_ref_ajax.asp)

## 二、原生 AJAX

AJAX = 异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。它并不是一种单一的技术，而是有机地利用了一系列交互式网页应用相关的技术所形成的结合体。简短地说，在不重载整个网页的情况下，AJAX 通过后台加载数据，并在网页上进行显示，其最大的特点就是异步。

### 2.1 AJAX 的优势与劣势

#### 2.1.1 优势

- 不需要插件支持
- 优秀的用户体验
- 提高 Web 程序的性能
- 减轻服务器和带宽的负担

#### 2.1.2 劣势

- 浏览器对 XMLHttpRequest 对象支持度不足(关于 XMLHttpRequest 对象稍后我们会介绍到)
- 破坏浏览器前进、“后退”按钮的正常功能
- 对搜索引擎的支持的不足
- 开发和调试工具的缺乏

### 2.2 AJAX 的 XMLHttpRequest 对象

现在，我们正式来看一看原生的 AJAX。AJAX 的核心是 XMLHttpRequest 对象，它的实现方式非常多，但是绝大多数的浏览器都提供了类似的属性和方法。目前 W3C 组织正致力于制定一个各浏览器厂商都可以遵循的 XMLHttpRequest 对象标准，来推进 AJAX 技术发展。

### 2.3 创建 XMLHttpRequest 对象

既然 XMLHttpRequest 是一个对象，那么我们在使用的时候就要先创建这个对象，所有现代浏览器都支持 XMLHttpRequest 对象(IE5 IE6 使用 ActiveXObject)。

创建 XMLHttpRequest 对象的语法：

```js
var xhr = new XMLHttpRequest();
```

IE5 和 IE6 的语法：

```js
var xhr = new ActiveXObject('Microsoft.XMLHTTP');
```

### 2.4 发起 XMLHttpRequest 请求

创建完对象后，我们就可以使用 XMLHttpRequest 对象向服务器发送请求，我们模拟了一个后端程序，在调试本节实验的示例时，都需要将这个后端程序运行起来:

在终端中输入以下命令

```bash
$ wget https://labfile.oss.aliyuncs.com/courses/1318/server.zip

$ unzip server.zip

$ rm server.zip

$ cd server

$ npm start # 启动后端程序
```

终端出现以下界面表示后端程序启动成功，我们可以朝服务端发起请求了。

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077178745)

我们会使用 XMLHttpRequest 对象的 open() 和 send() 方法来将请求发送到服务器。

| 方法                   | 描述                                                                                                                                                                                           |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| open(method,url,async) | 规定请求的类型、URL 以及是否异步处理请求。<br /> method: 请求的类型，http 的请求类型，如 GET、POST。 <br /> url: 请求的服务端路径 <br /> async: Boolean 类型，true (异步处理)，false(同步处理) |
| send(string)           | 将请求发送到服务端。 <br /> string：仅用于 post 请求。                                                                                                                                         |

我们先来看一个简单的 GET 请求:

```js
xhr.open('GET', 'index.html', true);
xhr.send();
```

xhr 是我们之前创建的 XMLHttpRequest 对象的实例，通过调用 XMLHttpRequest 对象的 open 和 send 方法，就实现了一个简单的向路径 `index.html` 发起的 GET 请求，异步处理。因为 GET 请求的 header 中是没有参数的，所以 send() 里的值为 null。如果想要加上参数则需要在请求的 url 后添加，如：

```js
xhr.open('GET', 'http://helloworld.com?id=1', 'true');
xhr.send();
```

我们再来看一个 post 请求：

```js
xhr.open('POST', 'index.php', true);
xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
xhr.send('name=syl&id=2');
```

如果我们想像 HTML 表单那样 POST 数据，则需要使用 setRequestHeader() 来添加 HTTP 头，然后在 send 中规定要发送的数据。

在上面的两个例子中，async 我们都是用了 true，使用 true 和 false 有什么区别呢？

我们现在只是发送了请求，一个完整的 AJAX 过程还要包括服务端返回信息，这个过程的时间就是我们无法估计的了，谁都没法确定服务端会什么时候返回信息，所以如果 async 设为 true，当发送了 AJAX 请求后，js 无需等待服务端的响应，而是会去处理其它的脚本，等到服务端响应就绪的时候，js 会返回对 AJAX 中的剩余部分作相应处理，这个相应处理需要通过 `onreadystatechange` 来实现，我们会在下一部分中介绍。如果 async 设为 false，当然，一般不推荐使用 async = false，如果你这么使用了，那么请不要编写 `onreadystatechange` 函数，否则 js 会挂起在这个函数的地方，等待服务端响应，如果服务端瘫痪了，那么你的脚本就会无法运行。所以如果使用了 async = false，把处理代码放到 send() 后面即可。例如：

```js
xhr.open('GET', 'index.text', false);
xhr.send();
console.log(xhr.responseText);
```

### 2.5 服务端响应及 onreadystatechange 事件

在前面的部分中，我们完成了发送请求到服务端的工作，现在我们要执行一些基于服务端响应的任务了，而这个响应就是 readyState。

下面是 XMLHttpRequest 的三个重要的属性：

| 属性               | 描述                                                                                                                                                                                                 |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| onreadystatechange | 存储函数，每当 readyState 属性改变时，就会调用这个函数。                                                                                                                                             |
| readyState         | 这个变量储存了 XMLHttpRequest 的状态，有 0-4 并且分别对应不同的含义。 <br /><br /> 0：请求未初始化 <br />1:服务器连接已建立 <br /> 2:请求已接受 <br />3:请求处理中 <br /> 4:请求已完成，且响应已就绪 |
| status             | 200："OK" <br /><br /> 404：未找到页面                                                                                                                                                               |

一般情况下，我们需要处理的就是 `readyState == 4 && status == 200` 的情况，所以 onreadystatechange ，用于处理服务端响应的方法一般写法如下：

```js
xhr.onreadystatechange = function () {
  if (xhr.readyState == 4 && xhr.status == 200) {
    //code 前端处理代码
  }
};
```

那么我们该如何获取来自服务端的响应呢？ XMLHttpRequest 提供了两种方式：

| 属性         | 描述                     |
| ------------ | ------------------------ |
| responseText | 获得字符串形式的响应数据 |
| responseXML  | 获得 XML 形式的相应数据  |

用法示例如下：

```js
xhr.onreadystatechange = function () {
  if (xhr.readyState == 4 && xhr.status == 200) {
    console.log(xhr.responseText);
  }
};
```

关于原生 AJAX 更多的知识可以查看 w3c 等文档。下面给出一个完整的原生 AJAX 调用的例子。

**注意:本节实验中的 url 都需要根据实验楼的环境进行修改，方法如下: <br /><br /> 首先要确保已经运行了之前给出的后端程序，然后点击显示工具栏 -> Web 服务，将弹出的页面的 url 复制粘贴到代码对应位置即可，注意粘贴后的 url 有一个斜杠，代码中已经给出的地址也有一个斜杠，需要删去一个。**

下面我们给出一个原生的 AJAX 请求示例，在 /project 下新建一个 index.html 文件，键入以下内容，后续给出的示例都按这种方式操作。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <script>
      $(document).ready(function () {
        var xhr = null;

        //创建对象
        if (window.ActiveXObject) xhr = new ActiveXObject('Microsoft.XMLHTTP');
        else if (window.XMLHttpRequest) xhr = new XMLHttpRequest();

        //发送请求
        xhr.open('GET', 'https://042bc5dd9bf0.simplelab.cn/xhrtest', true);
        //上面是在实验楼环境里运行过的 url，你需要根据注意里的步骤，将后端运行起来后，把web 服务页面中的 url 替换到对应位置，下面一行注释给出了需要替换的位置。
        //xhr.open("GET","需要将 web 服务中 url 粘贴到这里/xhrtest",true);
        xhr.send();

        //处理响应
        xhr.onreadystatechange = function () {
          if (xhr.readyState == 4 && xhr.status == 200) {
            alert(xhr.responseText);
          }
        };
      });
    </script>
  </body>
</html>
```

确保后端程序运行后，将该 html 文件按照之前的方式打开即可查看效果。

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077251487)

## 三、jQuery Ajax

是不是觉得原生的 AJAX 有一些繁琐？不用担心，jQuery 对 AJAX 操作进行了封装。

### 3.1 load()、\$.get()、\$.post()

我们先来介绍常用的几个方法：

#### 3.1.1 load()

load() 是 jQuery 中最简单和常用的 AJAX 方法，语法为：

```js
load(url, [data], [callback]);
```

| 参数           | 类型     | 描述                                                                                                                                                                            |
| -------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| url            | String   | 请求的 url 地址                                                                                                                                                                 |
| data(可选)     | Object   | 发送到服务器的数据                                                                                                                                                              |
| callback(可选) | Function | 请求完成时的回调函数，无论成功失败 <br /><br /> function 有三个额外参数<br/><br/>response 包含来自请求的结果数据 <br/> status 包含请求的状态 <br/> xhr 包含 XMLHttpRequest 对象 |

下面这个示例加载了一个 html 文件到页面中来

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div id="result"></div>
    <script>
      $(document).ready(function () {
        $('#result').load('https://042bc5dd9bf0.simplelab.cn/load.html');
        //上面是在实验楼环境里运行过的 url，你需要根据注意里的步骤，将后端运行起来后，把web 服务页面中的 url 替换到对应位置，下面一行注释给出了需要替换的位置。
        //$("#result").load("需要将 web 服务中 url 粘贴到这里/load.html");
      });
    </script>
  </body>
</html>
```

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077287375)

我们只使用了参数 url，你可以将其它参数加入进来查看效果，例如加入回调函数：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div id="result"></div>
    <script>
      $(document).ready(function () {
        //下面是在实验楼环境里运行过的 url，你需要根据注意里的步骤，将后端运行起来后，把web 服务页面中的 url 替换到对应位置，下面一行注释给出了需要替换的位置。
        //"需要将 web 服务中 url 粘贴到这里/load.html"
        $('#result').load(
          'https://042bc5dd9bf0.simplelab.cn/load.html',
          function (response, status, xhr) {
            alert(status);
          }
        );
      });
    </script>
  </body>
</html>
```

使用回调函数弹出了 status。

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077331545)

#### 3.1.2 \$.get()

get() 通过远程 HTTP GET 请求载入信息。语法：

```js
$(selector).get(url, data, success(response, status, xhr), dataType);
```

| 参数                         | 描述                                                                                                                        |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| url                          | 必须，请求的 url                                                                                                            |
| data                         | 可选，发送到服务器的数据                                                                                                    |
| success(response,status,xhr) | 可选，当请求成功时运行该方法 <br/> response-包含来自请求的结果数据 <br/>status-包含请求的状态 <br/> xhr-XMLHttpRequest 对象 |
| dataType                     | 可选，规定预计接收服务端的响应的数据，可能的类型有 xml、html、text、script、json                                            |

get() 方法是 ajax 方法的简写形式，我们还没有介绍 ajax 方法，但是这里我们给出 get 等价的 ajax 形式：

```js
$.ajax({
  type: 'GET',
  url: url,
  data: data,
  success: success,
  dataType: dataType,
});
```

下面是一个简单的 get() 方法的示例：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <button id="btn">click me!</button>
    <script>
      $(document).ready(function () {
        $('#btn').click(function () {
          //下面是在实验楼环境里运行过的 url，你需要根据注意里的步骤，将后端运行起来后，把web 服务页面中的 url 替换到对应位置，下面一行注释给出了需要替换的位置。
          //"需要将 web 服务中 url 粘贴到这里/gettest"
          $.get('https://042bc5dd9bf0.simplelab.cn/gettest', function (result) {
            alert(result);
          });
        });
      });
    </script>
  </body>
</html>
```

点击 click me 按钮，就会发出一个 get 请求，请求成功后，会调用 function 回调函数弹出结果。

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077360497)

#### 3.1.3 \$.post()

post() 方法通过 HTTP POST 请求从服务器载入数据，post 方法和 get 方法比较类似。但还是存在一些区别：

- get 请求会将参数跟在 url 后面进行传递，而 post 请求的参数则是作为 HTTP 的消息的实体内容，在 AJAX 请求中，这种区别对用户是不可见的。
- get 请求对传输的数据有大小限制，一般是小于 2KB 的，而 post 理论上没有限制。
- get 请求的数据会被浏览器缓存起来，一些敏感数据同样会被缓存，而 post 则可以避免这个问题。

语法:

```js
jQuery.post(url, data, success(data, textStatus, jqXHR), dataType);
```

| 参数                           | 描述                                                                                                                          |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| url                            | 必须，请求的 url                                                                                                              |
| data                           | 可选，发送到服务器的数据                                                                                                      |
| success(data,textStatus,jqXHR) | 可选，当请求成功时运行该方法 <br/> data-包含来自请求的结果数据 <br/>textStatus-包含请求的状态 <br/> jqXHR-XMLHttpRequest 对象 |
| dataType                       | 可选，规定预计接收服务端的响应的数据，可能的类型有 xml、html、text、script、json                                              |

post() 方法是 ajax 方法的简写形式，我们还没有介绍 ajax 方法，但是这里我们给出 post 等价的 ajax 形式：

```js
$.ajax({
  type: 'POST',
  url: url,
  data: data,
  success: success,
  dataType: dataType,
});
```

下面是一个简单的 post 的示例

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <button id="btn">click me</button>
    <script>
      var data = {
        name: 'syl',
        id: 1,
      };
      $(document).ready(function () {
        $('#btn').click(function () {
          //下面是在实验楼环境里运行过的 url，你需要根据注意里的步骤，将后端运行起来后，把web 服务页面中的 url 替换到对应位置，下面一行注释给出了需要替换的位置。
          //"需要将 web 服务中 url 粘贴到这里/posttest"
          $.post('https://042bc5dd9bf0.simplelab.cn/posttest', data, function (
            result
          ) {
            alert(result);
          });
        });
      });
    </script>
  </body>
</html>
```

我们要发送的数据为：

```js
var data = {
  name: 'syl',
  id: 1,
};
```

点击按钮，将数据发送到服务端，经过服务端的逻辑处理后，返回了处理后的数据，如下图：

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077387525)

你可能已经发现了，post 和 get 方法只存在方法名的区别。另外，当 load() 方法带有参数传递的时候，会使用 POST 方式发送请求，因此，上述代码我们可以使用 load 来改写：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <button id="btn">click me</button>
    <div id="result"></div>
    <script>
      var data = {
        name: 'syl',
        id: 1,
      };
      $(document).ready(function () {
        $('#btn').click(function () {
          //下面是在实验楼环境里运行过的 url，你需要根据注意里的步骤，将后端运行起来后，把web 服务页面中的 url 替换到对应位置，下面一行注释给出了需要替换的位置。
          //"需要将 web 服务中 url 粘贴到这里/posttest"
          $('#result').load(
            'https://042bc5dd9bf0.simplelab.cn/posttest',
            data,
            function (result) {
              alert(result);
            }
          );
        });
      });
    </script>
  </body>
</html>
```

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077410132)

### 3.2 \$.getScript() 、 \$.getJSON()

#### 3.2.1 \$.getScript()

有时，在页面第一次加载时就获取所需的全部的 JavaScript 文件并没有必要，虽然可以在需要哪个 JS 文件时动态的创建 script 标签，但是这种动态创建的方法并不理想，为此 jQuery 提供了 \$.getScript() 方法来直接加载 js 文件，加载的 js 文件会自动执行。getScript 通过 HTTP GET 请求载入并执行 JavaScript 文件，语法：

```js
jQuery.getScript(url, success(response, status));
```

| 参数                     | 描述                                                                                                   |
| ------------------------ | ------------------------------------------------------------------------------------------------------ |
| url                      | 请求的 url 地址                                                                                        |
| success(response,status) | 可选，请求成功后执行的回调函数。<br/><br/> response-包含来自请求的结果数据 <br/> status-包含请求的状态 |

这个函数同样是简写的 ajax 函数，等价于

```js
$.ajax({
  type: 'GET',
  url: url,
  dataType: 'script',
  success: success,
});
```

下面我们来看一个简单的示例，通过 \$.getScript() 方法来动态的加载一段 js 脚本。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <button id="btn">click me</button>
    <script>
      $(document).ready(function () {
        $('#btn').click(function () {
          $.getScript('https://042bc5dd9bf0.simplelab.cn/getscripttest');
          //上面是在实验楼环境里运行过的 url，你需要根据注意里的步骤，将后端运行起来后，把web 服务页面中的 url 替换到对应位置，下面一行注释给出了需要替换的位置。
          //"需要将 web 服务中 url 粘贴到这里/getscripttest"
        });
      });
    </script>
  </body>
</html>
```

点击按钮，可以看到出现了一个弹框，这个逻辑是我们请求得到的 js 文件中写的。

```js
alert('This file was loaded dynamically by method getScript()!');
```

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077439662)

#### 3.2.2 \$.getJSON()

\$.getJson() 用于加载 JSON 文件，与 \$.getScript() 的用法相同。语法：

```js
jQuery.getJSON(url, data, success(data, status, xhr));
```

| 参数                     | 描述                                                                                                                             |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| url                      | 请求的 url 地址                                                                                                                  |
| data                     | 可选，发送到服务端的数据                                                                                                         |
| success(data,status,xhr) | 可选，请求成功后执行的回调函数。<br/><br/> data-包含来自请求的结果数据 <br/> status-包含请求的状态 <br/> xhr-XMLHttpRequest 对象 |

该函数还是简写的 Ajax 函数，等价于：

```js
$.ajax({
  type: 'GET',
  url: url,
  data: data,
  success: callback,
  dataType: json,
});
```

发送到服务器的数据 data 可作为查询字符串附加到 URL 之后。传递给 success 的返回数据 data 可以是 js 对象，也可以是 json 结构的数组，需要使用 \$.parseJSON() 进行解析。

下面给出一个简单的示例：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <button id="btn">click me</button>
    <script>
      $(document).ready(function () {
        $('#btn').click(function () {
          //下面是在实验楼环境里运行过的 url，你需要根据注意里的步骤，将后端运行起来后，把web 服务页面中的 url 替换到对应位置，下面一行注释给出了需要替换的位置。
          //"需要将 web 服务中 url 粘贴到这里/getjsontest"
          $.getJSON('https://042bc5dd9bf0.simplelab.cn/getjsontest', function (
            data
          ) {
            alert(
              typeof data +
                '\n' +
                data[0].id +
                ' ' +
                data[0].name +
                '\n' +
                data[1].id +
                ' ' +
                data[1].name +
                '\n' +
                data[2].id +
                ' ' +
                data[2].name +
                '\n'
            );
          });
        });
      });
    </script>
  </body>
</html>
```

我们向服务器发送了一个 get 请求，请求返回 JSON 数据，结果如下图：

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077461481)

可以看到返回的是一个 object 类型的数据，符合结果。

### 四、\$.ajax()

\$.ajax 是 jQuery 最底层的实现，它的语法为：

```js
\$.ajax(options);
```

options 是一个 `{}`,里面包含了 ajax 请求的参数，我们之前介绍的方法都可以通过 ajax 来实现，下面我们来看一看常用的参数：

| 参数        | 类型             | 描述                                                                                                                                                                                             |
| ----------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| url         | String           | 发送请求的地址                                                                                                                                                                                   |
| type        | String           | 请求的方式，例如我们之前使用过的 GET、POST，除此之外，还有其他的 HTTP 方法                                                                                                                       |
| timeout     | Number           | 设置请求超时时间(毫秒)                                                                                                                                                                           |
| data        | Object 或 String | 发送到服务器的数据                                                                                                                                                                               |
| dataType    | String           | 预期服务器返回的数据类型，如果不指定，jQuery 将自动根据 HTTP 包 MIME 信息返回 responseXML 或 responseText，并作为回调函数参数传递，有如下这些常用类型：xml、html、script、json、jsonp、text 等。 |
| beforeSend  | Function         | 发送请求前可以修改 XMLHttpRequest 对象的函数，比如可以添加自定义的 HTTP 请求头，判读是否发送本次 Ajax 请求等，XMLHttpRequest 对象是唯一的参数                                                    |
| complete    | Function         | 请求完成后调用的回调函数(请求成功或失败都要调用该函数) ，参数：XMLHttpRequest 对象和描述成功请求类型的字符串 textStatus                                                                          |
| success     | Function         | 请求成功后调用的回调函数，参数：由服务器返回，并根据 dataType 处理后的数据，还有一个是 textStatus                                                                                                |
| error       | Function         | 请求失败时被调用的函数，参数：XMLHttpRequest 对象、错误信息、捕获的错误对象(可选)                                                                                                                |
| contentType | String           | 发送信息至服务器时内容的编码，默认为 `application/x-www-form-urlencoded`                                                                                                                         |

其它不常用的参数可以自行查看 w3c 提供的文档。

下面给出一个 ajax 方法的示例：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <h2>Game</h2>
    <h3>猜数字</h3>
    <span>答案是0-100之间的整数</span>
    <input id="content" />
    <button id="btn">guess</button>
    <script>
      $(document).ready(function () {
        $('#btn').click(function () {
          //下面面是在实验楼环境里运行过的 url，你需要根据注意里的步骤，将后端运行起来后，把web 服务页面中的 url 替换到对应位置，下面一行注释给出了需要替换的位置。
          //"需要将 web 服务中 url 粘贴到这里/ajaxtest"
          $.ajax({
            type: 'POST',
            url: 'https://042bc5dd9bf0.simplelab.cn/ajaxtest',
            data: {
              value: $('#content').val(),
            },
            dataType: 'text',
            beforeSend: function (xhr) {
              var reg = /^((?!0)\d{1,2}|100)$/;
              if (!reg.test(parseInt($('#content').val()))) {
                xhr.abort();
                alert('请输入 0 - 100 的正整数！');
              }
            },
            success: function (result) {
              alert(result);
            },
            error: function (xhr, e) {
              console.log(e);
            },
          });
        });
      });
    </script>
  </body>
</html>
```

这是一个猜数字游戏，在输入框中输入数字，点击按钮，发送 ajax 请求，由后端处理后返回结果，下面给出后端处理部分的逻辑代码：

```js
router.post('/ajaxtest', function (req, res, next) {
  if (req.body.value == 23) res.send('恭喜你，猜对了！');
  else res.send('不是 ' + req.body.value + ' 哦，别灰心，再接再厉！');
});
```

我们在 beforeSend 参数中设置了一个正则判断，需要满足输入的数字为 0-100 的整数才会发送 ajax 请求，否则会调用 abort() 取消这次 ajax 请求。

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077495738)

![图片描述](https://doc.shiyanlou.com/courses/uid920932-20190517-1558077514606)

### 五、jQuery 中的全局 AJAX 事件

想象这样一个应用场景，用户点击了一个按钮，而这个请求需要相当长的时间才能返回，如果页面上没有提示信息，用户很容易就失去耐心，所以我们需要对 ajax 请求有一个全局的监控。jQuery 中 AJAX 的全局事件有：

| 方法                   | 描述                                                                |
| ---------------------- | ------------------------------------------------------------------- |
| ajaxStart(callback)    | Ajax 请求开始时执行的函数                                           |
| ajaxComplete(callback) | Ajax 请求完成时执行的函数                                           |
| ajaxError(callback)    | Ajax 请求发生错误时执行的函数，捕捉到的错误可以作为最后一个参数传递 |
| ajaxSend(callback)     | Ajax 请求发送前执行的函数                                           |
| ajaxSuccess(callback)  | Ajax 请求成功时执行的函数                                           |
| ajaxStop(callback)     | Ajax 请求结束时执行的函数                                           |

例如：

```js
$('$test').ajaxStart(function () {
  $(this).show();
});
$('$test').ajaxStop(function () {
  $(this).hide();
});
```

如果想要某个 ajax 请求不受全局事件监听影响，那么可以在使用 \$.ajax 时，把参数中的 global 设置为 false。

## 六、总结

在本节实验中，我们学习了 ajax 的相关知识以及 jQuery 中 ajax 的常用操作。
