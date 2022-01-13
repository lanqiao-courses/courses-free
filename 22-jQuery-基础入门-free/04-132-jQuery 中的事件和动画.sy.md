---
show: step
version: 1.0
enable_checker: true
---

# jQuery 中的事件和动画

## 一、实验介绍

#### 1.1 实验内容

JavaScript 使我们有能力创建动态页面。事件是可以被 JavaScript 侦测到的行为。 网页中的每个元素都可以产生某些可以触发 JavaScript 函数的事件。比方说，我们可以在用户点击某按钮时产生一个 onClick 事件来触发某个函数。事件在 HTML 页面中定义。

注：定义来自搜狗百科。

除了使用传统的 JavaScript 事件来完成我们的交互，也可以使用 jQuery，jQuery 不仅提供了更加优雅的事件处理语法，而且极大的增强了事件处理能力。下面我们将分别学习 jQuery 中的事件和动画。

本节实验将带领大家学习 jQuery 中的事件和动画的操作。

#### 1.2 实验知识点

- 加载 DOM

- 事件绑定

- 合成事件

- 事件冒泡

- 事件对象的属性

- 移除事件

- 模拟操作

- show()方法和 hide()方法

- fadeIn() 方法和 fadeOut() 方法

- slideUp() 方法和 slideDown() 方法

- animate()方法

- 动画回调函数

- 停止动画和判断是否处于动画状态

- 其他动画方法

#### 1.3 实验环境

- `Theia`: 一款前后端分离的、基于 web 的 云 IDE。
- `Preview 或 Mini Browser`：浏览器，右键点击目标 html 文件，选择 open with 下的 Preview 或 Mini Browser 即可在 IDE 中打开浏览器。

#### 1.4 参考链接

- [jQuery 中文网](https://www.jquery123.com/)

## 二、事件

现在我们来讲解 jQuery 事件。

### 2.1 加载 DOM

在第一个实验中，我们简单的对比了一下 JavaScript 中原生的 `window.onload` 方法和 jQuery 中 `$(document).ready()` 方法的区别，下面我们再详细的对比一下它们之间的区别。

（1）执行时机

`window.onload` 方法是在网页中所有的元素（包括元素的所有关联文件）完全加载到浏览器后才执行，也就是说这个时候 JavaScript 才可以访问网页中的任何元素。而通过 jQuery 中 `$(document).ready()` 方法注册的事件处理程序，在 DOM 完全就绪时就可以被调用，也就是说这个时候网页的所有元素对 jQuery 而言都是可以访问的，但是并不意味着这些元素相关联的文件都已经下载完毕了。

（2）多次使用

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

（3）简写方式

```js
$(document).ready(function(){

});

// 可以简写成

$(funciton(){

})
```

另外 \$(document) 也可以简写成 \$(),当 \$() 不带参数时，默认参数就是 “document”，因此也可以简写为：

```js
$().ready(function () {});
```

### 2.2 事件绑定

```js
bind()   向元素添加事件处理程序（3.0 版本已经弃用）
on()     向元素添加事件处理程序（`自 jQuery 版本 1.7 起，on() 方法是向被选元素添加事件处理程序的首选方法。`）
one()    向被选元素添加一个或多个事件处理程序。该处理程序只能被每个元素触发一次
```

`on()` 方法在被选元素及子元素上添加一个或多个事件处理程序。自 jQuery 版本 1.7 起，`on()` 方法是 `bind()`、`live()` 和 `delegate()` 方法的新的替代品。该方法给 API 带来很多便利，我们推荐使用该方法，它简化了 jQuery 代码库。

注意：

- 使用 `on()` 方法添加的事件处理程序适用于当前及未来的元素（比如由脚本创建的新元素）。

- 如需移除事件处理程序，请使用 `off()` 方法。

- 如需添加只运行一次的事件然后移除，请使用 `one()` 方法。

语法为：

```js
$(selector).on(event,childSelector,data,function)
```

参数说明：

- event 必需。表示事件类型，规定要从被选元素移除的一个或多个事件或命名空间。由空格分隔多个事件值，也可以是数组。必须是有效的事件。事件类型包括：blur、focus、load、resize、scroll、unload、click、dbclick、mousedown、mouseup、mousemove、mouseover、mouseout、keydown、keypress 等，也可以为自定义名称。

- childSelector 可选。规定只能添加到指定的子元素上的事件处理程序（且不是选择器本身，比如已废弃的 `delegate()` 方法）。

- data 可选。规定传递到函数的额外数据。

- function 可选。规定当事件发生时运行的函数。

（1）简单使用

示例：为 li 元素绑定 click 事件

```html
<!DOCTYPE html>
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
        $("ul li").on("click", function () {
          $(this).clone().appendTo("ul");
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555309989711.png/wm)

（2）简写绑定事件

像 `click`，`mouseover` 和 `mouseout` 这类事件，我们经常会用到，jQuery 为此提供了一套简写的方法，使得我们能够减少代码量。

示例：将上节事件绑定的例子简写绑定事件

```html
<!DOCTYPE html>
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
        $("ul li").click(function () {
          $(this).clone().appendTo("ul");
        });
      });
    </script>
  </body>
