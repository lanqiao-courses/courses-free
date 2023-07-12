---
show: step
version: 1.0
enable_checker: false
---

# HTML5 Web Storage 和文件上传

## 简介

本节中，我们将学习 HTML5 Web Storage 和 HTML5 的文件上传。Web Storage 有两种存储数据的新方法，HTML5 Web Storage 的使用可以提高网页的效率，在不影响网站性能的情况下可以存储大量数据。同时，HTML5 中提供了文件操作的 API，使我们不必再借助第三方插件进行文件模块的开发。

#### 知识点

- Web Storage 概述
- localStorage 方法
- sessionStorage 方法
- HTML5 文件上传概述
- 如何实现文件上传
- 文件读取

## Web Storage 概述

早些时候，本地存储使用的是 cookie。但是 Web 存储需要更加的安全与快速。这些数据不会被保存在服务器上，但是这些数据只用于用户请求网站数据上。它也可以存储大量的数据，而不影响网站的性能。

HTML5 定义了本地存储规范 Web Storage，提供了两种存储类型 API：localStorage 和 sessionStorage。

先说说 cookie：

1. 大小的限制：cookie 的大小被限制在 4KB。
2. 带宽的限制：只要涉及 cookie 的请求，cookie 数据都会在服务器和浏览器间来回传送。这样无论访问哪个页面，cookie 数据都会消耗网络的带宽。
3. cookie 会频繁的在网络中传送，而且数据在网络中是可见的，因此在不加密的情况下 ，是有安全风险的。

Web Storage 数据存储机制相比于 Cookie 有明显的优势：

- 存储空间的大小一般为 5~10MB，与具体浏览器有关。

- 存储内容仅仅存储在本地客户端，不会被发送到服务器。

- 提供了更丰富、更易用的接口、操作更方便。

## localStorage 方法

特点：持久化的本地存储，除非主动手动删除，否则数据一直不会过期。

#### 方法：

**1.设置**

```js
setItem(key,value)：在本地客户端存储一个字符串类型的数据。
setItem.key=value：也可以像这样直接存储。
```

保存数据：

```js
localStorage.setItem(key, value);
```

保存添加数据例子：

```js
//方法1向本地存储中添加一个名为name,值为"syl"的key-value对象
localStorage.setItem("name", "syl");
//方法2
localStorage["price"] = 1314;
//方法3
localStorage.amount = 520;
```

注：使用 `setItem` 方法保存数据时，将第一个参数 `key` 指定为键名，将第二个参数 `value` 指定为键值，保存时不允许保存相同的键名，保存后可以修改键值，但不允许改键名（只能重新取键名，然后再保存键值）。

**2.获取**

```js
getItem(key)：读取已存储在本地的数据，获取键值。
localStorage.key：也可以像这样直接获取值。
```

读取数据：

```js
localStorage.getItem(key);
```

注：使用 `getItem` 方法读取数据时，将参数指定为键名，返回键值。

**3.删除**

```js
removeItem(key)：移除已存储在本地数据，通过键名作为参数删除数据。
localStorage.clear()：也可以一次性清除
```

删除单个数据：

```js
localStorage.removeItem(key);
```

注：通过 `key` 删除本地数据。

例子：

```html
<body>
  <h1>简单Web留言本</h1>
  <textarea id="memo" cols="60" rows="6"></textarea><br />
  <input type="button" value="新增留言" onclick="saveStorage('memo');" />
  <input type="button" value="清空数据" onclick="clearStorage();" />
  <input
    type="button"
    value="清空最后一个数据"
    onclick="clearsingleStorage();"
  />
  <hr />
  <p id="msg"></p>
  <script type="text/javascript">
    //savaStorage是一个新增留言的函数
    function saveStorage(id) {
      //获取textarea的value值
      var data = document.getElementById(id).value;
      //获取当前时间
      var time = new Date().toUTCString();
      //将当前时间作为键名，textarea的value值（也就是用户输入的值）的值作为键值
      localStorage.setItem(time, data);
      //显示留言
      showMsg("msg");
    }
    //showMsg是一个显示留言的函数
    function showMsg(id) {
      var result = '<table border="1">';
      //遍历本地储存数据
      for (var i = 0; i < localStorage.length; i++) {
        //获取key值
        var key = localStorage.key(i);

        //获取value值
        var value = localStorage.getItem(key);
        //显示数据
        result += "<tr><td>" + value + "</td><td>" + key + "</td></tr>";
      }
      result += "</table>";
      var target = document.getElementById(id);
      target.innerHTML = result;
    }
    //显示留言
    showMsg("msg");
    //clearStorage是一个清空留言的函数
    function clearStorage() {
      //清空数据
      localStorage.clear();
      //显示留言
      showMsg("msg");
    }
    //clearsingleStorage是一个删除单个数据的函数
    function clearsingleStorage() {
      localStorage.removeItem(localStorage.key(localStorage.length - 1));
      //显示留言
      showMsg("msg");
    }
  </script>
</body>
```

