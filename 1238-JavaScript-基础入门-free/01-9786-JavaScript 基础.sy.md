---
show: step
version: 1.0
enable_checker: true
---

# JavaScript 基础

## 简介

JavaScript 是一种脚本语言。学一种编程语言，首先就要从这种语言的基础语法入手。本节我们就将对 JavaScript 的基础语法进行学习。

#### 知识点

- JavaScript 是什么
- 变量
- 数字与运算符
- 数组
- null & undefined
- 字符串
- 类型转换

### 什么是 JavaScript

> JavaScript，通常缩写为 JS，是一种高级的，解释执行的编程语言。JavaScript 是一门基于原型、函数先行的语言，是一门多范式的语言，它支持面向对象编程，命令式编程，以及函数式编程。它提供语法来操控文本、数组、日期以及正则表达式等，不支持 I/O，比如网络、存储和图形等，但这些都可以由它的宿主环境提供支持。它已经由 ECMA（欧洲计算机制造商协会）通过 ECMAScript 实现语言的标准化。它被世界上的绝大多数网站所使用，也被世界主流浏览器（Chrome、IE、Firefox、Safari、Opera）支持。
>
> 虽然 JavaScript 与 Java 这门语言不管是在名字上，或是在语法上都有很多相似性，但这两门编程语言从设计之初就有很大的不同，JavaScript 的语言设计主要受到了 Self（一种基于原型的编程语言）和 Scheme（一门函数式编程语言）的影响。在语法结构上它又与 C 语言有很多相似，例如 if 条件语句、while 循环、switch 语句、do-while 循环等。
>
> 在客户端，JavaScript 在传统意义上被实现为一种解释语言，但在最近，它已经可以被即时编译（JIT）执行。随着最新的 HTML5 和 CSS3 语言标准的推行，它还可用于游戏、桌面和移动应用程序的开发和在服务器端网络环境运行，如 Node.js。

注：定义来自于维基百科。

#### JavaScript 的组成

- ECMAScript：JavaScript 的语法标准。
- DOM：JavaScript 操作网页上的元素的 API。
- BOM：JavaScript 操作浏览器的部分功能的 API。

#### JavaScript 的特点

- 可以使用任何文本编辑工具编写，然后使用浏览器就可以执行程序。
- 是一种解释型脚本语言：代码不进行预编译，从上往下逐行执行，不需要进行严格的变量声明。
- 主要用来向 HTML 页面添加交互行为。

### JavaScript 范例

实验需新建一个 test.html 文件，用于编写代码。后续的例子中，将不再提醒建立文件，大家根据个人需求自行创建对应的 html 文件，并完成代码练习：