</html>
```

想要了解更多关于事件绑定的案例，可以访问 [jQuery 中文网 绑定事件处理器](https://www.jquery123.com/category/events/event-handler-attachment/)。

### 2.3 合成事件

`hover()` 方法的语法结构为：

```js
$(selector).hover(inFunction, outFunction);
```

参数说明：

- inFunction 必需。规定 mouseover 事件发生时运行的函数。

- outFunction 可选。规定 mouseout 事件发生时运行的函数。

`hover()` 方法用于模拟光标悬停事件。当光标移动到元素上时，会触发指定的第 1 个函数，当光标移出这个元素时，会触发指定的第 2 个函数。

示例：当鼠标移动到 li 上时字体大小变成 24px，移开变为 14px。

```html
<!DOCTYPE html>
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
        $("ul li").hover(
          function () {
            $(this).css({
              "font-size": "24px",
            });
          },
          function () {
            $(this).css({
              "font-size": "14px",
            });
          }
        );
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555311841017.png/wm)

此外还有一个 toggle 事件也是合成事件，但是它在 jQuery 1.9 中已经移除，有兴趣了解的同学，可以访问 [jQuery 中文网 toggle 事件](https://www.jquery123.com/toggle-event/)。

### 2.4 事件冒泡

（1）什么是事件冒泡

当事件发生后，这个事件就要开始传播(从里到外或者从外向里)。为什么要传播呢？因为事件源本身（可能）并没有处理事件的能力，即处理事件的函数（方法）并未绑定在该事件源上。例如我们点击一个按钮时，就会产生一个 click 事件，但这个按钮本身可能不能处理这个事件，事件必须从这个按钮传播出去，从而到达能够处理这个事件的代码中（例如我们给按钮的 onclick 属性赋一个函数的名字，就是让这个函数去处理该按钮的 click 事件），或者按钮的父级绑定有事件函数，当该点击事件发生在按钮上，按钮本身并无处理事件函数，则传播到父级去处理。

注：定义来自于搜狗百科。

来看个例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div id="content">
      我是外层div元素
      <span>我是内部span元素</span>
      我是外层div元素
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("span").on("click", function () {
          alert("我是内部span元素，我被点击了");
        });
        $("#content").on("click", function () {
          alert("我是外部div元素，我被点击了");
        });
        $("body").on("click", function () {
          alert("我是body元素，我被点击了");
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555314035990.png/wm)

在点击 span 元素的时候，会触发 span 元素的 click 事件，会输出三条记录，如上图所示，这就是事件冒泡所引起的。span 元素的 click 事件按照以下的顺序冒泡：

- span

- div

- body

之所以称为事件冒泡，是因为事件会按照 DOM 的层次结构像水泡一样不断向上直至顶端。

（2）事件冒泡引发的问题

事件冒泡可能会引起预料之外的结果，比如我们上面的例子中，需求是点击 span 元素，只触发 span 元素的 click 事件，然而 div 元素 和 body 元素的 click 事件也被触发了，因此，我们需要对事件的作用范围进行限制。

- 事件对象：在程序中使用事件对象非常简单，只需要为函数添加一个参数。比如：

```js
$("element").on("click", function (event) {
  //event 表示事件对象
});
```

- 停止事件冒泡：停止事件冒泡可以阻止事件中其他对象的事件处理函数被执行，在 jQuery 中提供了 `event.stopPropagation()` 方法来停止冒泡。使用 `event.isPropagationStopped()` 方法来检查指定的事件上是否调用了该方法，如果 `event.stopPropagation()` 被调用则该方法返回 true，否则返回 false。前面的例子可以改写为：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div id="content">
      我是外层div元素
      <span>我是内部span元素</span>
      我是外层div元素
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("span").on("click", function (event) {
          alert("我是内部span元素，我被点击了");
          event.stopPropagation(); //停止事件冒泡
        });
        $("#content").on("click", function (event) {
          alert("我是外部div元素，我被点击了");
          event.stopPropagation(); //停止事件冒泡
        });
        $("body").on("click", function () {
          alert("我是body元素，我被点击了");
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555315204472.png/wm)

- 阻止默认行为：网页中的元素都有自己默认的行为。比如单击超链接后会跳转，单击“提交”按钮后表单会提交，有时候我们需要阻止元素的默认行为。在 jQuery 中提供了 `event.preventDefault()` 方法阻止元素发生默认的行为。使用 `event.isDefaultPrevented()` 方法来检查指定的事件上是否调用了 `preventDefault()` 方法。

例子：阻止链接打开 url

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <a href="https://www.lanqiao.cn/">实验楼</a>
    <script type="text/javascript">
      $(document).ready(function () {
        $("a").click(function (event) {
          event.preventDefault();
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555315645670.png/wm)

另外表单中我们可以使用该方法来阻止表单的提交，比如当用户名为空时，点击提交按钮，出现提示信息让用户输入内容，并且表单不能提交，只有在用户名里输入内容时，才能提交表单。

- 事件捕获：事件捕获和事件冒泡刚好是两个相反的过程，事件捕获是从最顶端往下开始触发。jQuery 不支持事件捕获，这里简单提一下它们的区别，如果想要使用事件捕获，请直接使用原生的 JavaScript。

### 2.5 事件对象的属性

jQuery 在遵循 W3C 规范的情况下，对事件对象的常用属性进行了封装，使得事件处理在各大浏览器下都可以正常运行而不需要进行浏览器类型判断。

（1）event.type 获取事件的类型。

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <a href="https://www.lanqiao.cn/">实验楼</a>
    <script type="text/javascript">
      $(document).ready(function () {
        $("a").click(function (event) {
          event.preventDefault();
          alert(event.type);
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555316313387.png/wm)

（2）`event.stopPropagation()` 方法，防止事件冒泡到 DOM 树上，也就是不触发的任何前辈元素上的事件处理函数。参考上面。JavaScript 中符合 W3C 规范的 `stopPropagation()` 方法在 IE 浏览器下无效，jQuery 对其进行了封装，使之能兼容各种浏览器。

（3）`event.preventDefault()` 方法， 如果调用这个方法，默认事件行为将不再触发。参考上面。JavaScript 中符合 W3C 规范的 `preventDefault()` 方法在 IE 浏览器下无效，jQuery 对其进行了封装，使之能兼容各种浏览器。我们可以用 `event.isDefaultPrevented()` 来确定这个方法是否(在那个事件对象上)被调用过了。

（4）`event.target` 属性返回哪个 DOM 元素触发了事件。target 属性可以是注册事件时的元素，或者它的子元素。通常用于比较 `event.target` 和 `this` 来确定事件是不是由于冒泡而触发的。经常用于事件冒泡时处理事件委托。

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <h1>我是标题</h1>
    <p>我是段落</p>
    <button>我是按钮</button>
    <p>
      标题，段落和按钮元素设置了点击事件。分别点击元素查看是哪个元素的事件被触发了。
    </p>
    <div style="color:red;"></div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("p, button, h1").click(function (event) {
          $("div").html("通过 " + event.target.nodeName + " 元素触发。");
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555316922013.png/wm)

（5）`event.pageX` 和 `event.pageY` 分别获取鼠标相对于文档的左边缘的位置（左边）和鼠标相对于文档的顶部边缘的位置（坐标）。如果没有使用 jQuery，那么在 IE 浏览器中使用 `event.x/event.y`，而在 Firefox 浏览器中使用 `event.pageX/event.pageY`,如果页面中有滚动条，则还要加上滚动条的宽度和高度。

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>鼠标指针在: <span></span></p>

    <script type="text/javascript">
      $(document).ready(function () {
        $(document).mousemove(function (event) {
          $("span").text("X: " + event.pageX + ", Y: " + event.pageY);
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555318999325.png/wm)

想要了解更多关于事件对象的属性的知识，可以访问 [jQuery 中文网事件对象](https://www.jquery123.com/category/events/event-object/)

### 2.6 移除事件

在绑定事件的过程中，不仅可以为同一个元素绑定多个事件，也可以为多个元素绑定同一个事件。比如：

```js
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script type="text/javascript" src="jquery-3.3.1.js"></script>
    </head>
    <body>
        <button>点击按钮</button>
        <div id="test">

        </div>

        <script type="text/javascript">
            $(document).ready(function() {
                $("button").on("click", function() {
                    $('#test').append("<p>我是绑定函数1</p>");
                }).on("click", function() {
                    $('#test').append("<p>我是绑定函数2</p>");
                }).on("click", function() {
                    $('#test').append("<p>我是绑定函数3</p>");
                });

            });
        </script>
    </body>
</html>

```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555320003100.png/wm)

与 `on()` 方法绑定事件相对应的 `off()` 方法用来移除事件。自 jQuery 版本 1.7 起，`off()` 方法是 `unbind()`、`die()` 和 `undelegate()` 方法的新的替代品。该方法给 API 带来很多便利，我们推荐使用该方法，它简化了 jQuery 代码库。

注:如需移除指定的事件处理程序，当事件处理程序被添加时，选择器字符串必须匹配 `on()` 方法传递的参数。

语法为：

```js
$(selector).off(event,selector,function(eventObj),map)
```

参数说明：

- event 必需。规定要从被选元素移除的一个或多个事件或命名空间。由空格分隔多个事件值。必须是有效的事件。

- selector 可选。规定添加事件处理程序时最初传递给 on() 方法的选择器。

- function(eventObj) 可选。规定当事件发生时运行的函数。

- map 规定事件映射 ({event:function, event:function, ...})，包含要添加到元素的一个或多个事件，以及当事件发生时运行的函数。

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>点击这个段落修改它的背景颜色。</p>
    <p>点击一下按钮再点击这个段落( click 事件被移除 )。</p>
    <button>移除 click 事件</button>

    <script type="text/javascript">
      $(document).ready(function () {
        $("p").on("click", function () {
          $(this).css("background-color", "red");
        });
        $("button").click(function () {
          $("p").off("click");
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555322178079.png/wm)

注：如需添加只运行一次的事件然后移除，请使用 `one()` 方法。语法为：

```js
$(selector).one(event,data,function)
```

参数说明：

- event 必需。规定添加到元素的一个或多个事件。由空格分隔多个事件值。必须是有效的事件。

- data 可选。规定传递到函数的额外数据。

- function 必需。规定当事件发生时运行的函数。

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>这是一个段落 。</p>
    <p>这是另外一个段落。</p>
    <p>点击任意一个段落来修改段落的字体大小，每个段落只会执行一次。</p>

    <script type="text/javascript">
      $(document).ready(function () {
        $("p").one("click", function () {
          $(this).animate({
            fontSize: "+=6px",
          });
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555379171537.png/wm)

### 2.7 模拟操作

（1）常用模拟

我们前面的例子中都是需要用户操作，比如 click 事件必须要用户单击才能触发，但有时候，我们需要通过模拟用户操作，来达到单击的效果。在 jQuery 中，可以使用 `trigger()` 方法来完成模拟操作。语法为：

```js
$(selector).trigger(event,param1,param2,...)
```

参数说明：

- event 必需。规定指定元素上要触发的事件。可以是自定义事件，或者任何标准事件。

- param1,param2,... 可选。传递到事件处理程序的额外参数。额外的参数对自定义事件特别有用。

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>这是一个段落 。</p>
    <p>这是另外一个段落。</p>
    <p>点击任意一个段落来修改段落的字体大小，每个段落只会执行一次。</p>

    <script type="text/javascript">
      $(document).ready(function () {
        $("p").one("click", function () {
          $(this).animate({
            fontSize: "+=6px",
          });
        });
        $("p").trigger("click");
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555380925438.png/wm)

（2）触发自定义事件

`trigger()` 方法不仅能触发浏览器支持的具有相同名称的事件，也可以触发自定义名称的事件。

例子：

```js
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script type="text/javascript" src="jquery-3.3.1.js"></script>

    </head>
    <body>
        <p>这是一个段落 。</p>
        <p>这是另外一个段落。</p>
        <p>点击任意一个段落来修改段落的字体大小，每个段落只会执行一次。</p>

        <script type="text/javascript">
            $(document).ready(function() {
                $("p").one("myClick", function() {
                    $(this).animate({
                        fontSize: "+=6px"
                    });
                });
                //触发自定义事件
                $("p").trigger("myClick");
            });
        </script>
    </body>
</html>

```

运行效果同上。

（3）传递数据

`trigger(type,[data])` 方法有两个参数，第 1 个参数是要触发的事件类型，第 2 个参数是要传递给处理函数的附加数据，以数组形式传递。通常可以传递一个参数给回调函数来区别这次事件是代码触发的还是用户触发的。

例子：

```js
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script type="text/javascript" src="jquery-3.3.1.js"></script>

    </head>
    <body>
        <button id="btn">点击我</button>
        <div id="test"></div>

        <script type="text/javascript">
            $(document).ready(function() {
                $('#btn').on("myClick",function(event,message1,message2){
                    $('#test').append("<p>"+message1+message2+"</p>");
                });
                $('#btn').trigger("myClick",["我的自定义","事件"]);
            });
        </script>
    </body>
</html>

```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555383469050.png/wm)

（4）执行默认操作

`trigger()` 方法触发事件后，会执行浏览器默认操作。例如：

```js
$("input").trigger("focus");
```

以上代码不仅会触发为 input 元素绑定的 foucs 事件，也会使 input 元素本身得到焦点（浏览器的默认操作）。如果我们只想触发绑定的 focus 事件，而不想执行浏览器默认操作，我们可以使用 `triggerHandler()` 方法。它们之间的不同之处有：

- `triggerHandler()` 不触发事件的默认行为。（比如表单提交）

- `.trigger()` 会操作 jQuery 对象匹配的所有元素，而 `.triggerHandler()` 只影响第一个匹配元素。

- 由 `.triggerHandler()` 创建的事件不会在 DOM 树中冒泡；如果目标元素不直接处理它们，则不会发生任何事情。

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <button id="old">.trigger( "focus" )</button>
    <button id="new">.triggerHandler( "focus" )</button><br /><br />
    <p>
      执行 .trigger 后 input 输入框自动获取焦点，触发事件的默认行为，而
      .triggerHandler 仅仅
      执行了指定的事件浏览器并未执行动作，输入框也没有获取焦点。
    </p>
    <input type="text" value="将获取焦点" />
    <script type="text/javascript">
      $(document).ready(function () {
        $("#old").click(function () {
          $("input").trigger("focus");
        });
        $("#new").click(function () {
          $("input").triggerHandler("focus");
        });
        $("input").focus(function () {
          $("<span>获取焦点!</span>").appendTo("body").fadeOut(1000);
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555384672894.png/wm)

## 三、动画

通过 jQuery 中的动画方法，我们能够轻松的为我们的页面添加非常棒的视觉效果。

### 3.1 show()方法和 hide()方法

（1）`show()` 方法

`show()` 方法显示隐藏的被选元素。另外需要注意的是 `show()` 适用于通过 jQuery 方法和 CSS 中 `display:none` 隐藏的元素（不适用于通过 `visibility:hidden` 隐藏的元素）。

语法为：

```js
$(selector).show(speed, easing, callback);
```

参数说明：

- speed 可选。规定显示效果的速度。可能的值：毫秒，"slow"，"fast"。

- easing 可选。规定在动画的不同点上元素的速度。默认值为 "swing"。可能的值："swing" 表示在开头/结尾移动慢，在中间移动快。"linear" 表示匀速移动。提示：扩展插件中提供更多可用的 easing 函数。

- callback 可选。`show()` 方法执行完之后，要执行的函数。

注意: 如果使用!important 在你的样式中，比如 `display: none !important`，如果你希望`.show()` 方法才能正常工作，必须使用 `.css('display', 'block !important')` 重写样式。

（2）`hide()` 方法

与 `show()` 方法相对应的 `hide()` 方法隐藏被选元素。与 CSS 属性 display:none 类似。隐藏的元素不会被完全显示（不再影响页面的布局）。

语法为：

```js
$(selector).hide(speed, easing, callback);
```

参数说明与 `show()` 方法一致。只不过是把显示改为了隐藏。

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>我是测试段落</p>
    <button class="btn1">隐藏</button>
    <button class="btn2">显示</button>
    <script type="text/javascript">
      $(document).ready(function () {
        $(".btn1").click(function () {
          $("p").hide(1000, function () {
            $("p").css("background-color", "yellow");
          });
        });
        $(".btn2").click(function () {
          $("p").show(1000, function () {
            $("p").css("background-color", "red");
          });
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555394267515.png/wm)

想要了解更多关于 show()方法和 hide()方法的知识，可以访问 [jQuery 中文网基本特效](https://www.jquery123.com/category/effects/basics/)。

### 3.2 fadeIn() 方法和 fadeOut() 方法

（1）`fadeIn()` 方法

`fadeIn()` 方法通过淡入的方式显示匹配元素。它逐渐改变被选元素的不透明度，从隐藏到可见（褪色效果）。隐藏的元素不会被完全显示（不再影响页面的布局）。该方法通常与 `fadeOut()` 方法一起使用。

语法：

```js
$(selector).fadeIn(speed, easing, callback);
```

参数说明同上。

（2）`fadeOut()` 方法

`fadeOut()` 方法通过淡出的方式隐藏匹配元素。它逐渐改变被选元素的不透明度，从可见到隐藏（褪色效果）。隐藏的元素不会被完全显示（不再影响页面的布局）。该方法通常与 `fadeIn()` 方法一起使用。

语法：

```js
$(selector).fadeOut(speed, easing, callback);
```

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>我是测试段落</p>
    <button class="btn1">隐藏</button>
    <button class="btn2">显示</button>
    <script type="text/javascript">
      $(document).ready(function () {
        $(".btn1").click(function () {
          $("p").fadeOut(2000, function () {
            $("p").css("background-color", "yellow");
          });
        });
        $(".btn2").click(function () {
          $("p").fadeIn(2000, function () {
            $("p").css("background-color", "red");
          });
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555395068905.png/wm)

想要了解更多关于淡入淡出的知识，可以访问 [jQuery 中文网渐变](https://www.jquery123.com/category/effects/fading/)。

### 3.3 slideUp() 方法和 slideDown() 方法

（1）`slideUp()` 方法

`slideUp()` 方法以滑动方式隐藏被选元素。隐藏的元素不会被完全显示（不再影响页面的布局）。如需以滑动方式显示元素，请查看 `slideDown()` 方法。

语法为：

```js
$(selector).slideUp(speed, easing, callback);
```

（2）`slideDown()` 方法

`slideDown()` 方法以滑动方式显示被选元素。`slideDown()` 适用于通过 jQuery 方法隐藏的元素，或在 CSS 中声明 display:none 隐藏的元素（不适用于通过 visibility:hidden 隐藏的元素）。

语法为：

```js
$(selector).slideDown(speed, easing, callback);
```

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <p>我是测试段落</p>
    <button class="btn1">隐藏</button>
    <button class="btn2">显示</button>
    <script type="text/javascript">
      $(document).ready(function () {
        $(".btn1").click(function () {
          $("p").slideUp(2000, function () {
            $("p").css("background-color", "yellow");
          });
        });
        $(".btn2").click(function () {
          $("p").slideDown(2000, function () {
            $("p").css("background-color", "red");
          });
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555396671654.png/wm)

想要了解更多关于滑动的知识，可以访问 [jQuery 中文网滑动](https://www.jquery123.com/category/effects/sliding/)

### 3.4 animate()方法

在前面我们已经讲了 3 种类型的动画，其中 `show()` 方法和 `hide()` 方法会同时修改元素的多个样式属性，即高度，宽度和不透明度。`fadeIn()` 方法和 `fadeOut()` 方法只会修改元素的不透明度。`slideUp()` 方法和 `slideDown()` 方法只会改变元素的高度。但是这远远不够满足用户的各种需求，因此我们需要使用 `animate()` 方法。

`animate()` 方法，用来实现自定义动画。该方法通过 CSS 样式将元素从一个状态改变为另一个状态。CSS 属性值是逐渐改变的，这样就可以创建动画效果。只有数字值可创建动画（比如 `"margin:30px"`）。字符串值无法创建动画（比如 `"background-color:red"`）。

提示：请使用 "+=" 或 "-=" 来创建相对动画。

语法：

```js
$(selector).animate({ params }, speed, callback);
```

参数说明：

- params 参数，必需的，定义形成动画的 CSS 属性。

- speed 参数，可选的，规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

- callback 参数，可选的，是动画完成后所执行的函数名称。

注：默认情况下，所有 HTML 元素都有一个静态位置，且无法移动。
如需对位置进行操作，比如使用 “top”，“right”，“bottom”，“left” 属性要记得首先把元素的 CSS position 属性设置为 relative、fixed 或 absolute！

（1）简单动画

示例：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      .title {
        position: relative;
        border: 1px solid red;
        height: 100px;
        width: 200px;
      }
    </style>
  </head>
  <body>
    <div class="title">shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $(".title").animate(
          {
            left: "500px",
          },
          3000
        );
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555399946058.png/wm)

（2）累加、累减动画

使用 "+=" 或 "-=" 来创建相对动画。

例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      .title {
        position: relative;
        border: 1px solid red;
        height: 100px;
        width: 200px;
      }
    </style>
  </head>
  <body>
    <div class="title">shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $(".title").animate(
          {
            left: "500px",
            height: "+=150px",
            width: "+=150px",
          },
          3000
        );
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555400394641.png/wm)

（3）多重动画

- 同时执行多个动画。如上面的例子所示，div 元素在向右滑动的同时，也会放大。

- 按顺序执行多个动画。我们只需要把上面的代码拆开即可，然后按照顺序写就可以了。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      .title {
        position: relative;
        border: 1px solid red;
        height: 100px;
        width: 200px;
      }
    </style>
  </head>
  <body>
    <div class="title">shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $(".title").animate(
          {
            left: "500px",
          },
          3000
        );
        $(".title").animate(
          {
            height: "+=150px",
          },
          3000
        );
        $(".title").animate(
          {
            width: "+=150px",
          },
          3000
        );
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555401036249.png/wm)

因为 `animate()` 方法都是对同一个 jQuery 对象进行操作，所以也可以改成链式的写法，代码如下：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      .title {
        position: relative;
        border: 1px solid red;
        height: 100px;
        width: 200px;
      }
    </style>
  </head>
  <body>
    <div class="title">shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $(".title")
          .animate(
            {
              left: "500px",
            },
            3000
          )
          .animate(
            {
              height: "+=150px",
            },
            3000
          )
          .animate(
            {
              width: "+=150px",
            },
            3000
          );
      });
    </script>
  </body>
</html>
```

注：想要了解更多关于 `animate()` 的知识，可以访问 [jQuery 中文网 animate](https://www.jquery123.com/animate/)

### 3.5 动画回调函数

在前面动画参数的说明中我们提到了一个 callback 回调函数，那么它到底是用来干什么的呢？来看个例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      p {
        display: none;
      }
    </style>
  </head>
  <body>
    <button>显示</button>
    <p>我是测试段落</p>
    <script type="text/javascript">
      $(document).ready(function () {
        $("button").click(function () {
          $("p").show(1000);
          $("p").css("background-color", "red");
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555402564760.png/wm)

使用回调函数：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      p {
        display: none;
      }
    </style>
  </head>
  <body>
    <button>显示</button>
    <p>我是测试段落</p>
    <script type="text/javascript">
      $(document).ready(function () {
        $("button").click(function () {
          $("p").show(1000, function () {
            $("p").css("background-color", "red");
          });
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555402958762.png/wm)

区别就是如果不使用回调函数，`css()` 方法并不会加入到动画队列中，而是立即执行，可以使用回调函数 callback 对非动画实现排队。只要把 `css()` 方法写在最后一个动画的回调函数里面即可。callback 函数适用于 jQuery 中所有的动画效果。

### 3.6 停止动画和判断是否处于动画状态

（1）停止动画

jQuery `stop()` 方法用于在动画或效果完成前对它们进行停止。`stop()` 方法适用于所有 jQuery 效果函数，包括滑动、淡入淡出和自定义动画。

语法：

```js
$(selector).stop( [clearQueue ] [, jumpToEnd ] )
```

参数说明：

- clearQueue 参数，可选值，规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。

- jumpToEnd 参数，可选值，规定是否立即完成当前动画。默认是 false。

例子：

只停止当前正在进行的动画，停止当前动画后，队列中的下一个动画开始进行：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
    <style type="text/css">
      #panel,
      #flip {
        padding: 10px;
        text-align: center;
        background-color: #e5eecc;
        border: solid 1px #c3c3c3;
      }

      #panel {
        padding: 20px;
        display: none;
      }
    </style>
  </head>
  <body>
    <button id="stop">停止滑动</button>
    <div id="flip">点我向下滑动面板</div>
    <div id="panel">Hello syl!</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("#flip").click(function () {
          $("#panel").slideDown(5000);
          $("#panel").slideUp(5000);
        });
        $("#stop").click(function () {
          $("#panel").stop();
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555404483963.png/wm)

请大家将例子中的 `$("#panel").stop();` 改成 `$("#panel").stop(true);` 和 `$("#panel").stop(true,true);` 分别看看效果。

（2）判断元素是否处于动画状态

在使用 `animate()` 方法的时候，要避免动画积累而导致的动画与用户的行为不一致，当用户快速在某个元素上执行 `animate()` 动画时，就会出现动画积累，解决方法是判断元素是否处于动画状态，如果元素不处于动画状态，才为元素添加新的动画，否则不添加，代码如下：

```js
if (!$(element).is(":animated")) {
  //判断元素是否处于动画状态
  //如果当前没有进行动画，则添加新动画
}
```

（3）延迟动画

在动画执行的过程中，如果想对动画进行延迟操作，那么可以使用 `delay()` 方法。

语法为：

```js
$(selector).delay(speed, queueName);
```

参数说明：

- speed 可选。规定延迟的速度。可能的值：毫秒，"slow"，"fast"。

- queueName 可选。规定队列的名称。默认是 "fx"，标准效果队列。

例子：

使用 `delay()` 方法来设置不同的速度值。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <button>点击按钮，显示多个的 div 框。</button>
    <br /><br />
    <div
      id="div1"
      style="width:90px;height:90px;display:none;background-color:black;"
    ></div>
    <br />
    <div
      id="div2"
      style="width:90px;height:90px;display:none;background-color:green;"
    ></div>
    <br />
    <div
      id="div3"
      style="width:90px;height:90px;display:none;background-color:blue;"
    ></div>
    <br />
    <div
      id="div4"
      style="width:90px;height:90px;display:none;background-color:red;"
    ></div>
    <br />
    <div
      id="div5"
      style="width:90px;height:90px;display:none;background-color:purple;"
    ></div>
    <br />
    <script type="text/javascript">
      $(document).ready(function () {
        $("button").click(function () {
          $("#div1").delay("slow").fadeIn();
          $("#div2").delay("fast").fadeIn();
          $("#div3").delay(800).fadeIn();
          $("#div4").delay(2000).fadeIn();
          $("#div5").delay(4000).fadeIn();
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1555405322133.png/wm)

### 3.7 其他动画方法

除了上面我们所讲到的，jQuery 中还有四个专门用于交互的动画方法。

- `toggle()` 方法：`hide()` 和 `show()` 方法之间的切换。

- `slideToggle()` 方法：`slideUp()` 和 `slideDown()` 方法之间的切换。

- `fadeToggle()` 方法：在 `fadeIn()` 和 `fadeOut()` 方法之间进行切换。

- `fadeTo()` 方法：把被选元素逐渐改变至给定的不透明度。

自己简单的提一下，有兴趣的可以访问 [jQuery 中文网特效](https://www.jquery123.com/category/effects/)

另外我们特别需要注意的是 `animate()` 方法，因为它可以用来代替其他所有的动画方法。

## 四、总结

本节实验我们学习了 jQuery 中的事件和动画的操作，主要包括以下知识点：

- 加载 DOM

- 事件绑定

- 合成事件

- 事件冒泡

- 事件对象的属性

- 移除事件

- 模拟操作

- show()方法和 hide()方法

- fadeIn() 方法和 fadeOut() 方法

- slideUp() 方法和 slideDown() 方法

- animate()方法

- 动画回调函数

- 停止动画和判断是否处于动画状态

- 其他动画方法

在下一节实验中我们将学习 jQuery 对表单、表格的操作。
