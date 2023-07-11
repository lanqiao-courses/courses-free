---
show: step
version: 1.0
enable_checker: true
---

# jQuery 中的 DOM 操作

## 一、实验介绍

#### 1.1 实验内容

本节实验将带领大家学习 jQuery 中的 DOM 操作。

#### 1.2 实验知识点

- DOM 介绍

- 查找节点

- 创建节点

- 插入节点

- 删除节点

- 复制节点

- 替换节点

- 包裹节点

- 属性操作

- 样式操作

- 设置和获取 HTML、文本和值

- 遍历节点

- CSS-DOM 操作

#### 1.3 实验环境

- `Theia`: 一款前后端分离的、基于 web 的 云 IDE。
- `Preview 或 Mini Browser`：浏览器，右键点击目标 html 文件，选择 open with 下的 Preview 或 Mini Browser 即可在 IDE 中打开浏览器。

#### 1.4 参考链接

- [jQuery 中文网](https://www.jquery123.com/)

## 二、jQuery 中的 DOM 操作

下面我们来介绍一下 jQuery 中的 DOM 操作。

### 2.1 DOM 介绍

文档对象模型（Document Object Model，简称 DOM），是 W3C 组织推荐的处理可扩展标志语言的标准编程接口。DOM 定义了访问 HTML 和 XML 文档的标准。DOM 可以把 HTML 看做是文档树，通过 DOM 提供的 API 可以对树上的节点进行操作。下面我们来看一下 W3C 上的 dom 树：

![](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547429789687.png/wm)

### 2.2 查找节点

使用 jQuery 在文档树上查找节点非常容易，我们可以通过第二个实验所学的 jQuery 选择器来完成。

（1）查找元素节点

```js
var $li = $("ol li:eq(0)"); //获取<ol>里第一个<li>节点
var li_txt = $li.text(); //获取第一个<li>元素节点的文本内容
alert(li_txt); //打印文本内容
```

（2）查找属性节点

利用 jQuery 选择器查找到需要的元素之后，就可以使用 `attr()` 方法来获取它的各种属性的值。attr() 方法的参数可以是一个，也可以是两个。当参数是一个时，则是要查询的属性的名字，比如：

```js
var $para = $("p"); //获取<p>节点
var p_txt = $para.attr("title"); //获取<p>元素节点属性title
alert(p_txt); //打印title属性值
```

注：下面属性操作的部分会具体讲解 attr()方法。

### 2.3 创建节点

（1）创建元素节点

创建元素节点可以用 `$(html)` 函数。\$(html) 方法会根据传入的 HTML 标记字符串，创建一个 DOM 对象，并将这个 DOM 对象包装成一个 jQuery 对象后返回。首先创建一个 li 元素如下所示：

```js
var $li = $("<li></li>"); //创建一个<li>元素
```

当然上面只是创建出来了，要使用的话，还需要使用 `append()` 等方法将该元素插入文档中（下面会讲插入节点）。

（2）创建文本节点

创建文本节点就是在创建元素节点时直接把文本内容写出来，然后使用 `append()` 等方法将它们添加到文档中就可以了，例如：

```js
var $li = $("<li>syl</li>"); //创建一个<li>元素,包括元素节点和文本节点，“syl”就是创建的文本节点
```

（3）创建属性节点

创建属性节点和创建文本节点类似，也是直接在创建元素节点时一起创建，比如：

```js
var $li = $("<li title='syl'>syl</li>"); //创建一个<li>元素,包括元素节点和文本节点和属性节点，“syl”就是创建的文本节点,title='syl' 就是创建的属性节点
```

示例：将新建的 li 元素插入到 ul 中

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <ul>
      <li>blue</li>
      <li>white</li>
      <li>red</li>
    </ul>
    <script type="text/javascript">
      $(document).ready(function () {
        var li_obj = $("<li>黄色</li>");
        $("ul").append(li_obj);
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554966113945.png/wm)

### 2.4 插入节点

(1) DOM 插入现有元素内:

- `.append()` 在每个匹配元素里面的末尾处插入参数内容。

- `.appendTo()` 将匹配的元素插入到目标元素的最后面。

- `.html()` 获取集合中第一个匹配元素的 HTML 内容 设置每一个匹配元素的 html 内容。

- `.prepend()` 将参数内容插入到每个匹配元素的前面（元素内部）。

- `.prependTo()` 将所有元素插入到目标前面（元素内）。

- `.text()` 得到匹配元素集合中每个元素的合并文本，包括他们的后代设置匹配元素集合中每个元素的文本内容为指定的文本内容。

(2) DOM 插入现有元素外:

- `.after()` 在匹配元素集合中的每个元素后面插入参数所指定的内容，作为其兄弟节点。

- `.before()` 根据参数设定，在匹配元素的前面插入内容。

- `.insertAfter()` 在目标元素后面插入集合中每个匹配的元素(注：插入的元素作为目标元素的兄弟元素)。

- `.insertBefore()` 在目标元素前面插入集合中每个匹配的元素(注：插入的元素作为目标元素的兄弟元素)。

注：这些插入节点的方法不仅能将新创建的 DOM 元素插入到文档中，也能对原有的 DOM 元素进行移动。

插入节点示例：将新建的 li 元素插入到 ul 中

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <ul>
      <li>blue</li>
      <li>white</li>
      <li>red</li>
    </ul>
    <script type="text/javascript">
      $(document).ready(function () {
        $("<li>yellow</li>").appendTo("ul");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554966929576.png/wm)

移动节点示例：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <ul>
      <li>blue</li>
      <li>white</li>
      <li>red</li>
      <h1>I like</h1>
    </ul>
    <script type="text/javascript">
      $(document).ready(function () {
        var $li = $("ul li:eq(1)"); //获取<ul>节点中的第2个<li>元素节点
        var $h1 = $("h1"); //获取<h1>节点
        $h1.insertBefore($li); //移动节点
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554967438062.png/wm)

想要查看更多关于插入节点的实例，可以访问 [jQuery 中文官网 DOM 插入现有元素内](https://www.jquery123.com/category/manipulation/dom-insertion-inside/) 和 [jQuery 中文官网 DOM 插入现有元素外](https://www.jquery123.com/category/manipulation/dom-insertion-outside/)

### 2.5 删除节点

如果文档中某一个元素多余，那么我们可以使用 jQuery 中的 `remove()`,`detach()` 和 `empty()` 方法删除节点。

（1）`detach()` 方法

从 DOM 中去掉所有匹配的元素。`.detach()` 方法和 `.remove()` 一样, 除了 `.detach()` 保存所有 jQuery 数据而且和被移走的元素相关联。当需要移走一个元素，不久又将该元素插入 DOM 时，这种方法很有用。

例子：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <ul>
      <li>blue</li>
      <li title="syl">white</li>
      <li>red</li>
    </ul>
    <script type="text/javascript">
      $(document).ready(function () {
        $("ul li").click(function () {
          alert($(this).html());
        });
        var $li = $("ul li:eq(1)").detach(); //删除元素
        $li.appendTo("ul"); //重新追加此元素,发现它之前绑定的事件还在,如果使用remove()方法删除元素的话,那么它之前绑定的事件将失效
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554970362030.png/wm)

（2）`empty()` 方法

从 DOM 中移除集合中匹配元素的所有子节点。这个方法不接受任何参数。这个方法不仅移除子元素（和其他后代元素），同样移除元素里的文本。因为，根据说明，元素里任何文本字符串都被看做是该元素的子节点。

例子：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <ul>
      <li>blue</li>
      <li title="syl">white</li>
      <li>red</li>
    </ul>
    <script type="text/javascript">
      $(document).ready(function () {
        $("ul li:eq(1)").empty(); //获取第二个<li>元素节点后,清除此元素里的内容,注意是元素里
      });
    </script>
  </body>
</html>
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554970764009.png/wm)

注：如果你想删除元素，不破坏他们的数据或事件处理程序（这些绑定的信息还可以在之后被重新添加回来），请使用 `.detach()`。

（3）`remove()` 方法

将匹配元素集合从 DOM 中删除。（注：同时移除元素上的事件及 jQuery 数据。）和 `.empty()` 相似。`.remove()` 将元素移出 DOM。 当我们想将元素自身移除时我们用 `.remove()`，同时也会移除元素内部的一切，包括绑定的事件及与该元素相关的 jQuery 数据。在要删除元素同时保留数据和事件的情况下，使用 `.detach()` 来代替。

例子：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <ul>
      <li>blue</li>
      <li>white</li>
      <li>red</li>
    </ul>
    <script type="text/javascript">
      $(document).ready(function () {
        var $li = $("ul li:eq(1)").remove(); //获取<ul>节点中的第2个<li>元素节点后,将它从网页中删除
        $li.appendTo("ul"); //把刚才删除的节点又重新添加到 <ul> 元素里

        //可以直接使用 appendTo() 方法来简化上面的代码
        //appendTo() 方法也可以用来移动元素,移动元素时首先将文档上删除此元素,然后讲该元素插入得到文档中的指定节点
        //$("ul li:eq(1)").appendTo("ul");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554968995921.png/wm)

从运行效果来看也验证了我们所说的元素用 `remove()` 方法删除后，还是可以继续使用的。

另外 `remove()` 方法也可以通过传递参数来选择性的删除元素。比如：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <ul>
      <li>blue</li>
      <li title="syl">white</li>
      <li>red</li>
    </ul>
    <script type="text/javascript">
      $(document).ready(function () {
        $("ul li").remove("li[title!=syl]"); //将<li>元素中属性title不等于'syl'的<li>元素删除
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554969355134.png/wm)

想要了解更多关于删除节点的例子，可以访问 [jQuery 中文官网 DOM 移除](https://www.jquery123.com/category/manipulation/dom-removal/)。

### 2.6 复制节点

复制节点可以通过 `clone()` 方法来实现， 当 `clone()` 中传递了参数 true 时，代表复制元素的同时复制其所绑定的元素。

示例：点击 li 元素即可复制其本身到 ul 中

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <ul>
      <li>blue</li>
      <li title="syl">white</li>
      <li>red</li>
    </ul>
    <script type="text/javascript">
      $(document).ready(function () {
        $("ul li").on("click", function () {
          $(this).clone().appendTo("ul");
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554971604525.png/wm)

想要了解更多关于复制节点的操作，可以访问 [jQuery 中文官网 复制元素](https://www.jquery123.com/clone/)。

### 2.7 替换节点

- `.replaceAll()` 用集合的匹配元素替换每个目标元素。

- `.replaceWith()` 用提供的内容替换集合中所有匹配的元素并且返回被删除元素的集合。

注：`.replaceAll()` 和 `.replaceWith()` 功能一样，但是目标和源相反。

示例：替换 p 元素

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>shiyanlou</p>
    <script type="text/javascript">
      $(document).ready(function () {
        $("p").replaceWith("<p>SHIYANLOU</p>");
        //注释代码与上面的代码作用一样
        // $("<p>SHIYANLOU</p>").replaceAll("p");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554972562681.png/wm)

想要了解更多关于替换节点的例子，可以访问 [jQuery 中文官网 DOM 替换](https://www.jquery123.com/category/manipulation/dom-replacement/)。

### 2.8 包裹节点

（1） `wrap()` 方法

每个匹配的元素外层包上一个 html 元素。`.wrap()` 函数可以接受任何字符串或对象，可以传递给 \$() 工厂函数来指定一个 DOM 结构。这种结构可以嵌套了好几层深，但应该只包含一个核心的元素。每个匹配的元素都会被这种结构包裹。该方法返回原始的元素集，以便之后使用链式方法

例子：

用一个有边框的 DIV 将 P 元素包裹起来

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>shiyanlou</p>
    <p>shiyanlou</p>
    <script type="text/javascript">
      $(document).ready(function () {
        $("p").wrap("<div style='border:1px red solid;'></div>");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554973461328.png/wm)

（2）`wrapAll()` 方法

在所有匹配元素外面包一层 HTML 结构。`.wrapAll()` 函数可以接受任何字符串或对象，可以传递给 `\$()` 工厂函数来指定一个 DOM 结构。这种结构可以嵌套多层，但是最内层只能有一个元素。所有匹配元素将会被当作是一个整体，在这个整体的外部用指定的 HTML 结构进行包裹。

注：该元素会将所有匹配的元素用一个元素来包裹，它不同于 `wrap()` 方法，`wrap()` 方法是将所有的元素进行单独的包裹。

例子：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>shiyanlou</p>
    <p>shiyanlou</p>
    <script type="text/javascript">
      $(document).ready(function () {
        $("p").wrapAll("<div style='border:1px red solid;'></div>");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554973617931.png/wm)

（3）`wrapInner()` 方法

在匹配元素里的内容外包一层结构。`.wrapInner()` 函数可以接受任何字符串或对象，可以传递给 `$()` 工厂函数来指定一个 DOM 结构。这种结构可以嵌套多层，但是最内层只能有一个元素。每个匹配元素的内容都会被这种结构包裹。`wrapInner()` 方法将每一个匹配的元素的子内容（包括文本节点）用其他结构化的标记包裹起来。

例子：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>shiyanlou</p>
    <p>shiyanlou</p>
    <script type="text/javascript">
      $(document).ready(function () {
        $("p").wrapInner("<div style='border:1px red solid;'></div>");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554973962583.png/wm)

想要了解更多关于包裹节点的实例，可以访问 [jQuery 中文官网 DOM 插入并包裹现有内容](https://www.jquery123.com/category/manipulation/dom-insertion-around/)。

### 2.9 属性操作

在 jQuery 中， `attr()` 方法用来获取和设置元素的属性，`removeAttr()` 方法用来删除元素属性。

（1）获取元素属性

如果要获取元素的属性，那么只需要给 `attr()` 方法传递一个参数，即属性名称。

示例：获取 P 元素的 class 属性值，并追加到 div 中

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p class="shiyanlou-class">shiyanlou</p>
    <div></div>
    <script type="text/javascript">
      $(document).ready(function () {
        var p_class = $("p").attr("class");
        $("div").append(p_class);
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554974979373.png/wm)

（2）设置元素属性

如果需要设置元素的属性值，也可以使用 `attr()` 方法，不同的是，需要传递两个参数即属性名称和对应的值。

示例：设置 div 的 class 值

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      .demo {
        border: 1px solid red;
        height: 100px;
      }
    </style>
  </head>
  <body>
    <div>shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("div").attr("class", "demo");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554975181227.png/wm)

如果需要一次性为同一个元素设置多个元素，可以使用下面的代码来实现：

```js
$("div").attr({ class: "demo", name: "test" }); //将一个 “名/值” 形式的对象设置为匹配元素的属性
```

（3）删除元素属性

用 `removeAttr()` 方法来实现删除元素属性。`.removeAttr()` 方法使用原生的 JavaScript removeAttribute() 函数,但是它的优点是可以直接在一个 jQuery 对象上调用该方法，并且它解决了跨浏览器的属性名不同的问题。

示例：删除 div 的 class

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      .demo {
        border: 1px solid red;
        height: 100px;
      }
    </style>
  </head>
  <body>
    <div class="demo">shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("div").removeAttr("class");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554975571231.png/wm)

想要了解更多关于属性操作的例子，可以访问 [jQuery 中文官网通用属性操作](https://www.jquery123.com/category/manipulation/general-attributes/)。

### 2.10 样式操作

（1）获取样式和设置样式

HTML 代码：

```html
<p class="syl">实验楼</p>
```

其中 class 也是 p 标签的属性，因此获取 class 和 设置 class 都可以使用我们前面所学的 `attr()` 方法。比如：

```js
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script type="text/javascript" src="jquery-3.3.1.js"></script>
        <style type="text/css">
            .syl {
                background-color: red;
            }
            .SYL {
                background-color: yellow;
            }
        </style>
    </head>
    <body>
        <p class="syl">实验楼</p>
        <script type="text/javascript">
            $(document).ready(function() {
                    var  p_class = $("p").attr("class");//获取<p>元素的class
                    console.log(p_class);//打印值为syl
                    $("p").attr("class","SYL");//替换class样式，如果想要添加可以使用addClass()方法
            });
        </script>
    </body>
</html>

```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555036600841.png/wm)

（2）追加样式

`.addClass()` 方法为每个匹配的元素添加指定的样式类名，值得注意的是这个方法不会替换一个样式类名。它只是简单的添加一个样式类名到元素上。

示例：为 div 追加一个新样式 another

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      .demo {
        border: 1px solid red;
        height: 100px;
      }

      .another {
        width: 50%;
      }
    </style>
  </head>
  <body>
    <div class="demo">shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("div").addClass("another");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555037694435.png/wm)

注：上例中 div 元素同时拥有两个 class 值，即 “demo” 和 “another” ，在 css 中有以下两条规定：

- 如果给一个元素添加了多个 class 值，那么就相当于合并了它们的样式。

- 如果有不同的 class 设定了同一样式属性，则后者覆盖前者。

（3）移除样式

`.removeClass()` 方法移除集合中每个匹配元素上一个，多个或全部样式。如果一个样式类名作为一个参数,只有这样式类会被从匹配的元素集合中删除 。如果没有样式名作为参数，那么所有的样式类将被移除。

示例：移除 div 的 another 样式

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      .demo {
        border: 1px solid red;
        height: 100px;
      }

      .another {
        width: 50%;
      }
    </style>
  </head>
  <body>
    <div class="demo another">shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("div").removeClass("another");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555038098024.png/wm)

注：如果要删除多个 class 值，我们可以以空格的方式删除多个 class 名，比如：

```js
$("div").removeClass("another demo"); //删除 another 类和 demo 类
```

如果 `removeClass()` 方法不带参数，就会将 class 的值全部删除，比如：

```js
$("div").removeClass(); //删除<div>元素的所有class
```

（4）切换样式

`.toggleClass()` 在匹配的元素集合中的每个元素上添加或删除一个或多个样式类,取决于这个样式类是否存在或值切换属性。即：如果存在（不存在）就删除（添加）一个类。

例子：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      .main {
        color: red;
      }
    </style>
  </head>
  <body>
    <p>实验楼</p>
    <button class="btn1">切换段落的 "main" 类</button>
    <script type="text/javascript">
      $(document).ready(function () {
        $("button").click(function () {
          $("p").toggleClass("main");
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555039666845.png/wm)

（5）判断是否含有某个样式

`.hasClass()` 可以用来判断元素中是否含有某个 class，如果有则返回 true，否则返回 false。比如：

```js
$("p").hasClass("another");
```

想要查看更多关于样式操作的实例，可以访问 [jQuery 中文官网 class 属性](https://www.jquery123.com/category/manipulation/class-attribute/)。

### 2.11 设置和获取 HTML、文本和值

（1）`.html()` 方法

`.html()` 获取集合中第一个匹配元素的 HTML 内容 或 设置每一个匹配元素的 html 内容。类似于我们原生 JavaScript 中的 `innerHTML` 属性。

示例：获取 div 中的 HTML 内容

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div><p>实验楼</p></div>
    <script type="text/javascript">
      $(document).ready(function () {
        var div_html = $("div").html(); //获取<div>元素的HTML代码
        alert(div_html); //打印<div>元素的HTML代码
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555061984913.png/wm)

示例：设置 div 中的 HTML 内容

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div></div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("div").html("<span>shiyanlou</span>");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062108735.png/wm)

（2）`.text()` 方法

`.text()` 得到匹配元素集合中每个元素的文本内容结合，包括他们的后代，或设置匹配元素集合中每个元素的文本内容为指定的文本内容。类似于 JavaScript 中的 `innerText` 属性。

示例：获取 div 元素的文本内容

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>实验楼</div>
    <script type="text/javascript">
      $(document).ready(function () {
        var p_text = $("div").text();
        alert(p_text);
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062178744.png/wm)

示例：设置 div 中的文本内容

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>实验楼</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("div").text("shiyanlou");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062288869.png/wm)

（3）`.val()` 方法

`.val()` 获取匹配的元素集合中第一个元素的当前值或设置匹配的元素集合中每个元素的值。类似于 JavaScript 中的 `value` 属性。`.val()` 方法主要用于获取表单元素的值，比如 input, select 和 textarea。对于 `<select multiple="multiple">` 元素, `.val()` 方法返回一个包含每个选择项的数组，如果没有选择性被选中，它返回 null。

示例：设置输入框的值

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <input type="text" value="" />
    <script type="text/javascript">
      $(document).ready(function () {
        $("input").val("shiyanlou");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062428371.png/wm)

### 2.12 遍历节点

（1）`.children()` 方法

获得匹配元素集合中每个元素的子元素，选择器选择性筛选。

鉴于一个 jQuery 对象，表示一个 DOM 元素的集合，.children()方法允许我们通过在 DOM 树中对这些元素的直接子元素进行搜索，并且构造一个新的匹配元素的 jQuery 对象。`.find()` 和 `.children()` 方法是相似的，但后者只是针对向下一个级别的 DOM 树。还要注意的是和大多数的 jQuery 方法一样，`.children()` 不返回文本节点;让所有子元素包括使用文字和注释节点，建议使用 `.contents()`。

`.children()` 方法选择性地接受同一类型选择器表达式，我们可以将参数传递给 `$()` 函数。如果提供选择器参数，将过滤出来的元素，测试它们是否匹配。

示例：获取 ul 的子元素 li 的文本值

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <ul>
      <li>blue</li>
      <li>white</li>
      <li>red</li>
    </ul>
    <script type="text/javascript">
      $(document).ready(function () {
        var ul_chlildList = $("ul").children();
        for (var i = 0, len = ul_chlildList.length; i < len; i++) {
          alert(ul_chlildList[i].innerHTML);
        }
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062497856.png/wm)

（2）`.next()` 方法

取得匹配的元素集合中每一个元素紧邻的后面同辈元素的元素集合。如果提供一个选择器，那么只有紧跟着的兄弟元素满足选择器时，才会返回此元素。

如果一个 jQuery 代表了一组 DOM 元素，`.next()` 方法允许我们找遍元素集合中紧跟着这些元素的直接兄弟元素，并根据匹配的元素创建一个新的 jQuery 对象。

该方法还可以接受一个可选的选择器表达式，该选择器表达式可以是任何可传给 `$()` 函数的选择器表达式。如果每个元素的直接兄弟元素满足所提供的选择器，那么它会保存在新生成的 jQuery 对象中，否则，不会包含该元素。

示例：获取 div 后面紧邻的同辈元素

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>shiyanlou</div>
    <p>SHIYANLOU</p>
    <script type="text/javascript">
      $(document).ready(function () {
        var div_next = $("div").next();
        alert(div_next.text());
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062597369.png/wm)

（3）`.prev()`

取得一个包含匹配的元素集合中每一个元素紧邻的前一个同辈元素的元素集合。选择性筛选的选择器。

如果提供的 jQuery 代表了一组 DOM 元素，`.prev()` 方法通过这些元素组合传递到方法构造一个新的 jQuery 对象。

该方法选择性地接受同一类型选择器表达式，我们可以传递给 `$()` 函数。如果提供了选择器表达式，那么会先测试该元素是否满足匹配的选择器表达式。

示例：获取 p 前面紧邻的同辈元素

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>shiyanlou</div>
    <p>SHIYANLOU</p>
    <script type="text/javascript">
      $(document).ready(function () {
        var p_prev = $("p").prev();
        alert(p_prev.text());
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062658893.png/wm)

（4）`.siblings()` 方法

获得匹配元素集合中每个元素的兄弟元素,可以提供一个可选的选择器。

如果提供的 jQuery 代表了一组 DOM 元素，`.siblings()` 方法通过这些元素组合传递到方法构造一个新的 jQuery 对象。

该方法选择性地接受同一类型选择器表达式，我们可以传递给 `$()` 函数。如果提供了选择器表达式，那么会先测试该元素是否满足匹配的选择器表达式。

示例：改变 p 元素前后所有的同辈元素的颜色

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>shiyanlou</div>
    <p>SHIYANLOU-P</p>
    <div>SHIYANLOU</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("p").siblings().css("background-color", "red");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062719015.png/wm)

（5）`.parent()` 方法

取得匹配元素集合中，每个元素的父元素，可以提供一个可选的选择器。

如果提供的 jQuery 代表了一组 DOM 元素，`.parent()` 方法允许我们能够在 DOM 树中搜索到这些元素的父级元素，从有序的向上匹配元素，并根据匹配的元素创建一个新的 jQuery 对象。

`.parents()` 和 `.parent()` 方法是相似的，但后者只是进行了一个单级的 DOM 树查找（注：也就是只查找一层，直接的父元素，而不是更加上级的祖先元素）。此外，`$( "html" ).parent()` 方法返回一个包含 document 的集合，而 `$( "html" ).parents()` 返回一个空集合。

该方法还可以接受一个可选的选择器表达式，该选择器表达式可以是任何可传给 `$()` 函数的选择器表达式。如果提供了选择器表达式，那么会先测试该元素是否满足匹配的选择器表达式。

示例：获取 p 元素的父级元素的 class

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div class="demo">
      <p>SHIYANLOU-P</p>
    </div>
    <script type="text/javascript">
      var p_pa = $("p").parent();
      alert(p_pa.attr("class"));
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062777779.png/wm)

另外还有两个方法 `closest()` 和 `parents()` 方法大家可以了解一下：

- `closest()` 方法从元素本身开始，逐级向上级元素匹配，并返回最先匹配的祖先元素。也就是说首先检查当前元素是否匹配，如果匹配则直接返回元素本身，如果不匹配则向上查找父级元素，逐级向上直到找到匹配选择器的元素，如果什么都没找到则返回一个空的 jQuery 对象。

- `parents()` 获得集合中每个匹配元素的祖先元素。查找方式和 `parent()` 方法类似，不同点在于，当它找到第一个父节点时并没有停止查找，而是继续查找，最后返回多个父节点。

想要了解更多关于遍历节点的操作，可以访问 [jQuery 中文官网遍历](https://www.jquery123.com/category/traversing/)。

### 2.13 CSS-DOM 操作

CSS-DOM 技术简单来说就是读取和设置 style 对象的各种属性。

（1）`.css()`

获取匹配元素集合中的第一个元素的样式属性的值或设置每个匹配元素的一个或多个 CSS 属性。

`.css()` 方法可以非常方便地获取匹配的元素集合中第一个元素的样式属性值， 对于某些属性而言，浏览器访问样式属性的方式是不同的，该方法对于取得这些属性是非常方便的(例如，某些属性在标准浏览器下是通过的 `getComputedStyle()` 方法取得的，而在 Internet Explorer 下是通过 `currentStyle` 和 `runtimeStyle` 属性取得的)并且，某些特定的属性，不同浏览器的写法不一。举个例子， Internet Explorer 的 DOM 将 float 属性写成 styleFloat 实现，W3C 标准浏览器将 float 属性写成 cssFloat。 为了保持一致性，您可以简单地使用"float"，jQuery 将为每个浏览器返回它需要的正确值。

另外,jQuery 同样可以解析 CSS 和 用 multiple-word 格式化（用横杠连接的词，比如：background-color）的 DOM 属性的不同写法。举个例子：jQuery 能解析 `.css('background-color')` 和 `.css('backgroundColor')` 并且返回正确的值。不同的浏览器可能会返回 CSS 颜色值在逻辑上相同，但在文字上表现不同，例如： #FFF, #ffffff, 和 rgb(255,255,255)。

简写速写的 CSS 属性(例如： margin, background, border) 是不支持的，例如，如果你想重新获取 margin，可以使用 `$(elem).css('marginTop')` 和 `$(elem).css('marginRight')`，其他的也是如此。

从 jQuery 1.9 开始, 传递一个 CSS 的样式属性的数组给 `.css()` 将返回 属性 - 值 配对的对象。例如，要获取元素 4 个边距宽度值 `border-width`，你可以使用 `$( elem ).css([ "borderTopWidth", "borderRightWidth", "borderBottomWidth", "borderLeftWidth" ])`.

示例：获取 div 的背景颜色

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      div {
        background-color: red;
      }
    </style>
  </head>
  <body>
    <div>shiyanlou</div>
    <script type="text/javascript">
      alert($("div").css("background-color"));
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062888673.png/wm)

示例：为 div 设置边框和高度属性

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>shiyanlou</div>
    <script type="text/javascript">
      $("div").css({ border: "1px solid red", height: "100px" });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555062949580.png/wm)

对于透明度的设置，可以直接使用 opacity 属性，jQuery 已经处理好了兼容性的问题，比如：

```js
$("p").css("opacity", "0.5");
```

（2）`.height()`、`.width()`

- `.height()` 获取匹配元素集合中的第一个元素的当前计算高度值 或 设置每一个匹配元素的高度值。

- `.width()` 为匹配的元素集合中获取第一个元素的当前计算宽度值 或 给每个匹配的元素设置宽度。

示例：获取 div 的高度和宽度

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>

    <style type="text/css">
      .demo {
        border: 1px solid red;
        height: 100px;
        width: 200px;
      }
    </style>
  </head>
  <body>
    <div class="demo">shiyanlou</div>
    <script type="text/javascript">
      alert($("div").height() + " && " + $("div").width());
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555063004720.png/wm)

`height()` 方法也能用来设置元素的高度，如果传递的是一个数字，则默认单位是 px，如果要用其他单位，则必须传递一个字符串，比如：

```js
$("p").height(520);//设置<p>元素的高度值为520px
$("p").height(10rem);//设置<p>元素的高度值为10rem
```

还可以通过 css 方法来获取高度值：

```js
$(element).css("height");
```

两者的区别是：`css()` 方法获取的高度值与样式的设置有关，可能会得到 “auto” ，也可能得到 “10px” 之类的字符串，而 `height()` 方法获取的高度值则是元素在页面中的实际高度，与样式的设置无关，而且不带单位。

同样的 width 方法也是相类似的，这里就不再重复的讲解了，大家可以自行尝试使用看看效果。

（3）元素定位

- `offset()` 方法，在匹配的元素集合中，获取的第一个元素的当前坐标，或设置每一个元素的坐标，坐标相对于文档。这个方法不接受任何参数。`.offset()` 方法允许我们检索一个元素相对于文档（document）的当前位置。和 `.position()` 的差别在于：`.position()` 是相对于相对于父级元素的位移。当通过全局操作（特别是通过拖拽操作）将一个新的元素放置到另一个已经存在的元素的上面时，若要取得这个新的元素的位置，那么使用 `.offset()` 更合适。`.offset()` 返回一个包含 top 和 left 属性的对象 。比如：

```js
var p_offset = $("p").offset(); //获取<p>元素的offset()
var p_offsetLeft = p_offset.left; //获取左偏移
var p_offsetTop = p_offset.top; //获取右偏移
```

- `position()` 方法，获取匹配元素中第一个元素的当前坐标，相对于 offset parent 的坐标。(offset parent 指离该元素最近的而且被定位过的祖先元素 ) `.position()` 方法可以取得元素相对于父元素的偏移位置。与 `.offset()` 不同, `.offset()` 是获得该元素相对于 documet 的当前坐标 当把一个新元素放在同一个容器里面另一个元素附近时，用 `.position()` 更好用。`.position()`返回一个包含 top 和 left 属性的对象。

```js
var position = $("p").position(); //获取<p>元素的position()
var left = position.left; //获取左偏移
var top = position.top; //获取右偏移
```

- `scrollTop()` 方法和 `scrollLeft()` 方法，这两个方法的作用是分别获取元素的滚动条距顶端的距离和距左侧的距离。另外可以为这两个方法指定一个参数，控制元素的滚动条滚动到指定位置。比如：

```js
var $p = $("p");
var scrollTop = $p.scrollTop(); //获取元素的滚动条距顶端的距离
var scrollLeft = $p.scrollLeft(); //获取元素的滚动条距左侧的距离
$("textarea").scrollTop(300); //元素的垂直滚动条滚动到指定的位置
$("textarea").scrollLeft(300); //元素的横向滚动条滚动到指定的位置
```

想要了解更多关于 css 属性的操作可以访问 [jQuery 中文官网 CSS 属性](https://www.jquery123.com/category/manipulation/style-properties/)。

## 三、总结

本节实验我们学习了 jQuery 中的 DOM 操作，主要知识点如下：

- DOM 介绍

- 查找节点

- 创建节点

- 插入节点

- 删除节点

- 复制节点

- 替换节点

- 包裹节点

- 属性操作

- 样式操作

- 设置和获取 HTML、文本和值

- 遍历节点

- CSS-DOM 操作

在下一节我们将会学习 jQuery 中的事件和动画。想要了解更多 DOM 操作的知识，可以访问 [jQuery 中文官网 DOM 操作](https://www.jquery123.com/category/manipulation/)