运行效果如下所示：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1550456823130.png/wm)

## sessionStorage 方法

直接关闭页面，再重新运行查看一下效果。

`sessionStorage` 方法将数据保存在 `session` 对象中，所谓 `session` 直译过来就是会话，再通俗一点讲就是指用户在浏览某个网站时，从进入网站到关闭浏览器的这段时间，`session` 对象可以用来保存在这段时间内所要求保存的任何数据。我们称之为会话级别的本地存储。

注：当浏览器窗口被关闭时，`session` 对象保存的数据会被删除。

`sessionStorage` 方法的使用与 `localStorage` 方法的使用类似，这里不在累赘：

例子：

```html
<body>
  <script type="text/javascript">
    if (sessionStorage.pagecount) {
      sessionStorage.pagecount = Number(sessionStorage.pagecount) + 1;
    } else {
      sessionStorage.pagecount = 1;
    }
    document.write("你刷新了本页面 " + sessionStorage.pagecount + " 次");
  </script>

  <p>刷新页面看看效果。</p>

  <p>关闭浏览器再运行看看效果</p>
</body>
```

注：上面的案例 `sessionStorage` 改成 `localStorage` 也就可以简单的记录自己的网站的浏览人数了，当然这只是我们自己写简单网站，自己看自己浏览次数的使用方法。请大家自行尝试一下 `sessionStorage` 保存数据，读取数据，删除单个数据，清空数据的写法。

## HTML5 文件上传概述

在之前我们操作本地文件都是使用 flash、silverlight 或者第三方的 activeX 插件等技术，由于使用了这些技术后就很难进行跨平台、或者跨浏览器、跨设备等情况下实现统一的表现，从另外一个角度来说就是让我们的 web 应用依赖了第三方的插件，而不是很独立。 在 HTML5 标准中，默认提供了操作文件的 API 让这一切直接标准化。有了操作文件的 API，让我们的 Web 应用可以很轻松的通过 JS 来控制文件的读取、写入、文件夹、文件等一系列的操作。

## 如何实现文件上传

在 HTML4 标准中文件上传控件只接受一个文件，而在新标准中，只需要设置 `multiple`，就支持多文件上传。按住 `Ctrl` 或者 `Shift` 即可选择多个文件。

```html
<input type="file" class="file" multiple />
```

#### 获取文件信息

选中文件通过 `HTMLInputElement.files` 属性返回，返回值是一个 `FileList` 对象，这个对象是一个包含了许多 File 文件的列表。比如我们首先运行上面例子的代码，然后随便上传一个文件（这里上传的是一个名为 700.png 的图片），然后按 F12 进入控制台，输入代码:

```js
file.files;
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1550641526726.png/wm)

从运行效果我们可以看出每个 File 对象包含了以下信息：

- lastModified 表示 UNIX timestamp 形式的最后修改时间。

- lastModifiedDate 表示 Date 形式的最后修改时间。

- name 表示文件名。只读字符串，只包含文件名称，不包含任何路径。

- size 表示文件的字节大小，只读的 64 位整数。

- type 表示文件的 MIME 类型。当类型不确定时为 ""。

- webkitRelativePath 此处为空，当在 `input` 上加 `webkitdirectory` 属性时，用户可选择文件夹，此时 `webkitRelativePath` 表示文件夹中文件的相对路径。

注：FileList 对象由 DOM 提供，列出了所有用户选择的文件，每一个代表了一个 File 对象。你可以通过检查文件列表的 `length` 属性决定用户可以选则多少文件。各个 File 对象可以方便地通过访问文件列表来获取，像访问数组那样。

#### 限制文件的上传类型

如果我们需要限制用户上传文件的类型，比如某处我们只希望能够上传图片，那么我们可以使用 `input` 的 `accept` 属性，`accept` 属性接受一个逗号分隔的 `MIME` 类型字符串。比如：

```html
<!-- 表示只接受 png 图片 -->
accept="image/png" 或 accept=".png"

<!-- 表示接受PNG/JPEG 文件. -->
accept="image/png, image/jpeg" 或 accept=".png, .jpg, .jpeg"

<!-- 表示接受任何图片文件类型. -->
accept="image/*"

<!-- 表示接受任何 MS Doc 文件类型. -->
accept=".doc,.docx,.xml,application/msword,application/vnd.openxmlformats-officedocument.wordprocessingml.document"
```

例子：

```html
<form>
  <div>
    <label for="profile_pic">上传图片</label>
    <input
      type="file"
      id="profile_pic"
      name="profile_pic"
      accept=".jpg, .jpeg, .png"
    />
  </div>
  <div>
    <button>Submit</button>
  </div>
