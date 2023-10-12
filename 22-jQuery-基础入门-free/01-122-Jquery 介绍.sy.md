---
show: step
version: 1.0
enable_checker: true
---

# jQuery 介绍

## 一、实验介绍

#### 1.1 实验内容

本节实验将带领大家了解 jQuery 的定义，它有什么作用，我们为什么要学它，以及如何使用它，它的语法是什么，最后对比了 jQuery 对象和 DOM 对象的区别。

> 由于实验楼使用是 WebIDE 的在线环境，所以有不熟悉对同学请阅读：[实验楼 WebIDE 使用指南](https://www.lanqiao.cn/library/shiyanlou-docs/feature/webide) 的前端部分。

#### 1.2 实验知识点

- 什么是 jQuery

- jQuery 特色

- 为什么要学习 jQuery

- 配置 jQuery 环境

- jQuery 语法

- jQuery 代码风格

- jQuery 对象和 DOM 对象

- 解决 jQuery 和其他库的冲突

#### 1.3 实验环境

- `Theia`: 一款前后端分离的、基于 web 的 云 IDE。
- `Preview 或 Mini Browser`：浏览器，右键点击目标 html 文件，选择 open with 下的 Preview 或 Mini Browser 即可在 IDE 中打开浏览器。

#### 1.4 参考链接

- [jQuery 中文官网](https://www.jquery123.com/)

- 《锋利的 jQuery 第二版》

- 知乎文章：[再见 jQuery，我的老朋友](https://zhuanlan.zhihu.com/p/40739079)

## 二、jQuery 简介

#### 2.1 什么是 jQuery

jQuery 是开源软件，使用 MIT 许可证授权。jQuery 的语法设计使得许多操作变得容易，如操作文档对象（document）、选择文档对象模型（DOM）元素、创建动画效果、处理事件、以及开发 Ajax 程序。jQuery 也给开发人员提供了在其上创建插件的能力。这使开发人员可以对底层交互与动画、高级效果和高级主题化的组件进行抽象化。模块化的方式使 jQuery 函数库能够创建功能强大的动态网页以及网络应用程序。

微软和诺基亚已宣布在他们的平台上绑定 jQuery。微软最初在 Visual Studio 中集成了 jQuery 以便在微软自己的 ASP.NET AJAX 框架和 ASP.NET MVC Framework 中使用，而诺基亚则在他的 Web 运行时组件开发平台中集成了 jQuery。MediaWiki 自从 1.16 版本后也开始使用 jQuery。

jQuery 1.3 版以后，引入全新的层叠样式表（CSS）选择器引擎 Sizzle。同时不再提供 Packed 版本，因为解压缩所消耗的时间，远大于所节省的下载时间，且不利于调试，且已有 Google AJAX Libraries API 等公开站台提供 jQuery 的 js 的引用服务，故 Packed 版本原本的优点已荡然无存。

注：定义来自维基百科。

我们可以简单的理解为 jQuery 是一个 JavaScript 函数库。jQuery 是一个轻量级的"写的少，做的多"的 JavaScript 库。

#### 2.2 特色

- 使用多浏览器开源选择器引擎 Sizzle（jQuery 项目的派生产品）进行 DOM 元素选择

- 基于 CSS 选择器的 DOM 操作，使用元素的名称和属性（如 id 和 class）作为选择 DOM 中节点的条件

- 事件

- 特效和动画

- Ajax

- Deferred 和 Promise 对象来控制异步处理

- JSON 解析

- 通过插件扩展

- 工具函数，如特征检测

- 现代浏览器中本地的兼容性方法，但对于旧版浏览器需要后备（fallback）方法，比如 inArray()和 each()

- 多浏览器（不要与跨浏览器混淆）支持

## 三、为什么要学习 jQuery

新闻一：

2018 年 7 月 25 日，Mislav Marohnić 发了一条推文，宣布 GitHub.com 前端已经彻底删除了 jQuery。而且，还自问自答地解释，删除 jQuery 之后也没用其他框架，而是全部依赖原生 API。

新闻二：

Bootstrap 发布了最新版本 4.3.0(https://blog.getbootstrap.com/2019/02/11/bootstrap-4-3-0/)，作为 Bootstrap 4.3 发布的一部分，团队也公布了下一个主要版本 Bootstrap 5 的开发计划。

开发团队表示在发布 v4.3 版本后，将会在开发 Bootstrap 5 的过程中实现一些关键变化，或许会是重大的变化，而这也将被认为是 Bootstrap 5 的基础。开发团队重点提到了以下几方面：

放弃 jQuery：Bootstrap 5 将删除 jQuery 作为依赖项。开发团队已经在这方面工作了很长时间，PR 也是处于正在进行中并已接近完成的状态(https://github.com/twbs/bootstrap/pull/23586)

改进开发分支：v3-dev 分支将成为 master 分支；v4-dev 则保持原样，不过会从该分支切出一个新的 master 分支来开发 v5 版本

从 Jekyll 迁移到 Hugo：目前已有一个 PR 正在进行并且已接近完成(https://github.com/twbs/bootstrap/pull/28014)

移除 jQuery 这个最大的依赖之后，开发团队表示未来将使用原生的纯 JavaScript 来代替 jQuery。这和去年 GitHub 改版重构页面时移除了 jQuery 的举措有点像。

当时 GitHub 的前端团队趁着改版的机会，在重构页面时乘机移除了其中的 jQuery，并且没有使用其它框架来代替 jQuery，而是使用原生 JS：

- 用 querySelectorAll 来查询 DOM 节点

- 使用 fetch 代替 ajax（在不支持的浏览器上使用 XHR）

- 使用代理事件来进行事件处理

- 为一些尚未实现的 DOM 标准写了 polyfill

- 更多地使用自定义元素 (CustomElement)

从这些信息来看，jQuery 正在一步步走向消亡，那么我们还有必要学习 jQuery 吗？首先我们来回顾一下 jQuery 的辉煌。

#### 3.1 jQuery 的辉煌

jQuery 最初诞生于 2006 年 8 月，作者是 John Resig。10 多年前，网页开发者（当时还没有“前端”这个概念）深受浏览器不兼容性之苦。以 jQuery 为代表的一批 JavaScript 库/框架应运而生。

jQuery 官网是这样描述的：

```text
jQuery is a fast, small, and feature-rich JavaScript library. It makes things like HTML document traversal and manipulation, event handling, animation, and Ajax much simpler with an easy-to-use API that works across a multitude of browsers. With a combination of versatility and extensibility, jQuery has changed the way that millions of people write JavaScript.
```

jQuery 强调的理念就是写的少，做的多（write less,do more）。它有如下的优势：

- 轻量级。

- 强大的选择器。

- 出色的 DOM 操作的封装。

- 可靠的事件机制。

- 完善的 ajax。

- 不污染顶级变量。

- 出色的浏览器兼容性。

- 链式操作方式。

- 隐式迭代。

- 行为层与结构层的分离。

- 丰富的插件支持。

- 完善的文档。

- 开源。

2011 年新版的“犀牛书”第 6 版——JavaScript: The Definitive Guide, 6th（JavaScript 权威指南第六版）甚至拿出第 19 章整整 64 页篇幅隆重讲解了 jQuery（“Chapter 19. The jQuery Library”）。jQuery 从此走向鼎盛和辉煌。后来，随着前端交互越来越重和移动应用的普及，jQuery UI、jQuery Mobile 相继面世。时至今日，jQuery 仍然在支撑着数以千万计各种规模网站的运作——尽管聚光灯下已经不常看到她的身影。

最近 10 年，是“前端行业”有史以来发展最快的 10 年。

移动社交时代的到来不仅没有让桌面 Web 失色，反倒刺激了 Web 标准的迅猛改进。HTML5 不仅带来了极大的向后兼容性，也带来了更丰富的原生 DOM API。CSS 从 CSS3 开始走上模块化的快车道，文本样式、排版布局、媒体查询，各种新模块让人目不暇接。

各大主流浏览器也在快速跟进，Firefox、Chrome、Opera、Safari、IE 乃至 Edge，都在积极重构甚至重写内核，争做支持 Web 标准的“楷模”。在这个大背景下，各大互联网公司不断调高兼容的 IE 版本号，从 8 到 9 到 10，再到 11。

当然，还有 ECMAScript 语言标准。自从划时代的 ES6（ECMAScript 2015）发布之后，JavaScript 终于真正开始摆脱“玩具”语言的尴尬境地。更重要的，从 ES6 起，ECMAScript 也进入了快速迭代、每年发一版的节奏。ES7、ES8，以及 ES9，每次都会给这门语言注入更强大的语言特性。

与此同时，Node.js 和 Babel 等服务端运行时及转译工具的出现，也让前端工程化，以及向传统工业级软件开发最佳实践靠拢的速度日益加快。

谷歌主打 SPA（Single Page Application，单页应用）的 Angular 终于一枝独秀。不久，脸书推出的“在 JS 里写 HTML 一样优雅”的 React 则一路高歌猛进。最终，集各家所长且简单易用的 Vue 横空出世。

前端开发已经从后“刀耕火种”时代的“农业文明”，逐渐进化为以大规模、可扩展、规范化、自动化为特征的准“工业文明”。

俗话说：“皮之不存，毛将焉附。”随着时代变迁、技术进步，jQuery 赖以存在的环境正逐渐消失。如前所述，新的环境催生了一批框架新秀。曾经辉煌的 jQuery 终于走到了可以华丽谢幕的时刻。

注：以上文章部分摘取自知乎文章：[再见 jQuery，我的老朋友](https://zhuanlan.zhihu.com/p/40739079)。另外推荐一篇文章 [jQuery 都过时了，那我还学它干嘛？](https://zhuanlan.zhihu.com/p/46753973)

对于小白来说，还是可以简单的学一下 jQuery 的，因为 jQuery 真的很简单，甚至都不需要你会多少 JavaScript，你就可以学会，我们可以简单的学一下 jQuery 的思想，有利于我们去学习现在主流的三大框架：vue，react，angular。而且虽然 jQuery 已经渐渐的退出了历史舞台，新写的项目都很少会再使用 jQuery 了，但是其实很多大公司的一些老项目还是有用到 jQuery 的，不要以为大厂的技术栈就一定会很新很新，其实大厂会有很多老项目需要维护的，这个时候就算你懂前端所有的新技术，但是在大厂你负责的项目可能需要你对 jQuery 有深入的理解，这样你才能维护好这个项目。而创业公司为了快速适应市场，追求的是产品开发效率，vue,react 等则极大的提高了开发效率。在竞争日益激烈的互联网环境下，多学点东西总是没坏处的，而且它真的很简单，我们花了几天来学习一下又有何不可？简历上也能多写一项会的技能。

- jQuery：我们考虑如何操作 DOM，jQuery 考虑如何让我们更方便地操作 DOM。

- vue 和 React：我们考虑如何操作数据，框架考虑如何将改变后的数据更新到界面。

这里贴一张网络上的搞笑图：

![图片描述](https://doc.shiyanlou.com/courses/uid897174-20190423-1556006091428)

## 四、配置 jQuery 环境

进入 jQuery 的官方网站 `http://jquery.com/`,可以下载最新的 jQuery 文件到本地，然后再引入到项目即可。官方网站如下所示：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554787571171.png/wm)

引入代码示例：

```html
<head>
  <script type="text/javascript" src="jquery.js"></script>
</head>
```

注意：这里的路径位置，请大家根据实际的情况自行调整。另外官方网站有两个版本的 jQuery 可供下载：一份是精简过的，另一份是未压缩的（供调试或阅读），请大家根据自己的需要自行选择下载。实验楼课程中可以通过命令下载：

```js
wget https://labfile.oss.aliyuncs.com/courses/22/jquery-3.3.1.js
```

当然如果我们不想把 jQuery 下载到本地，也可以使用 Google 的 CDN 或者使用 Microsoft 的 CDN：

使用 Google 的 CDN：

**Google CDN 国内已经无法访问，以下内容仅作演示。**

```html
<head>
  <script
    type="text/javascript"
    src="http://ajax.googleapis.com/ajax/libs
/jquery/1.4.0/jquery.min.js"
  ></script>
</head>
```

使用 Microsoft 的 CDN：

```html
<head>
  <script
    type="text/javascript"
    src="http://ajax.microsoft.com/ajax/jquery
/jquery-1.4.min.js"
  ></script>
</head>
```

注：实验楼的环境中不能连外网，所以大家本地可以使用 CDN 的方式，实验楼环境下，通过前面的命令下载下来然后引入的方式来使用。

## 五、jQuery 语法

jQuery 语法是为 HTML 元素的选取编制的，可以对元素执行某些操作。

基础语法是：

```js
$(selector).action();
```

- 美元符号 \$ 定义 jQuery。

- 选择符（selector）“查询”和“查找” HTML 元素。

- jQuery 的 action() 执行对元素的操作。

另外需要注意的是：在 jQuery 库中 \$ 符号就是 jQuery 的一个简写形式，例如 `$("#syl")` 和 `jQuery("#syl")` 是等价的，`$.ajax` 和 `jQuery.ajax` 是等价的，如果没有特别说明，程序中的 `$` 符号都是 jQuery 的一个简写形式。

#### 5.1 文档就绪函数

所有 jQuery 函数位于一个 document ready 函数中：

```js
$(document).ready(function(){

});

// 可以简写成

$(funciton(){

});
```

这是为了防止文档在完全加载（就绪）之前运行 jQuery 代码。如果在文档没有完全加载之前就运行函数，操作可能失败。下面是两个具体的例子：

- 试图隐藏一个不存在的元素。

- 获得未完全加载的图像的大小。

上面的这段代码其实有点类似于传统 JavaScript 中的 window.onload 方法，不过它们还是有一些区别的，简单对比如下所示：

|          | window.onload                                          | \$(doucment).ready()                                                     |
| -------- | ------------------------------------------------------ | ------------------------------------------------------------------------ |
| 执行时机 | 必须等待网页中所有的内容加载完毕后才能执行（包括图片） | 网页中所有 DOM 结构绘制完毕后就执行，可能 DOM 元素关联的东西并没有加载完 |
| 编写个数 | 不能同时编写多个。                                     | 能同时编写多个。                                                         |

编写个数的意思就是：

```js
window.onload = function () {
  alert("test1");
};
window.onload = function () {
  alert("test2");
};
//结果只会输出 test2。
```

```js
$(document).ready(function () {
  alert("test1");
});
$(document).ready(function () {
  alert("test2");
});
//结果两次都输出
```

#### 5.2 编写我们的第一个 jQuery 程序

例子：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <!-- Google 的 CDN 的方式加载jQuery，请大家自行修改为本地 -->
    <script
      type="text/javascript"
      src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.0/jquery.min.js"
    ></script>
  </head>
  <body>
    <script type="text/javascript">
      //等待dom元素加载完毕
      $(document).ready(function () {
        //弹出一个框:显示hello syl
        alert("hello syl");
      });
    </script>
  </body>
</html>
```

> 请大家将加载 jQuery 的 Google 的 CDN 改为我们之前讲到的引入本地的 jQuery 文件，请大家将它下载下来然后做一个替换，因为在环境内无法访问到这些 CDN，之后的实验中同理，不再赘述。

> 修改内容为，将`<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.0/jquery.min.js"></script>`修改为`<script type="text/javascript" src="jquery.min.js"></script>`。这里需要使用前面的命令，将 jquery.min.js 下载到与代码文件同一路径目录下。

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554793438595.png/wm)

## 六、jQuery 代码风格

良好的代码风格使得代码更加具有可读性，适当的注释代码，对于日后代码的维护也是非常有利的。来看个例子：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery.min.js"></script>
  </head>
  <body>
    <div class="box">
      <ul class="menu">
        <li class="level1">
          <a href="#">春天</a>
          <ul class="level2">
            <li>春意盎然</li>
            <li>春意盎然</li>
            <li>春意盎然</li>
            <li>春意盎然</li>
          </ul>
        </li>
        <li class="level1">
          <a href="#">夏天</a>
          <ul class="level2">
            <li>夏日炎炎</li>
            <li>夏日炎炎</li>
            <li>夏日炎炎</li>
            <li>夏日炎炎</li>
          </ul>
        </li>
        <li class="level1">
          <a href="#">秋天</a>
          <ul class="level2">
            <li>秋高气爽</li>
            <li>秋高气爽</li>
            <li>秋高气爽</li>
            <li>秋高气爽</li>
          </ul>
        </li>
      </ul>
    </div>
    <script type="text/javascript">
      //等待dom元素加载完毕
      $(document).ready(function () {
        $(".level1>a").click(function () {
          $(this)
            .addClass("current")
            .next()
            .show()
            .parent()
            .siblings()
            .children("a")
            .removeClass("current")
            .next()
            .hide();
          return false;
        });
      });
    </script>
  </body>
</html>
```

代码很简单，我们没有加入 css 样式这些，主要还是讲解 jQuery，运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554796558716.png/wm)

简单解释一下这段代码，当鼠标点击到 a 标签的时候给其添加一个名为 current 的 class，然后调用 next() 和 show() 将其后面的元素显示出来，然后调用 parent()、siblings()、children("a") 将它的父辈的同辈元素的内部的子元素 a 都去掉一个名为 current 的 class (`removeClass("current")`)，并且将紧邻它们后面的元素都隐藏。

这就是 jQUery 的强大的链式操作，一行代码就完成了我们导航栏的功能，大家可以试着去写一下原生的 JavaScript 代码，看看需要写多少行，这也就是我们 jQuery 的魅力所在。当然上面的那些方法看不懂也没关系，后面都会讲解的。不过为了进一步改善代码的可读性和可维护性，推荐一种写法：

```js
$(document).ready(function () {
  $(".level1>a").click(function () {
    $(this)
      .addClass("current") //给当前元素添加"current"样式
      .next()
      .show() //下一个元素显示
      .parent()
      .siblings()
      .children("a")
      .removeClass("current") //父元素的同辈元素的子元素a移除"current"样式
      .next()
      .hide(); //它们的下一个元素隐藏
    return false;
  });
});
```

也就是说适当的换行和添加注释可以让我们对代码作用一目了然，增加代码的可读性，便于日后的维护，提高开发效率。

## 七、jQuery 对象和 DOM 对象

#### 7.1 DOM 对象

DOM （Document Object Model）对象，也就是我们经常说的文档对象模型，每一份 DOM 都可以表示成一棵 DOM 树：

![](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547429789687.png/wm)

比如这样的一段代码：

```html
<h1></h1>
<p></p>
<ul>
  <li></li>
</ul>
```

h1,p,ul 以及 li 标签都是 DOM 元素节点，我们可以通过 JavaScript 中的 `document.getElementById()`，`document.getElementsByTagName()` 等来获取元素节点，像这样获取的 DOM 元素就是 DOM 对象，DOM 对象可以使用 JavaScript 中的方法，比如：

```js
var domObj = document.getElementById("id"); //获取DOM对象
var objHtml = domObj.innerHTML; //使用JavaScript中的属性innerHTML
```

#### 7.2 jQuery 对象

jQuery 对象就是通过 jQuery 包装 DOM 对象后产生的对象。jQuery 对象是 jQuery 独有的，如果一个对象是 jQuery 对象，那么它就可以使用 jQuery 里的方法，比如：

```js
$("#syl").html(); //获取id为syl的元素内的html代码，html()是jQuery中的方法
```

这段代码等同于：

```js
document.getElementById("syl").innerHTML;
```

在 jQuery 对象中无法使用 DOM 对象中的任何方法，例如 `$("#syl").innerHTML;` 之类的写法是错的，可以使用 `$("#syl").html();` 之类的 jQuery 方法来代替，同样的道理，DOM 对象也不能使用 jQuery 里的方法，例如:`document.getElementById("syl").html();`也是会报错的。

注：用 `#id` 作为选择符取得的是 jQuery 对象而并非 `document.getElementById("id");` 所得到的 DOM 对象，两者并不等价。我们一定要从开始就树立这样的一个观念：jQuery 对象和 DOM 对象是有区别的，它们并不是等价的。

#### jQuery 对象和 DOM 对象之间的相互转换

在讲解 `jQuery` 对象和 `DOM` 对象之间的相互转换之前，我们先约定好定义变量的风格，如果获取的是 `jQuery` 对象，那么我们在变量前面加上 `$` 符号，例如：

```js
var $test = jQuery 对象;
```

如果获取的是 DOM 对象：

```js
var test = DOM 对象;
```

注：这里加个 $ 只是为了区分变量是 jQuery 对象还是 DOM 对象并不是说所有使用 jQuery 的代码中变量声明都需要 $。

1. jQuery 对象转换为 DOM 对象

我们前面说过 jQuery 对象不能使用 DOM 中的方法，但是如果我们又不得不使用 DOM 中的方法呢？比如：对 jQuery 对象所提供的方法不熟悉或者忘了但是知道 DOM 中的方法，自己又很懒不想去查 jQuery 手册或者 jQuery 本身就没有封装我们想要使用的方法。有以下的两种处理方法：

- [index]:jQuery 对象是一个类似数组的对象，可以通过 `[index]` 的方法得到对应的 DOM 对象，比如：

```js
var $cr = $("#cr"); //jQuery 对象
var cr = $cr[0]; //DOM 对象
```

- 通过 get(index) 方法得到相应的 DOM 对象，比如：

```js
var $cr = $("#cr"); //jQuery 对象
var cr = $cr.get(0); //DOM 对象
```

2. DOM 对象转换为 jQuery 对象

对于一个 DOM 对象，只需要用 `$()` 把 DOM 对象包装起来，就可以获得一个 jQuery 对象了，比如：

```js
var cr = document.getElementById("cr"); //DOM 对象
var $cr = $(cr); //jQuery 对象
```

注：这里再次强调一次，DOM 对象才能使用 DOM 中的方法，jQuery 对象不可以使用 DOM 中的方法，但 jQuery 对象提供了一套更加完善的工具用于操作 DOM，在后面的学习中，我们都会为大家一一的进行讲解。我们平时用到的 jQuery 对象都是通过 \$() 函数制造出来的，\$() 函数就是一个 jQuery 对象的制造工厂。

下面我们来看个例子：

DOM 方式判断复选框是否被选中：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <input type="checkbox" id="cr" /><label for="cr">我已阅读协议</label>
    <script type="text/javascript">
      //等待dom元素加载完毕
      $(document).ready(function () {
        var $cr = $("#cr"); //jQuery对象
        var cr = $cr[0]; //DOM对象，或者$cr.get(0)
        $cr.click(function () {
          if (cr.checked) {
            //DOM方式判断
            alert("你已同意本协议");
          }
        });
      });
    </script>
  </body>
</html>
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554863100406.png/wm)

jQuery 方式判断复选框是否被选中：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <input type="checkbox" id="cr" /><label for="cr">我已阅读协议</label>
    <script type="text/javascript">
      //等待dom元素加载完毕
      $(document).ready(function () {
        var $cr = $("#cr");
        $cr.click(function () {
          if ($cr.is(":checked")) {
            alert("你已同意本协议");
          }
        });
      });
    </script>
  </body>
</html>
```

注：上面的例子简单的演示了 DOM 对象和 jQuery 对象的不同，但是最终的运行效果是一样的。

## 八、解决 jQuery 和其他库的冲突

jQuery 使用 \$ 符号作为 jQuery 的简写。如果其他 JavaScript 框架也使用 \$ 符号作为简写怎么办？

其他一些 JavaScript 框架包括：MooTools、Backbone、Sammy、Cappuccino、Knockout、JavaScript MVC、Google Web Toolkit、Google Closure、Ember、Batman 以及 Ext JS。

其中某些框架也使用 \$ 符号作为简写（就像 jQuery），如果您在用的两种不同的框架正在使用相同的简写符号，有可能导致脚本停止运行。

jQuery 的团队考虑到了这个问题，并实现了 noConflict() 方法。

noConflict() 方法会释放对 \$ 标识符的控制，这样其他脚本就可以使用它了。当然，我们仍然可以通过全名替代简写的方式来使用 jQuery：

```js
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script type="text/javascript" src="jquery-3.3.1.js"></script>
    </head>
    <body>
        <p>这是一个段落。</p>
        <button>点我</button>
        <script type="text/javascript">
            //等待dom元素加载完毕
            $.noConflict();
            jQuery(document).ready(function() {
                jQuery("button").click(function() {
                    jQuery("p").text("jQuery 仍然在工作!");
                });
            });
        </script>
    </body>
</html>

```

我们也可以创建自己的简写。noConflict() 可返回对 jQuery 的引用，我们可以把它存入变量，以供稍后使用：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>这是一个段落。</p>
    <button>点我</button>
    <script type="text/javascript">
      //等待dom元素加载完毕
      var jq = $.noConflict();
      jq(document).ready(function () {
        jq("button").click(function () {
          jq("p").text("jQuery 仍然在工作!");
        });
      });
    </script>
  </body>
</html>
```

如果你的 jQuery 代码块使用 \$ 简写，并且您不愿意改变这个快捷方式，那么您可以把 \$ 符号作为变量传递给 ready 方法。这样就可以在函数内使用 \$ 符号了，而在函数外，依旧不得不使用 "jQuery"：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>这是一个段落。</p>
    <button>点我</button>
    <script type="text/javascript">
      //等待dom元素加载完毕
      $.noConflict();
      jQuery(document).ready(function ($) {
        $("button").click(function () {
          $("p").text("jQuery 仍然在工作!");
        });
      });
    </script>
  </body>
</html>
```

运行效果都为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554867060642.png/wm)

## 九、总结

在本节实验中我们主要学习了以下内容：

- 什么是 jQuery

- jQuery 特色

- 为什么要学习 jQuery

- 配置 jQuery 环境

- jQuery 语法

- jQuery 代码风格

- jQuery 对象和 DOM 对象

- 解决 jQuery 和其他库的冲突

通过本节的学习我们初步熟悉了 jQuery 的定义以及语法，并且完成了第一个 jQuery 例子的编写，感受到了 jQuery 的魅力所在，下一节我们将学习 jQuery 选择器的内容。
