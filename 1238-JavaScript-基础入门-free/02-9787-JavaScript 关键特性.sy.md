---
show: step
version: 1.0
enable_checker: true
---

# JavaScript 关键特性

## 简介

在这个章节中, 我们将继续学习 JavaScript 的关键基本特性，在这一章中我们将详细的学习条件控制语句、循环语句、函数模块、事件等通用代码块。

#### 知识点

- 条件语句
- 循环语句
- 函数

## 条件

生活中，条件与我们息息相关。举几个例子：如果这周放假，那么我就要出去玩；如果明天不下雨，我就和小花出去踢足球；如果我饿了，我要么吃饭，要么吃面，要么就忍着。同样的，在 JavaScript 中，我们也有条件语句，下面为大家讲解在 JavaScript 中如何使用条件语句。

#### if...else 语句

1. 最基本的 `if...else` 语句。它的语法为：

```javascript
if (条件) {
  // 当条件为 true 时执行的语句
} else {
  // 当条件为 false 时执行的语句
}
```

例子：

```javascript
if (3 > 2) {
  console.log("我真帅");
} else {
  console.log("不可能");
}
```

上述例子在控制台中打印的语句为：我真帅。

2. `if...else` 嵌套。它的语法是：

```javascript
if(条件 1){
    // 当条件 1 为 true 时执行的代码
    }
else if(条件 2){
    // 当条件 2 为 true 时执行的代码
    }
else{
    // 当条件 1 和 条件 2 都不为 true 时执行的代码
    }
```

注：根据实际情况，还可以嵌套更多的 `else if`。

例子：

```javascript
var d = new Date().getDay();
if (d == 0) {
  console.log("今天星期天");
} else if (d == 1) {
  console.log("今天星期一");
} else if (d == 2) {
  console.log("今天星期二");
} else {
  console.log("好多啊，我不想写了");
}
```

#### switch case 语句

从前面的例子中我们可以看出来，当条件很多的时候，一直嵌套 `else if` 语句，显然是有点不科学的，由此我们引出了 `switch case` 语句，先来看看它的语法：

```javascript
switch(k){
    case 1:
        执行代码块 1 ;
        break;
    case 2:
        执行代码块 2 ;
        break;
    default:
        默认执行（k 值没有在 case 中找到匹配时）;
}
```

通过 `switch case` 语句来改写上面的例子：

```javascript
var d = new Date().getDay();
switch (d) {
  case 0:
    console.log("今天星期天");
    break;
  case 1:
    console.log("今天星期一");
    break;
  case 2:
    console.log("今天星期二");
    break;
  case 3:
    console.log("今天星期三");
    break;
  case 4:
    console.log("今天星期四");
    break;
  case 5:
    console.log("今天星期五");
    break;
  default:
    console.log("今天星期六");
    break;
}
```

#### 三元运算符

语法：

```bash
条件表达式？结果 1:结果 2
```

含义：问号前面的位置是判断的条件，判断结果为 boolean 型，为 true 时执行结果 1，为 false 时执行结果 2。

例子:

```javascript
3 > 2 ? console.log("3 比 2 大") : console.log("3 比 2 小");
```

## 循环

其实前面讲 `switch case` 语句的例子的时候，大家可能当时觉得是要比嵌套的 `else if` 语句要简单些，但是大家有没有想过如果不止这么多呢？有成千上万个数据呢？用 `else if` 嵌套或者 `switch case` 来写显然都是不科学的，而这也由此引出了我们接下来要学习的：循环。通过循环函数来帮助我们完成一些重复性的工作。

#### for 循环

`for` 循环是我们编码中经常会使用到的。先来看看它的语法结构：

```javascript
for (初始化; 条件; 增量) {
  循环代码;
}
```

举个打印 1 到 100 的例子：

```javascript
for (var i = 1; i <= 100; i++) {
  console.log(i);
}
```

#### 使用 break 跳出循环

我们在前面的 `switch case` 结构中已经见过 `break` 语句了，当 `switch` 语句中符合输入表达式的情况满足时，`break` 语句立即退出 `switch` 语句并移动到之后的代码。上述的 `for` 循环例子，我们来加个条件，使其能打印一个能被 7 整除的整数。