</form>
```

## 文件读取

以上 File 对象只获取到了对文件的描述信息，但没有获得文件中的数据。HTML5 中提供了 `FileReader` 对象允许 Web 应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容。

首先创建一个 `FileReader` 实例：

```js
var reader = new FileReader();
```

#### 属性

`FileReader.error` 属性表示在读取文件时发生的错误，只读。语法为：

```js
var error = instanceOfFileReader.error;
```

`FileReader.readyState` 属性表示 `FileReader` 状态的数字，只读。取值如下：

| 常量名    | 值  | 描述                  |
| --------- | --- | --------------------- |
| `EMPTY`   | `0` | 还没有加载任何数据.   |
| `LOADING` | `1` | 数据正在被加载.       |
| `DONE`    | `2` | 已完成全部的读取请求. |

语法为：

```js
var state = instanceOfFileReader.readyState;
```

`FileReader.result` 属性表示文件的内容。该属性仅在读取操作完成后才有效，数据的格式取决于使用哪个方法来启动读取操作，只读。

#### 事件处理

| FileReader.onabort     | 处理`abort`事件。该事件在读取操作被中断时触发。                       |
| ---------------------- | --------------------------------------------------------------------- |
| FileReader.onerror     | 处理`error`事件。该事件在读取操作发生错误时触发。                     |
| FileReader.onload      | 处理`load`事件。该事件在读取操作完成时触发。                          |
| FileReader.onloadstart | 处理`loadstart`事件。该事件在读取操作开始时触发                       |
| FileReader.onloadend   | 处理`loadend`事件。该事件在读取操作结束时（要么成功，要么失败）触发。 |
| FileReader.onprogress  | 处理`progress` 事件。该事件在读取 Blob 时触发                         |

#### 方法

| FileReader.abort()             | 中止读取操作。在返回时，`readyState`属性为`DONE`。                                                             |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| FileReader.readAsArrayBuffer() | 开始读取指定的  Blob 中的内容, 一旦完成, result 属性中保存的将是被读取文件的  ArrayBuffer  数据对象 。         |
| FileReader.readAsDataURL()     | 开始读取指定的 Blob 中的内容。一旦完成，result 属性中将包含一个 data: URL 格式的字符串以表示所读取文件的内容。 |
| FileReader.readAsText()        | 开始读取指定的 Blob 中的内容。一旦完成，result 属性中将包含一个字符串以表示所读取的文件内容。                  |

我们常用的是上传一个图片并显示出来和上传文本显示文本，因为这里只对 FileReader.readAsDataURL() 和 FileReader.readAsText() 方法进行举例说明，想要了解更多的知识可以访问 [MDN FileReader](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)

请上传 .txt 、.jpg 、.png 文件。

例子：

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>文件读取</title>
    <script type="text/javascript">
      //检查浏览器是否支持FileReader
      if (typeof FileReader == "undified") {
        alert("您老的浏览器不行了！");
      }

      function showDataByURL() {
        var resultFile = document.getElementById("fileDemo").files[0];
        if (resultFile) {
          //创建一个 FileReader 实例
          var reader = new FileReader();
          //读取文件数据
          reader.readAsDataURL(resultFile);
          /*读取的过程就相当于 加载过程 */
          /*读取完毕  预览 */
          reader.onload = function (e) {
            var urlData = this.result;
            document.getElementById("result").innerHTML +=
              "<img src='" + urlData + "' alt='" + resultFile.name + "' />";
          };
        }
      }

      function showDataByText() {
        var resultFile = document.getElementById("fileDemo").files[0];
        if (resultFile) {
          var reader = new FileReader();
          reader.readAsText(resultFile, "gb2312");
          reader.onload = function (e) {
            var urlData = this.result;
            document.getElementById("result").innerHTML += urlData;
          };
        }
      }
    </script>
  </head>

  <body>
    <input type="file" name="fileDemo" id="fileDemo" multiple="multiple" />
    <input
      type="button"
      value="显示图片"
      id="readAsDataURL"
      onclick="showDataByURL();"
    />
    <input
      type="button"
      value="显示文本"
      id="readAsText"
      onclick="showDataByText();"
    />
    <div id="result"></div>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1550646952468.png/wm)

## 总结

本节中我们学习了 HTML5 的 Web Storage 和文件操作。

- Web Storage 概述
- localStorage 方法
- sessionStorage 方法
- HTML5 文件上传概述
- 如何实现文件上传
- 文件读取

HTML5 的存储特性解决了 Cookie 遗留下来的一些限制，可以带给用户更好的使用体验。HTML5 文件操作 API 的出现，使我们网页可以做到跨平台、跨浏览器应用，不再受限于第三方软件的限制。

到这里，HTML5 基础课程的学习就结束了。我们一起领略操作了 HTML5 众多新特性，新元素，新属性，感受了新标准、新规则给网页带来的革新。课程中出现的案例操作希望大家能够动手编码调试，加深对这些特性的理解，记忆。