> 由于实验楼使用是 WebIDE 的在线环境，所以有不熟悉对同学请阅读下：[实验楼 WebIDE 使用指南](https://www.lanqiao.cn/library/shiyanlou-docs/feature/webide)，_前端开发_ 部分的内容。

首先来看看范例代码：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title></title>
  </head>
  <body>
    <script>
      console.log('hello world');
    </script>
  </body>
</html>
```

上述代码的含义就是在我们的控制台打印一句话：hello world。首先教大家怎么查看：将上述代码复制到一个 html 文件中，然后在浏览器中运行，点击 F12，再点击控制台上的 Console，即可查看。如下图所示：

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547012930726.png/wm)

值得注意的是我们可以直接在控制台上输入 JavaScript 代码，然后点击 enter 让其执行。如下图所示我们执行几行简单的加减乘除：

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547013173817.png/wm)

通过上面的代码，我们可以看出 JavaScript 代码是放在 `<script>……</script>` 标签里，而包含 JavaScript 代码的 script 标签，我们可以放在 `<body>……</body>` 标签里，也可以放在 `<head>……</head>` 标签里。比如上述范例也可以这样写：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title></title>
    <script>
      console.log('hello world');
    </script>
  </head>
  <body></body>
</html>
```

执行结果没有什么区别，不同的是执行顺序，简单的来说，放在前面的会先执行。此外，和 CSS 引入相类似，JavaScript 也可以通过外部引入。首先我们需要创建一个扩展名为 .js 的文件，然后在 html 页面中引入它。同样的拿上述范例来修改，我们首先创建一个叫 test.js，名字可以自己取，但是扩展名一定要是 .js，只有这样才能够识别包含 JavaScript 代码的文件，然后在里面写上我们的 JavaScript 代码：

```javascript
console.log('hello world');
```

在 html 文件中写上如下代码：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title></title>
  </head>
  <body>
    <script src="test.js"></script>
  </body>
</html>
```

值得注意的是 test.js 文件要和你的 html 文件在同一目录下才能用上面的方式引用，否则的话需要使用绝对路径来引入 js 文件，具体引入需要根据实际情况灵活运用了。前两种方式都是直接把 JavaScript 代码放在 HTML 中，在页面加载的同时，那些 JavaScript 的代码就被解析了。而把 JavaScript 代码放在外部文件中，只有在事件被触发，需要该段 JavaScript 代码时，才调用执行。这样做有个好处，当页面比较复杂的时候，把大量的 JavaScript 代码放到外部文件，只有在需要的时候才执行，那么会明显地加快页面加载速度，而且实现结构化分离，也便于我们维护自己的代码，所以建议大家养成外部引入的方式来写我们的 JavaScript 代码。

### 变量

#### 变量是什么

在计算机中，数据都存在内存中。而一个变量，就是一个用于存放数值的容器，每个变量存放的数值是可变的，每个变量都有其独有的名字，每个变量都占有一段内存。

注：变量不是数值本身，变量仅仅是一个用于储存数值的容器。

#### 声明变量

通过 var 关键字来声明变量，比如：

```javascript
var name = '实验楼';
```

上述代码声明了一个名为 name 的变量，并赋值为“实验楼”。注意此处的等于符号（=）为赋值符号，不是我们传统意义上理解的等号。

变量的命名规则如下：

- 变量名必须以字母、下划线 “\_”、美元符号 “\$” 开头，不能以数字开头。
- 变量可以包含字母、数字、下划线和美元符号。
- 不能使用 JavaScript 中的关键字做为变量名。
- 变量名不能有空格。
- 变量名对大小写敏感，比如：name 和 Name 就是两个完全不同的变量。

另外在 JavaScript 中，变量也可以不作声明，而在使用时再根据数据的类型来确其变量的类型，如：

```javascript
x = 50; // 变量 x 为整数
```

#### 变量类型

- Number：你可以在变量中存储数字，不论这些数字是 10（整数），或者是 3.1415926（浮点数）。

```javascript
var x1 = 10;
var x2 = 3.1415926;
```

- String：存储字符（比如 "shiyanlou"）的变量，字符串可以是引号中的任意文本，你可以使用单引号或双引号，也可以在字符串中使用引号，只要不匹配包围字符串的引号即可：

```javascript
var carname = 'shiyanlou';
var carname = 'shiyanlou';
var answer = "I Love 'shiyanlou'";
var answer = 'I Love "shiyanlou"';
```

- Boolean：布尔类型的值有两种：true 和 false。通常被用于在适当的代码之后，测试条件是否成立，后续会讲到。
- Array：数组是一个单个对象，其中包含很多值，方括号括起来，并用逗号分隔。后续我们将会对数值进行详细的讲解，此处看两个简单的数值例子：

```javascript
var myNameArray = ['Tom', 'Bob', 'Jim'];
var myNumberArray = [10, 15, 20];
```

- Object：对象类型。同样的我们会在后续的课程中详细讲解什么是对象，此处先看一个简单的例子：

```javascript
var student = { name: 'Tom', age: 18 };
```

#### 动态类型

JavaScript 是一种“动态类型语言”，这意味着不同于其他一些语言（如 C、Java），你不需要指定变量将包含什么数据类型（例如 number 或 string），全部用 `var` 关键字声明就是了。比如如果你声明一个变量并给它一个带引号的值，浏览器就会知道它是一个字符串：

```javascript
var myString = 'Hello';
```

值得注意的就是引号中如果是一个数字，它依然是 string 类型的。我们可以在控制台中通过 `typeof` 函数，来查看我们声明的变量是什么类型的。

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547018886902.png/wm)

#### 注释

单行注释：用来描述下面一个或多行代码的作用。单行注释快捷键：`Ctrl + /`。

```javascript
// 这是一个变量
var name = 'zhangsan';
```

多行注释：用来注释多条代码。多行注释快捷键：`Ctrl + Shift + /`。

```javascript
/*
var name = "zhangsan";
var age = 18;
console.log(name, age);
*/
```

### 数字与运算符

#### 数字类型

- 整数。例如：1, 2, 100, -10。
- 浮点数：就是小数。例如：0.2, 3.1415926。
- 双精度：是一种特定类型的浮点数，它们具有比标准浮点数更高的精度。

注：我们这里作为了解就可以了。因为和其他语言不同的是，在 JavaScript 中，不管什么数字类型的全部用 `var` 声明就可以了，而且 JavaScript 中只有一个数据类型：Number。

#### 算术运算符

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547022268324.png/wm)

加减乘除的运算和我们平时所用的差不多，大家可以自行在控制台中尝试。这里需要提醒大家的是累加运算和累减运算的用法：

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547022963371.png/wm)

从上述的图片中可以看出我们不能将这些直接应用于一个数字，不是对值进行操作，而是为变量赋值一个新的更新值。`num++` 其实是 `num = num + 1` 的省略写法。从图片上来看可能不好理解 `num++` 和 `++num` 的区别。我们来写一个例子：

```javascript
var num1 = 3;
var result1 = num1++;
console.log(result1);
console.log(num1);
var num2 = 4;
var result2 = ++num2;
console.log(result2);
console.log(num2);
```

最后控制台打印的数字为：3 4 5 5。这里我们也可以看出 `num++` 和 `++num` 的区别，前者是先赋值再自增，后者是先自增再赋值。同样的 `num--` 和 `--num` 也是一样的效果，大家可以自行尝试一下。

#### 操作运算符

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547024191664.png/wm)

#### 比较运算符

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547024508797.png/wm)

在控制台中输入这些示例，如果成立的话会返回 true 如果不成立的话则返回 false。

#### 逻辑运算符

![1](https://doc.shiyanlou.com/document-uid18510labid147timestamp1515116985209.png/wm)

#### 运算符的优先级

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547261443500.png/wm)

## 数组（概述，方法）

### 数组概述

通过我们前面所学的我们可以知道如果我们要保存一个数据，我们可以用 `var` 关键字去定义一个变量，但是如果数据很多呢？难道我需要一个个去定义？比如一个班上很多人，我们 `var name1 = "张三"; var name2 = "李四"; ...` 一直这样定义下去？显然这是不科学的，而这也由此引出了我们的数组。首先我们来看看维基百科对于数组的定义：

> 在计算机科学中，数组数据结构（英语：array data structure），简称数组（英语：Array），是由相同类型的元素（element）的集合所组成的数据结构，分配一块连续的内存来存储。利用元素的索引（index）可以计算出该元素对应的存储地址。

最简单的数据结构类型是一维数组。例如，索引为 0 到 9 的 32 位整数数组，可作为在存储器地址 2000，2004，2008，...2036 中，存储 10 个变量，因此索引为 i 的元素即在存储器中的 2000+4×i 地址。数组第一个元素的存储器地址称为第一地址或基础地址。

#### 数组语法

```javascript
var myarray = new Array(1, 2, 3, 4, 5); // 创建数组同时赋值
// or
var myarray = [1, 2, 3, 4, 5]; // 直接输入一个数组（称“字面量数组”）
```

注：我们可以用上述两种方法创建数组，myarray 指的是定义的数组名，可以自己取，数组储存的数据可以是任何类型（数字，字符，布尔值等）。每一个值都有一个索引值，从 0 开始。比如：

```javascript
var color = ['red', 'green', 'blue', 'yellow'];
color[0]; // returns "red"
color[1]; // returns "green"
color[2]; // returns "blue"
color[3]; // returns "yellow"
color[4]; // returns undefined
```

#### 多维数组

多维数组就是数组中还包含数组，我们可以一层一层的来看。比如：

```javascript
var student = [
  ['张三', '男', '18'],
  ['李四', '女', '20'],
];
student[0][2]; // returns "18"
```

### 操作数组

#### 修改数组

修改数组中的元素内容也很简单，直接为它提供新值就可以了。比如：

```javascript
var color = ['red', 'green', 'blue', 'yellow'];
color[0] = 'black';
color; // returns ["black", "green", "blue", "yellow"]
```

#### 获取数组长度

同样的我们使用 `length` 来获取数组的长度。比如：

```javascript
var color = ['red', 'green', 'blue', 'yellow'];
color.length; // returns 4
```

#### 数组和字符串之间的转换

通过 `split()` 方法，将字符串转换为数组。比如：

```javascript
'1:2:3:4'.split(':'); // returns ["1", "2", "3", "4"]
'|a|b|c'.split('|'); // returns ["", "a", "b", "c"]
```

相反的我们通过 `join()` 方法将数组转换为字符串。比如：

```javascript
['1', '2', '3', '4'].join(':'); // returns "1:2:3:4"
['', 'a', 'b', 'c'].join('|'); // returns "|a|b|c"
```

注：我们同样可以使用 `toString()` 方法将数组转换为字符串，但是 `join()` 方法可以指定不同的分隔符，而 `toString()` 方法只能是逗号。

#### 添加和删除数组项

在数组尾部添加一个或多个元素，使用 `push()` 方法。比如：

```javascript
var arr = ['1', '2', '3', '4'];
arr.push('5', '6');
arr; // returns ["1", "2", "3", "4", "5", "6"]
```

使用 `pop()` 方法将删除数组的最后一个元素，把数组长度减 1，并且返回它删除的元素的值。如果数组已经为空，则 `pop()` 不改变数组，然后返回 undefined 值。比如：

```javascript
var arr = ['1', '2', '3', '4'];
arr.pop(); // returns 4
arr; // returns ["1", "2", "3"]
var arr1 = [];
arr1.pop(); // returns undefined
arr1; // returns []
```

`unshift()` 和 `shift()` 从功能上与 `push()` 和 `pop()` 完全相同，只是它们分别作用于数组的开始，而不是结尾。大家可以直接在控制台自行尝试一番。

注：数组的方法其实还有很多，我们将在后续的课程中一一为大家讲解。

### Null 和 Undefined

null 和 undefined 都表示无，但是也有一些区别。现在控制台上执行以下语句：

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547019784085.png/wm)

这里需要说明的是：`==` 是相等操作符，比较值是否相等，如果相等输出为 true，否则为 false。`===` 是全等操作符，比较值和类型是否都相等，如果都相等输出为 true，否则为 false。通过我们在控制台中的实践可以发现，null 和 undefined 的值不等于 0，它们的值相等，但是类型不相等。undefined 表示所有没有赋值变量的默认值，而 null 则表示一个变量不再指向任何对象地址。

### 字符串

在前面的变量章节中，我们已经简单讲过字符串的基础知识，这里我们再拓展一下。我们前面讲过我们可以使用单引号或双引号，也可以在字符串中使用引号，只要不匹配包围字符串的引号即可。比如：

```javascript
var carname = 'shiyanlou';
var carname = 'shiyanlou';
var answer = "I Love 'shiyanlou'";
var answer = 'I Love "shiyanlou"';
```

下面的代码将会出现错误，因为它会混淆浏览器和字符串的结束位置:

```javascript
var x1 = 'I've got no right to take my place...';
```

聪明的你可能会觉得这样不行，我们就换种方法，比如：

```javascript
var x1 = 'I have got no right to take my place...';
```

或者：

```javascript
var x1 = "I've got no right to take my place...";
```

没错这样做都是可行的方法，但是其实我们还有另外一种方法，使用转义符号。转义字符意味着我们对它们做一些事情，以确保它们在文本中被认可，而不是代码的一部分。在 JavaScript 中，我们通过在字符之前放一个反斜杠来实现这一点。试试这个:

```txt
var x1 = 'I\'ve got no right to take my place...';
```

常用的转义符：

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547519780285.png/wm)

#### 连接字符串

我们通过 “+” 连接字符串，比如在控制台中输入下面的代码：

```javascript
var one = 'Hello,jack.';
var two = "I'm rose";
result = one + two;
```

最后控制台显示结果为："Hello,jack.I'm rose"。

如果我们在控制台输入的是 "jack"+18 思考一下会报错吗？要不再自己动手实践看看结果？结果是：浏览器很聪明的把数字转换成了字符串，最后结果为 "jack18"。另外输入 "20"+"19"，最后的结果也是字符串 "2019"。

#### 字符串转换

我们可以通过 `toString()` 方法把数字转换成字符串。

```javascript
var myNum = 123;
var myString = myNum.toString();
typeof myString;
```

也可以通过 `Number()` 对象把传递给它的字符串类型的数字转换为数字。

```javascript
var myString = '123';
var myNum = Number(myString);
typeof myNum;
```

注：如果传递的不是纯数字的字符串，则返回的不是数字，而是 NaN（not a number）。

#### 获取字符串的长度

通过 `length` 属性获取字符串的长度，结果返回一个数字。比如：

```javascript
var myString = 'hello world ';
myString.length;
```

上述代码在控制台中运行的结果为：12（除了字母还有两个空格）。

如果你要查找这个字符串的第一个字符，你可以这么做：

```javascript
myString[0];
```

同样的你也可以查找到其他的字符，比如第五个字符，就是 `myString[4]`，这是因为电脑是从 0 开始，而不是 1，因此我们都要执行减一操作。

#### 在字符串中查找子字符串并提取它

1. 有时候我们需要判断一个较长的字符串里面是否存在一个我们指定的较小的字符串，就比如我们要查找一段话里面是否包含一个词或者一个字，这个时候我们可以使用 `indexof()` 方法来完成，更详细的语法为：

```javascript
str.indexOf(searchValue, fromIndex);
```

str 指的是我们需要查的较长的字符串，`searchValue` 表示我们指定的较小的字符串，`fromIndex` 表示调用该方法的字符串中开始查找的位置，是一个可选的任意整数值，也可以不写，默认是 0 表示从头开始查，`fromIndex < 0` 和 `fromIndex = 0` 是一样的效果，表示从头开始查找整个字符串。如果 `fromIndex >= str.length`，则该方法的返回值为 -1。这里有个特殊的情况：就是如果被查找的字符串（searchValue）是一个空字符串，那么当 `fromIndex <= 0` 时返回 0，`0 < fromIndex <= str.length` 时返回 `fromIndex`，`fromIndex > str.length` 时返回 `str.length`。这样说你可能不太明白，我们来实践一下看看实际效果：

```javascript
'Blue Sky'.indexOf('Blue'); // returns  0
'Blue Sky'.indexOf('Ble'); // returns -1
'Blue Sky'.indexOf('Sky', 0); // returns  5
'Blue Sky'.indexOf('Sky', -1); // returns  5
'Blue Sky'.indexOf('Sky', 5); // returns  5
'Blue Sky'.indexOf('Sky', 9); // returns -1
'Blue Sky'.indexOf('', 0); // returns  0
'Blue Sky'.indexOf('', 5); // returns 5
'Blue Sky'.indexOf('', 9); // returns 8
```

注：返回值指的是指定值第一次出现的索引，如果没有找到返回 -1。`indexOf()` 方法区分大小写，比如：

```javascript
'Blue Sky'.indexOf('blue'); // returns -1
'Blue Sky'.indexOf('Blue'); // returns 0
```

2. 当你知道字符串中的子字符串开始的位置，以及想要结束的字符时，`slice()` 方法可以用来提取它。比如：

```javascript
'Blue Sky'.slice(0, 3); // returns "Blu"
```

注：`slice(strat，end)`，第一个参数 start 是开始提取的字符位置，第二个参数 end 是提取的最后一个字符的后一个位置。所以提取从第一个位置开始，直到但不包括最后一个位置。另外第二个参数也可以不写，不写代表某个字符之后提取字符串中的所有剩余字符。比如：

```javascript
'Blue Sky'.slice(2); // returns "ue Sky"
```

#### 转换大小写

字符串方法 `toLowerCase()` 和 `toUpperCase()` 字符串并将所有字符分别转换为小写或大写。比如：

```javascript
var string = 'I like study';
string.toLowerCase(); // returns "i like study"
string.toUpperCase(); // returns "I LIKE STUDY"
```

#### 替换字符串的某部分

可以使用 `replace()` 方法将字符串中的一个子字符串替换为另一个子字符串。比如：

```javascript
var string = 'I like study';
string.replace('study', 'sleep'); // returns "I like sleep"
```

注意这样只能替换第一个出现的字符串，如果字符串是类似 `I like study study`，那么第二个 `study` 不会被替换。

此时可以使用全局替换方法。

```js
var string = 'I like study study';
string.replace(/study/g, 'sleep');
```

注：字符串的操作方法其实还有很多，我们将在后续的课程中再为大家作深入讲解。

## 详解类型转换

### 转换成字符串

1. 几乎每个值都有 `toString()` 方法。比如在控制台输入以下代码：

```javascript
var num = 8;
var numString = num.toString();
numString; // returns "8"
var result = true;
var resultString = result.toString();
resultString; // returns "true"
```

数值类型的 `toString()`，可以携带一个参数，输出对应进制的值。比如：

```javascript
var num = 16;
console.log(num.toString()); // "16" 默认是10进制
console.log(num.toString(10)); // "16"
console.log(num.toString(2)); // "10000"
console.log(num.toString(8)); // "20"
console.log(num.toString(16)); // "10"
```

2. `String()` 函数。比如在控制台中输入以下代码：

```javascript
var num1 = 8;
var num2 = String(num1);
num2; // returns "8"
var result = true;
var result1 = String(result);
result1; // returns "true"
```

注：因为有的值没有 `toString()` 方法，所以需要用 `String()`，比如 null 和 undefined。如下所示：

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547257971275.png/wm)

3. 使用拼接字符串。比如在控制台中输入以下代码：

```javascript
var num1 = 16;
var num2 = num1 + '';
num2; // returns "16"
```

### 转换成数值类型

1. `Number()` 可以把任意值转换成数值，如果要转换的字符串中有一个不是数值的字符，返回 NaN（not a number）。比如在控制台中依次输入以下代码：

```javascript
var num1 = Number(true);
num1; // true 返回 1，false 返回 0

var num2 = Number(undefined);
num2; // 返回 NaN

var num3 = Number(null);
num3; // 返回 0

var num4 = Number('syl');
num4; // 返回 NaN

var num5 = Number('   ');
num5; // 如果是空字符串返回 0

var num6 = Number(123);
num6; // 返回123，如果是数字型的字符，返回数字

var num7 = Number('123abc');
num7; // 返回 NaN
```

2. `parseInt()` 把字符串转换成整数。比如在控制台中依次输入以下代码：

```javascript
var num1 = parseInt('12.3abc');
num1; // 返回 12，如果第一个字符是数字会解析知道遇到非数字结束，只取整，不是约等于

var num2 = parseInt('abc123');
num2; // 返回 NaN，如果第一个字符不是数字或者符号就返回 NaN

var num3 = parseInt('');
num3; // 空字符串返回 NaN，但是 Number("")返回 0

var num4 = parseInt('520');
num4; // 返回 520

var num5 = parseInt('0xA');
num5; // 返回 10
```

另外值得注意的是，`parseInt()` 可以传递两个参数，第一个参数是要转换的字符串，第二个参数是要转换的进制。大家可以自行尝试一下。

3. `parseFloat()` 把字符串转换成浮点数。写法和 `parseInt()` 相似，主要有以下几个不同点：

- parseFloat 不支持第二个参数，只能解析 10 进制数。
- 如果解析的内容里只有整数，解析成整数。

例子：

```javascript
parseFloat('10'); // returns 10
parseFloat('10.000'); // returns 10
parseFloat('10.01'); // returns 10.01
parseFloat('10'); // returns 10
parseFloat('10 hours'); // returns 10
parseFloat('aaa 10'); // returns NaN
```

4. 执行减 0 操作。比如：

```javascript
var n = '10';
var m = n - 0;
m; // returns 10
```

值得注意的是，如果该字符串不是纯粹的数字字符串，那么它执行减 0 操作后，虽然变成了一个数字类型，但是返回值为 NaN。比如：

```javascript
var n = 'abc';
var m = n - 0;
m; // returns NaN
typeof m; // returns "number"
```

### 转换成布尔类型

1. 使用 `Boolean()` 函数。比如：

```javascript
Boolean(123); // returns true
Boolean('abc'); // returns true
Boolean(null); // returns false
Boolean(undefined); // returns false
Boolean(NaN); // returns false
Boolean(0); // returns false
Boolean(''); // returns false
Boolean(false); // returns false
Boolean('false'); // returns true
Boolean(-1); // returns true
```

2. 流程控制语句会把后面的值隐式转换为布尔类型。比如：

```javascript
var message;
if (message) {
  //会自动把 message 转换成 false，最后打印结果为：我很好
  console.log('你好啊');
} else {
  console.log('我很好');
}
```

## 总结

本节主要就 JavaScript 的基础语法进行了学习，主要包括以下内容：

- JavaScript 是什么
- 变量
- 数字与运算符
- 数组
- null & undefined
- 字符串
- 类型转换

本节内容是对 JavaScript 基础知识的初步认识。请务必手动完成文档中的所有例子，并实际运行查看效果。加深对基础知识的理解，并达到灵活运用的水平。