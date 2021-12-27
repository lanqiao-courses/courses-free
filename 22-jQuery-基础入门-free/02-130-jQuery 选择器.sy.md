---
show: step
version: 1.0
enable_checker: true
---

# jQuery 选择器

## 一、实验介绍

#### 1.1 实验内容

本节实验将带领大家学习 jQuery 选择器的使用。

#### 1.2 实验知识点

- jQuery 选择器是什么

- jQuery 选择器的优势

- 基本选择器

- 层次选择器

- 表单选择器

- 过滤选择器

#### 1.3 实验环境

- `Theia`: 一款前后端分离的、基于 web 的 云 IDE。
- `Preview 或 Mini Browser`：浏览器，右键点击目标 html 文件，选择 open with 下的 Preview 或 Mini Browser 即可在 IDE 中打开浏览器。

#### 1.4 参考链接

- [jQuery 中文网](https://www.jquery123.com/)

## 二、jQuery 选择器是什么

- jQuery 中 的选择器完全继承了 CSS 的风格，通过使用 jQuery 选择器，我们可以快速的找到目标 DOM 元素，然后对它们进行一系列操作，学会使用选择器是学习 jQuery 的基础，jQuery 的行为规则都必须在获取到元素后才能生效。

- jQuery 选择器允许您对 HTML 元素组或单个元素进行操作。

- jQuery 选择器基于元素的 id、类、类型、属性、属性值等"查找"（或选择）HTML 元素。 它基于已经存在的 CSS 选择器，除此之外，它还有一些自定义的选择器。

- jQuery 中所有选择器都以美元符号开头：`$()`。

- jQuery 选择器的写法和 CSS 选择器的写法十分相似，只不过两者的作用效果不同，CSS 选择器找到元素后是添加样式，而 jQuery 选择器找到元素后是添加行为，jQuery 中涉及操作 CSS 样式的部分比单纯的 CSS 功能更为强大，并且拥有跨浏览器的兼容性。

#### jQuery 选择器的优势

- 简洁的写法。

- 支持 CSS1 到 CSS3 选择器。兼容性良好，可以直接使用而无需考虑浏览器的兼容性。

- 完善的处理机制。使用 jQuery 选择器不仅比使用传统的 DOM 对象方法简洁得多，而且还能避免某些错误，比如：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div id="">
      实验楼
    </div>

    <script type="text/javascript">
      document.getElementById('syl').style.color = 'red';
    </script>
  </body>
</html>
```

运行后是会报错的，这是因为网页中并没有 id 名为 "syl" 的元素，报错如下所示：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554876053332.png/wm)

改进一下后的代码如下：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div id="">
      实验楼
    </div>

    <script type="text/javascript">
      if (document.getElementById('syl')) {
        document.getElementById('syl').style.color = 'red';
      }
    </script>
  </body>
</html>
```

这样就不会报错了，但是如果要操作的元素很多，对每个元素都进行一次判断，显然是不合理的，不判断的话，以后因为某种原因删除了网页上某个以前使用的元素，再来改也会很麻烦，而使用 jQuery 选择器的话，则不用担心这个问题：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div id="">
      实验楼
    </div>

    <script type="text/javascript">
      $('#syl').css('color', 'red');
    </script>
  </body>
</html>
```

另外需要特别注意的是，`$('#syl')` 获取的永远是对象，即使网页上没有此元素，因为当要用 jQuery 来检查某个元素再网页上是否存在时，不能使用下面的代码：

```js
if ($('#syl')) {
}
```

而应该根据获取到元素的长度来判断，代码如下所示：

```js
if ($('#syl').length > 0) {
}
```

或者转化成 DOM 对象来判断，代码如下：

```js
if ($('#syl')[0]) {
}
```

## 三、基本选择器

基本选择器是 jQuery 中最常用的选择器，也是最简单的选择器，它通过元素 id、class 和 标签名等来查找 DOM 元素。

#### （1）`ID Selector ("#id")` 选择一个具有给定 id 属性的单个元素

对于 ID 选择，jQuery 使用 JavaScript 函数 `document.getElementById()`，这是非常有效的。当另一个选择是附加到 ID 选择器，比如 `h2#pageTitle`，在确定作为匹配的元素前，jQuery 执行一个额外的检查。

调用 jQuery() (或 \$()) 带上一个选择器作为它的参数，将返回一个 jQuery 对象包含零个或一个 DOM 元素的集合。

每个 id 值在一个文件中只能使用一次。如果多个元素分配了相同的 ID，将只匹配该 ID 选择集合的第一个 DOM 元素。但这种行为不应该发生;有超过一个元素的文件使用相同的 ID 是无效的。

如果 ID 包含点号或冒号，你必须将 这些字符用反斜杠转义。

例子：

选择 id 为 demo 的元素，并为此元素设置长、宽、背景色。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div id="demo"></div>

    <script type="text/javascript">
      $(document).ready(function () {
        $('#demo').css({
          width: '100px',
          height: '100px',
          'background-color': 'red',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554877063246.png/wm)

#### （2）`Class Selector (".class")` 选择给定样式类名的所有元素

对于类选择器，如果浏览器支持，jQuery 使用 JavaScript 的原生 `getElementsByClassName()` 函数来实现。

例子：

选择 class 为 demo 的元素，并为此元素设置长、宽、背景色。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div class="demo"></div>

    <script type="text/javascript">
      $(document).ready(function () {
        $('.demo').css({
          width: '100px',
          height: '100px',
          'background-color': 'red',
        });
      });
    </script>
  </body>
</html>
```

注：运行效果同上。

#### （3）`Element Selector ("element")` 根据给定（html）标记名称选择所有的元素

调用 JavaScript 的 `getElementsByTagName()` 函数，当该表达式使用时返回相应的元素。

例子：

选择所有 div 元素,并为所有元素设置长、宽、背景色。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>shiyanlou1</div>
    <div>shiyanlou2</div>

    <script type="text/javascript">
      $(document).ready(function () {
        $('div').css({
          width: '100px',
          height: '100px',
          'background-color': 'red',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554877339871.png/wm)

#### （4）`All Selector ("*")` 选择所有元素

选择页面所有元素，包括 body。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>shiyanlou1</div>
    <div>shiyanlou2</div>

    <script type="text/javascript">
      $(document).ready(function () {
        $('*').css({
          width: '100px',
          height: '100px',
          'background-color': 'red',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554877493640.png/wm)

警告： 除非被它自己使用，否则 \* 选择器或通用选择器，其速度是极其慢的。

#### （5）`Multiple Selector ("selector1, selector2, selectorN")` 将每一个选择器匹配到的元素合并后一起返回

您可以指定任何数量的选择器组合成一个单一的结果。这个多个表达组合是一种有效的方法来选择不同的元素。因为他们将按在文件的顺序，DOM 元素的顺序在返回的 jQuery 象中可能不相同。另一种选择器组合是 `.add()` 方法。

例子：

用 “,” 分隔开然后再拼成一个选择器字符串，同时选择多个匹配的选择器的内容
选择页面所有元素，并设置字体大小。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>shiyanlou</div>
    <a href="https://www.lanqiao.cn/">https://www.lanqiao.cn/</a>

    <script type="text/javascript">
      $(document).ready(function () {
        $('div,a').css({
          'font-size': '30px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![图片描述](https://doc.shiyanlou.com/courses/22/1347963/d774804319bbfee04eff3e1af5db49ed-0/wm)

## 四、 层次选择器

如果想通过 DOM 元素之间的层次关系来获取特定元素，例如后代元素、子元素、相邻元素和同辈元素等，那么我们可以使用 jQuery 层次选择器。

#### （1）`Descendant Selector ("ancestor descendant")`

选中给定的祖先元素的 ancestor 中的所有 descendant 元素（后代元素）。一个元素的后代可能是该元素的一个孩子，孙子，曾孙等。

例子：

选择类名为 demo 的元素的所有后代 a 元素，并设置字体大小。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div class="demo">
      <a href="https://www.lanqiao.cn/">shiyanlou</a>
      <div class="innerDemo">
        <a href="https://www.lanqiao.cn/">SHIYANLOU</a>
      </div>
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        $('.demo a').css({
          'font-size': '30px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554878446028.png/wm)

#### （2）`Child Selector ("parent > child")`

选择所有指定“parent”元素中指定的"child"的直接子元素。

作为一个 CSS 选择器，这个子元素组合器被 Safari, Firefox, Opera, Chrome, 和 Internet Explorer 7 及以上版本等现代浏览器支持，但尤其不被 Internet Explorer6 及以下版本支持。然而在 jQuery 中，这个选择器（与其他所有选择器）能在所有支持的浏览器中工作，包括 IE6。

这个子元素组合器(E > F)和(E F)都作为后代组合，但是他们有所不同，更具体的是(E > F)它只会选择第一级的后代。

注：选择的是子元素，注意跟后代元素的区别。

例子：

选择类名为 demo 的子元素 a，并设置字体大小。（此时只有第一个 a 元素的字体会改变）

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div class="demo">
      <a href="https://www.lanqiao.cn/">shiyanlou</a>
      <div class="innerDemo">
        <a href="https://www.lanqiao.cn/">SHIYANLOU</a>
      </div>
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        $('.demo>a').css({
          'font-size': '30px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554878698478.png/wm)

#### （3）`Next Adjacent Selector ("prev + next")`

prev 和 next 是两个同级别的元素，选中在 prev 元素后面的 next 元素。

例子：

选中 class 为 demo 后面的 a 元素，并设置字体大小。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div class="demo">
      <div class="demo">shiyanlou</div>
      <a href="https://www.lanqiao.cn/">SHIYANLOU</a>
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        $('.demo+a').css({
          'font-size': '30px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：
![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554879264344.png/wm)

#### （4）`Next Siblings Selector ("prev ~ siblings")`

匹配 “prev” 元素之后的所有 兄弟元素。具有相同的父元素，并匹配过滤“siblings”选择器。

`prev + next` 和 `prev ~ siblings` 之间最值得注意的不同点是他们各自的可及之范围。前者只达到紧随的同级元素，后者扩展了该达到跟随其的所有同级元素。

例子：

选中 class 为 demo 的 div 元素后面的所有 a 同辈元素，并设置字体大小。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div class="demo">demo</div>
    <a href="https://www.lanqiao.cn/">shiyanlou</a>
    <a href="https://www.lanqiao.cn/">SHIYANLOU</a>
    <script type="text/javascript">
      $(document).ready(function () {
        $('.demo~a').css({
          'font-size': '30px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![图片描述](https://doc.shiyanlou.com/courses/uid897174-20190424-1556076741130)

在层次选择器中，第 1 个和第 2 个选择器比较常用，而后面两个因为在 jQuery 里面可以用更加简单的方法来代替，所以使用的几率相对会少些：

- 可以使用 next() 方法来代替 \$('prev+next') 选择器。比如 `$(".one + div);` 和 `$(".one").next("div");` 是等价的。

- 可以使用 nextAll() 方法来代替 \$('prev~siblings') 选择器。比如 `$("#prev~div");` 和 `$("#prev").nextAll("div");` 是等价的。

简单提一下后面要讲解的 siblings() 方法，`$("#prev~div");` 选择器只能选择 “prev” 元素后面的同辈 div 元素，而 siblings() 方法与前后位置无关，只要是同辈节点都能匹配。

```js
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script type="text/javascript" src="jquery-3.3.1.js"></script>
    </head>
    <body>
        <div id="">

        </div>
        <div id="prev">

        </div>
        <div id="">

        </div>
        <script type="text/javascript">
            $(document).ready(function() {
                //选取#prev之后的所有同辈div元素
                $("#prev~div").css("background", "#bbffaa");
                //同上
                $("#prev").nextAll("div").css("background", "#bbffaa");
                //选取#prev所有的同辈div元素，无论前后位置
                $("#prev").siblings("div").css({
                    "width": "100px",
                    "height": "100px",
                    "border": "1px solid red"
                })
            });
        </script>
    </body>
</html>

```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554881400692.png/wm)

## 五、表单选择器

为了使用户能够更加灵活的操作表单，jQuery 中专门加入了表单选择器，利用这个选择器，我们能够特别方便的获取到表单的某个或某类型的元素。

- :input 选取所有的 &lt;input&gt; 、&lt;textarea&gt;、&lt;select&gt;和 &lt;button&gt;元素。

- :text 选取所有的单行文本框。

- :password 选取所有的密码框

- :radio 选取所有的单选框

- :checkbox 选取所有的多选框

- :submit 选取所有的提交按钮

- :image 选取所有的图像

- :reset 选取所有的重置按钮

- :button 选取所有的按钮

- :file 选取所有的上传域

- :hidden 选取所有不可见元素

示例：选取所有的 input 元素，并设置高度。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <input text="text" />
    <input text="textaera" />
    <script type="text/javascript">
      $(document).ready(function () {
        $(':input').css({
          height: '300px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554882246059.png/wm)

另外需要注意的是 `$("#form1 :input")` 与 `$("#form1 input")` 的区别，这里就不过多的说明，大家可以先看自己的理解，然后写个测试页面验证一下。

如果想要获得表单内单行文本框的个数，可以这么写：

```js
//假定已经有一个id名为form1的表单
$('#form1 :text').lenth;
```

同理，其他的表单选择器的操作与此类似大家可以自行尝试着去写一写体验一番。更多例子可以访问 [jQuery 中文官网表单](https://www.jquery123.com/category/selectors/form-selectors/)。

## 六、 过滤选择器

过滤选择器主要是通过特定的过滤规则来筛选出所需要的 DOM 元素，过滤规则与 CSS 中的伪类选择器语法相同，即选择器都以一个冒号 `:` 开头，按照不同的过滤规则，过滤选择器可以分为：

- 基本过滤选择器

- 内容过滤选择器

- 可见性过滤选择器

- 属性过滤选择器

- 子元素过滤选择器

- 表单对象属性过滤选择器

#### （1）基本过滤选择器

- `:animated Selector` 选择所有正在执行动画效果的元素.

- `:eq() Selector` 在匹配的集合中选择索引值为 index 的元素。

- `:even Selector` 选择索引值为偶数的元素，从 0 开始计数。 也可以查看 odd.

- `:first Selector` 选择第一个匹配的元素。

- `:focus Selector` 选择当前获取焦点的元素。

- `:gt() Selector` 选择匹配集合中所有大于给定 index（索引值）的元素。

- `:header Selector` 选择所有标题元素，像 h1, h2, h3 等.

- `:lang() Selector` 选择指定语言的所有元素。

- `:last Selector` 选择最后一个匹配的元素。

- `:lt() Selector` 选择匹配集合中所有索引值小于给定 index 参数的元素。

- `:not() Selector` 选择所有元素去除不匹配给定的选择器的元素。

- `:odd Selector` 选择索引值为奇数元素，从 0 开始计数。同样查看偶数 even.

- `:root Selector` 选择该文档的根元素。

- `:target Selector` 选择由文档 URI 的格式化识别码表示的目标元素。

示例：选取所有的 input 元素中的第一个 input 元素，并设置高度。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <input text="text" />
    <input text="textaera" />
    <script type="text/javascript">
      $(document).ready(function () {
        $('input:first').css({
          height: '300px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554883163216.png/wm)

想要了解更多基本过滤选择器的实例可以访问[jQuery 中文官网基础过滤](https://www.jquery123.com/category/selectors/basic-filter-selectors/)。

#### （2）内容过滤选择器

内容过滤选择器的过滤规则主要体现在它所包含的子元素或文本内容上。

- `:contains() Selector` 选择所有包含指定文本的元素。

- `:empty Selector` 选择所有没有子元素的元素（包括文本节点）。

- `:has() Selector` 选择元素其中至少包含指定选择器匹配的一个种元素。

- `:parent Selector` 选择所有含有子元素或者文本的父级元素。

示例：选取包含文本“shiyanlou”的 div 元素，并设置字体大小。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>demo</div>
    <div>shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("div:contains('shiyanlou')").css({
          'font-size': '30px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554883406591.png/wm)

想要了解更多内容过滤选择器的实例可以访问[jQuery 中文官网内容过滤](https://www.jquery123.com/category/selectors/content-filter-selector/)。

#### （3）可见性过滤选择器

可见性过滤选择器是根据元素的可见和不可见状态来选择相应的元素。

- `:hidden Selector` 选择所有隐藏的元素。

- `:visible Selector` 选择所有可见的元素。

示例：选取所有可见的 div 元素，并设置字体大小。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>demo</div>
    <div>shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $('div:visible').css({ 'font-size': '30px' });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554883607660.png/wm)

注意：在可见性选择器中，需要特别注意的是选择器 :hidden,它不仅包括样式属性 display 为 "none" 的元素，也包括文本隐藏域（&lt;input type="hidden" /&gt;）和 visibility:hidden; 之类的元素。

想要了解更多可见性过滤选择器的实例可以访问[jQuery 中文官网可见性过滤](https://www.jquery123.com/category/selectors/visibility-filter-selectors/)。

#### （4）属性过滤选择器

属性过滤选择器的过滤规则是通过元素的属性来获取相应的元素。

- `Attribute Contains Prefix Selector [name|="value"]` 选择指定属性值等于给定字符串或以该字符串为前缀（该字符串后跟一个连字符“-” ）的元素。

- `Attribute Contains Selector [name*="value"]` 选择指定属性具有包含一个给定的子字符串的元素。（选择给定的属性是以包含某些值的元素）

- `Attribute Contains Word Selector [name~="value"]` 选择指定属性用空格分隔的值中包含一个给定值的元素。

- `Attribute Ends With Selector [name$="value"]` 选择指定属性是以给定值结尾的元素。这个比较是区分大小写的。

- `Attribute Equals Selector [name="value"]` 选择指定属性是给定值的元素。

- `Attribute Not Equal Selector [name!="value"]` 选择不存在指定属性，或者指定的属性值不等于给定值的元素。

- `Attribute Starts With Selector [name^="value"]` 选择指定属性是以给定字符串开始的元素

- `Has Attribute Selector [name]` 选择所有具有指定属性的元素，该属性可以是任何值。

- `Multiple Attribute Selector [name="value"][name2="value2"]` 选择匹配所有指定的属性筛选器的元素

示例：选取拥有 class 属性的 div 元素，并设置字体大小。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div>demo</div>
    <div class="shiyanlou">shiyanlou</div>
    <script type="text/javascript">
      $(document).ready(function () {
        $('div[class]').css({
          'font-size': '30px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554884415345.png/wm)

想要了解更多属性过滤选择器的实例可以访问[jQuery 中文官网属性过滤](https://www.jquery123.com/category/selectors/attribute-selectors/)。

#### （5）子元素过滤选择器

- `:first-child Selector` 选择所有父级元素下的第一个子元素。

- `:first-of-type Selector` 选择所有相同的元素名称的第一个兄弟元素。

- `:last-child Selector` 选择所有父级元素下的最后一个子元素。

- `:last-of-type Selector` 选择的所有元素之间具有相同元素名称的最后一个兄弟元素。

- `:nth-child() Selector` 选择的他们所有父元素的第 n 个子元素。

- `:nth-last-child() Selector` 选择所有他们父元素的第 n 个子元素。计数从最后一个元素开始到第一个。

- `:nth-last-of-type() Selector` 选择的所有他们的父级元素的第 n 个子元素，计数从最后一个元素到第一个。

- `:nth-of-type() Selector` 选择同属于一个父元素之下，并且标签名相同的子元素中的第 n 个。

- `:only-child Selector` 如果某个元素是其父元素的唯一子元素，那么它就会被选中。

- `:only-of-type Selector` 选择所有没有兄弟元素，且具有相同的元素名称的元素。

示例：选取类名为 demo 的元素的第一个子 div 元素，并设置字体大小。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div class="demo">
      <div>shiyanlou</div>
      <div>SHIYANLOU</div>
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        $('.demo div:first-child').css({
          'font-size': '30px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554884574356.png/wm)

想要了解更多子元素过滤选择器的实例可以访问[jQuery 中文官网子元素过滤](https://www.jquery123.com/category/selectors/child-filter-selectors/)。

#### （6）表单对象属性过滤选择器

- `:enabled Selector` 选择所有可用的（注：未被禁用的元素）元素。

- `:disabled Selector` 选择所有被禁用的元素。

- `:checked Selector` 匹配所有勾选的元素。

- `:selected Selector` 获取 select 元素中所有被选中的元素。

示例：选择被选中元素，并设置宽度。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript" src="jquery-3.3.1.js"></script>
  </head>
  <body>
    <div class="demo">
      <input type="checkbox" checked="checked" />
      <input type="checkbox" />
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        $('input:checked').css({
          width: '300px',
        });
      });
    </script>
  </body>
</html>
```

运行效果为：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1554885074473.png/wm)

注：特别需要注意的是选择器中的空格是不容忽视的，多一个空格或者少一个空格也许得到的结果就截然不同了，比如 `$('.test :hidden');` 带空格的是后代选择器，表示选取 class 为 test 的元素里面的隐藏元素，而 `$('.test:hidden');` 不带空格的是过滤选择器，表示选取隐藏的 class 为 test 的元素。

## 七、总结

本节实验我们主要学习了 jQuery 选择器的内容，包含以下知识点：

- jQuery 选择器是什么

- jQuery 选择器的优势