```javascript
for (var i = 1; i <= 100; i++) {
  if (i % 7 == 0) {
    console.log(i);
    break;
  }
}
```

#### 使用 continue 跳过迭代

使用 `continue` 跳过迭代，不是完全跳出循环，而是跳过当前循环而执行下一个循环。比如我们使用 `continue` 可以实现打印 1 到 100 所有能被 7 整除的整数，而前面的例子中只能打印出：7。

```javascript
for (var i = 1; i <= 100; i++) {
  if (i % 7 == 0) {
    console.log(i);
    continue;
  }
}
```

这么写可能不好理解，大家或许会想到上面的例子不写 `continue` 最后打印的效果和写了是一样的啊，那 `continue` 是不是就没用了？其实 `continue` 主要是跳过当前循环去执行下一个循环也就是说，当前循环下的其他语句就不执行了。来看下面的例子：

```javascript
for (var i = 1; i <= 7; i++) {
  if (i % 7 == 0) {
    console.log(i);
    continue;
    console.log("*");
  }
}
```

大家可以把上面的代码运行一下，然后把 `continue` 删除，再运行一下，细细体会一下 `continue` 语句的作用。

#### while 语句 和 do while 语句

在 JavaScript 中不止有 `for` 循环，还有其他的循环语句。我们先来看看 `while` 循环的语法结构：

```javascript
while (条件) {
  // 需要执行的代码;
}
```

同样的，我们来写一个打印 1 到 100 之间整数的例子：

```javascript
var i = 1;
while (i <= 100) {
  console.log(i);
  i++;
}
```

`do while` 循环的语法结构：

```javascript
do {
  // 需要执行的代码;
} while (条件);
```

例子：

```javascript
var i = 1;
do {
  console.log(i);
  i++;
} while (i <= 100);
```

注：而这两者的区别是，`do while` 循环在检测条件之前就会执行，也就是说，即使条件为 false，`do while` 也会执行一次循环代码。而 `while` 循环只有在条件为真的时候才执行。
可以这样简单记忆：`while` 循环，先判断再执行；`do while` 循环先执行一次再判断。

## 函数（参数，函数返回值）

函数是被设计为执行特定任务的代码块，可重复调用执行。我们平时写代码的时候，也会经常遇到重复使用的代码，那我们是重复的一行行敲，或者 cv（`ctrl c + ctrl v`）大法一顿操作呢？显然这样有点不科学，如果我们使用函数将这些重复的代码封装起来，那么在需要使用的时候，就可以直接调用了。

#### 浏览器内置函数

其实在前面学习字符串和数组的时候，我们不知不觉已经使用了很多次函数。比如我们通过 `join()` 方法将数组转换为字符串，通过 `split()` 方法，将字符串转换为数组。这些都是浏览器已经封装好的内置函数，而我们可以直接调用。严格来说，内置浏览器函数并不是函数，它们是方法，但是没关系，我们前期学习阶段可以暂时不管这些，函数和方法在很大程度上是可互换的。

### 创建函数

#### 函数声明创建函数

语法为：

```javascript
function functionName(parameters) {
  // 执行的代码
}
```

例子：

```javascript
function f(a, b) {
  console.log(a + b);
} // 创建一个名为 f 的函数，它有两个形参 a，b

f(2, 3); // 调用函数 f，传入实参 2 和 3，最终运行结果为在控制台上打印出 5
```

#### 函数表达式创建函数

JavaScript 函数可以通过一个表达式定义。函数表达式可以存储在变量中。

语法为：

```javascript
var functionName = function (parameters) {
  // 执行的代码
};
```

把函数声明创建函数的例子改写为用函数表达式创建函数：

```javascript
var f = function (a, b) {
  console.log(a + b);
};
f(2, 3);
```

#### 函数声明和函数表达式的区别

- 函数声明

```javascript
// 此处的代码执行没有问题，JavaScript 解析器首先会把当前作用域的函数声明提前到整个作用域的最前面。
f(2, 3);

function f(a, b) {
  console.log(a + b);
}
```

- 函数表达式

```javascript
// 报错：f is not a function
// 这是因为函数表达式，如同定义其它基本类型的变量一样，只在执行到某一句时也会对其进行解析

f(2, 3);

var f = function (a, b) {
  console.log(a + b);
};
```

#### 函数的参数

- 形参：`function f(a, b){return a + b;} // a, b 是形参`，占位用，函数定义时形参无值。
- 实参：当我们调用上面的函数时比如 `f(2, 3);`其中 2 和 3 就是实参，会传递给 a 和 b，最后函数中执行的语句就变成了：`return 2 + 3;`。

注：在 JavaScript 中，实参个数和形参个数可以不相等。

#### 在 JavaScript 中没有重载

```javascript
function f(a, b) {
  return a + b;
}
function f(a, b, c) {
  return a + b + c;
}
var result = f(5, 6);
result; // returns NaN
```

上述代码中三个参数的 f 把两个参数的 f 覆盖，调用的是三个参数的 f，最后执行结果为 NaN，而不是 11。

#### 在 JavaScript 中函数的返回值

- 如果函数中没有 `return` 语句，那么函数默认的返回值是：undefined。
- 如果函数中有 `return` 语句，那么跟着 `return` 后面的值就是函数的返回值。
- 如果函数中有 `return` 语句，但是 `return` 后面没有任何值，那么函数的返回值也是：undefined。
- 函数在执行 `return` 语句后会停止并立即退出，也就是说 `return` 语句执行之后，剩下的代码都不会再执行了。
- 当函数外部需要使用函数内部的值的时候，我们不能直接给予，需要通过 `return` 返回。比如：

```javascript
var f = function (a, b) {
  a + b;
};
console.log(f(2, 3)); // 结果为 undefined
```

```javascript
var f = function (a, b) {
  return a + b;
};
console.log(f(2, 3)); // 结果为 5
```

#### 匿名函数

匿名函数就是没有命名的函数，一般用在绑定事件的时候。语法为：

```javascript
function(){
    // 执行的代码
}
```

例子：

```javascript
var myButton = document.querySelector("button");

myButton.onclick = function () {
  alert("hello");
};
```

注：将匿名函数分配为变量的值，也就是我们前面所讲的函数表达式创建函数。一般来说，创建功能，我们使用函数声明来创建函数。使用匿名函数来运行负载的代码以响应事件触发（如点击按钮），使用事件处理程序。

#### 自调用函数

匿名函数不能通过直接调用来执行，因此可以通过匿名函数的自调用的方式来执行。比如：

```javascript
(function () {
  alert("hello");
})();
```

## 挑战：制作直角三角形

任务要求：自定义一个函数，提示用户输入一个正整数。如果用户输入的非正整数，提示用户输入错误，并返回输入界面让用户输入。直到用户输入一个正确的正整数后，在页面打印出一个由 \* 组成的直角三角形。第一行打印一个 \* ，第二行打印两个 \* ，以此类推下去，最后一行的 \* 个数为用户输入的正整数。比如我们输入 9，最后打效果如下：

![1](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547173883560.png/wm)

参考源码：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title></title>
  </head>

  <body>
    <script>
      function star(i) {
        for (var j = 1; j <= i; j++) {
          for (var k = 1; k <= j; k++) {
            document.write("*");
          }
          document.write("<br>");
        }
      }
      do {
        var n = prompt("请输入一个正整数");
        if (Number(n) > 0 && parseInt(n) == parseFloat(n)) {
          star(n);
        } else {
          alert("输入错误，请输入一个正整数");
        }
      } while (!(Number(n) > 0 && parseInt(n) == parseFloat(n)));
    </script>
  </body>
</html>
```

## 总结

在本节中，我们继续学习了 JavaScript 的基础和一些关键特性，内容如下：

- 条件语句
- 循环语句
- 函数

与大多数语言相同，JavaScript 的构成也少不了这些语句。熟练掌握一二节，对后续的 JavaScript 学习会有很大的帮助。
