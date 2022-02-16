---
show: step
version: 1.0
enable_checker: true
---

# 通过例子学 Rust

## 课程介绍

[Rust](http://www.rust-lang.org/) 是一门注重安全（safety）、速度（speed）和并发（concurrency）的现代系统编程语言。Rust 通过内存安全来实现以上目标，但不用垃圾回收机制（garbage collection, GC)。

《通过例子学 Rust》（Rust By Example, RBE）内容由一系列可运行的实例组成，通过这些例子阐明了各种 Rust 的概念和基本库。想获取这些例子外的更多内容，可以 [安装 Rust 到本地](https://www.rust-lang.org/tools/install) 并查阅 [官方标准库文档](http://doc.rust-lang.org/std/)。

本课程为《通过例子学 Rust》的在线实验版本，通过在线实验一系列程序例子，一步步完成 Rust 编程语言的入门。欢迎在课程仓库中参与修订和完善，仓库地址见 [通过例子学 Rust - 在线实验版](https://github.com/shiyanlou/rust-by-example-cn)。所有文档内容版权跟随中文及英文原文档的版权（版权为 MIT 协议 或 Apache 协议）。

实验内容基于 [Rust By Example 中文版](https://github.com/rust-lang-cn/rust-by-example-cn) 进行制作和改编。感谢英文原版以及中文版本所有贡献者，让我们能够通过该教程快速入门 Rust 编程语言。

原中文翻译版本内容见 `src` 目录，按照 [**Rust 文档翻译指引**](https://github.com/rust-lang-cn/rust-translation-guide) 规范进行翻译。首次于 2016-08-07 翻译完全部内容，最后更新时间 2019-05-03。如果希望参与翻译工作，可以关注项目 [rust-by-example-cn](https://github.com/rust-lang-cn/rust-by-example-cn)。英文原版仓库见 [rust-by-example](https://github.com/rust-lang/rust-by-example)。

#### 授权协议

《通过例子学 Rust》（中文版与英文版 Rust By Example 均）使用以下两种协议的任一种进行授权：

- Apache 2.0 授权协议，（[LICENSE-APACHE](LICENSE-APACHE) 或 <http://www.apache.org/licenses/LICENSE-2.0>）
- MIT 授权协议 ([LICENSE-MIT](LICENSE-MIT) 或 <http://opensource.org/licenses/MIT>)

可以根据自己选择来定。

除非您有另外说明，否则您在本仓库提交的任何贡献均按上述方式进行双重许可授权，就如 Apache 2.0 协议所规定那样，而无需附加任何其他条款或条件。

#### 知识点

本节实验的主要内容包括以下知识点：

- 课程介绍
- 如何编写第一个程序
- Hello World 程序详解
- 注释
- 格式化输出

### 实验列表

课程实验根据原版教程内容进行规划，每个实验对应原版教程中的一个或多个章节：

- Hello World - 从经典的 "Hello World" 程序开始学习。
- 原生类型 - 学习有符号整型，无符号整型和其他原生类型。
- 自定义类型 - 结构体 `struct` 和 枚举 `enum`。
- 变量绑定 - 变量绑定，作用域，变量遮蔽。
- 类型系统 - 学习改变和定义类型。
- 类型转换
- 表达式
- 流程控制 - `if`/`else`，`for`，以及其他流程控制有关内容。
- 函数 - 学习方法、闭包和高阶函数。
- 模块 - 使用模块来组织代码。
- `crate` - crate 是 Rust 中的编译单元。学习创建一个库。
- cargo - 学习官方的 Rust 包管理工具的一些基本功能。
- 属性 - 属性是应用于某些模块、crate 或项的元数据（metadata）。
- 泛型 - 学习编写能够适用于多类型参数的函数或数据类型。
- 作用域规则 - 作用域在所有权（ownership）、借用（borrowing）和生命周期（lifetime）中起着重要作用。
- 特性 trait - trait 是对未知类型(`Self`)定义的方法集。
- 宏
- 错误处理 - 学习 Rust 语言处理失败的方式。
- 标准库类型 - 学习 `std` 标准库提供的一些自定义类型。
- 标准库更多介绍 - 更多关于文件处理、线程的自定义类型。
- 测试 - Rust 语言的各种测试手段。
- 不安全操作，兼容性和补充内容

现在我们开始进入到第一个 Hello World 实验吧！

## Hello World

在实验开始之前，我们首先需要安装 rust 开发环境，

```bash
sudo apt install rustc -y
```
![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20220216-1644973783064)

安装完成后可以通过 `rustc --version` 验证。

![图片描述](https://dn-simplecloud.shiyanlou.com/questions/uid810810-20220216-1644973867988)

我们的第一个程序将打印传说中的 "Hello World" 消息，下面是完整的程序代码和编译运行过程。

这是传统的 Hello World 程序的源码。首先，在实验楼 WebIDE 中 `/home/project` 目录下新建 `hello.rs` 文件，编写以下代码（以 `//` 开头的注释内容可以不必输入）：

```rust
// 这是注释内容，将会被编译器忽略掉
// 可以单击那边的按钮 "Run" 来测试这段代码 ->
// 若想用键盘操作，可以使用快捷键 "Ctrl + Enter" 来运行

// 这段代码支持编辑，你可以自由地修改代码！
// 通过单击 "Reset" 按钮可以使代码恢复到初始状态 ->

// 这是主函数
fn main() {
    // 调用编译生成的可执行文件时，这里的语句将被运行。

    // 将文本打印到控制台
    println!("Hello World!");
}
```

`println!` 是一个 **宏**（macros），可以将文本输出到控制台（console）。

在实验楼 WebIDE 终端中执行以下命令，使用 Rust 的编译器 `rustc` 从源程序生成可执行文件：

```bash
cd /home/project
rustc hello.rs
```

使用 `rustc` 编译后将得到可执行文件 `hello`，使用以下命令来运行生成的文件 `hello`：

```bash
./hello
```

执行后的结果如下所示：

![hello 执行结果](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383086/wm)

#### 动手试一试

```checker
- name: check if file exist
  script: |
    #!/bin/bash
    ls /home/project/hello.rs
  error: 未创建 hello.rs 文件！
  timeout: 2

- name: check file content
  script: |
    #!/bin/bash
    grep "I'm a Rustacean!" /home/project/hello.rs
  error: 请尝试完成「动手试一试」内容！
  timeout: 2
```

请尝试下在你的 `hello.rs` 程序中增加一行代码，再一次使用宏 `println!`，得到下面结果：

```text
Hello World!
I'm a Rustacean!
```

## 注释

注释对任何程序都不可缺少，同样 Rust 支持几种不同的注释方式。

- **普通注释**，其内容将被编译器忽略掉：

* `// 单行注释，注释内容直到行尾。`
* `/* 块注释， 注释内容一直到结束分隔符。 */`

- **文档注释**，其内容将被解析成 HTML 帮助文档:

* `/// 为接下来的项生成帮助文档。`
* `//! 为注释所属于的项（译注：如 crate、模块或函数）生成帮助文档。`

```rust
fn main() {
    // 这是行注释的例子
    // 注意有两个斜线在本行的开头
    // 在这里面的所有内容都不会被编译器读取

    // println!("Hello, world!");

    // 请运行一下，你看到结果了吗？现在请将上述语句的两条斜线删掉，并重新运行。

    /*
     * 这是另外一种注释——块注释。一般而言，行注释是推荐的注释格式，
     * 不过块注释在临时注释大块代码特别有用。/* 块注释可以 /* 嵌套, */ */
     * 所以只需很少按键就可注释掉这些 main() 函数中的行。/*/*/* 自己试试！*/*/*/
     */

     /*
      注意，上面的例子中纵向都有 `*`，这只是一种风格，实际上这并不是必须的。
      */

     // 观察块注释是如何简单地对表达式进行修改的，行注释则不能这样。
     // 删除注释分隔符将会改变结果。
     let x = 5 + /* 90 + */ 5;
     println!("Is `x` 10 or 100? x = {}", x);
}
```

## 格式化输出

打印操作由 `std::fmt` 里面所定义的一系列 `宏` 来处理，包括：

- `format!`：将格式化文本写到 `字符串`（String）。

  > 译注：`字符串` 是返回值不是参数。

- `print!`：与 `format!` 类似，但将文本输出到控制台（`io::stdout`）。

- `println!`: 与 `print!` 类似，但输出结果追加一个换行符。

- `eprint!`：与 `format!` 类似，但将文本输出到标准错误（`io::stderr`）。

- `eprintln!`：与 `eprint!` 类似，但输出结果追加一个换行符。

这些宏都以相同的做法解析（parse）文本。另外有个优点是格式化的正确性会在编译时检查。

新建 `format.rs` 文件，编写代码如下。

```rust
fn main() {
    // 通常情况下，`{}` 会被任意变量内容所替换。
    // 变量内容会转化成字符串。
    println!("{} days", 31);

    // 不加后缀的话，31 就自动成为 i32 类型。
    // 你可以添加后缀来改变 31 的类型。

    // 用变量替换字符串有多种写法。
    // 比如可以使用位置参数。
    println!("{0}, this is {1}. {1}, this is {0}", "Alice", "Bob");

    // 可以使用命名参数。
    println!("{subject} {verb} {object}",
             object="the lazy dog",
             subject="the quick brown fox",
             verb="jumps over");

    // 可以在 `:` 后面指定特殊的格式。
    println!("{} of {:b} people know binary, the other half don't", 1, 2);

    // 你可以按指定宽度来右对齐文本。
    // 下面语句输出 "     1"，5 个空格后面连着 1。
    println!("{number:>width$}", number=1, width=6);

    // 你可以在数字左边补 0。下面语句输出 "000001"。
    println!("{number:>0width$}", number=1, width=6);

    // println! 会检查使用到的参数数量是否正确。
    println!("My name is {0}, {1} {0}", "Bond");
    // 改正 ^ 补上漏掉的参数："James"

    // 创建一个包含单个 `i32` 的结构体（structure）。命名为 `Structure`。
    #[allow(dead_code)]
    struct Structure(i32);

    // 但是像结构体这样的自定义类型需要更复杂的方式来处理。
    // 下面语句无法运行。
    println!("This struct `{}` won't print...", Structure(3));
    // 改正 ^ 注释掉此行。
}
```

`std::fmt` 包含多种 `traits`（trait 有「特征，特性」等意思）来控制文字显示，其中重要的两种 trait 的基本形式如下：

- `fmt::Debug`：使用 `{:?}` 标记。格式化文本以供调试使用。
- `fmt::Display`：使用 `{}` 标记。以更优雅和友好的风格来格式化文本。

上例使用了 `fmt::Display`，因为标准库提供了那些类型的实现。若要打印自定义类型的文本，需要更多的步骤。

#### 动手试一试

```checker
- name: check if file exist
  script: |
    #!/bin/bash
    ls /home/project/format.rs
  error: 未创建 format.rs 文件！
  timeout: 2
```

- 改正上面代码中的两个错误（见代码注释中的「改正」），使它可以没有错误地运行。

- 再用一个 `println!` 宏，通过控制显示的小数位数来打印：`Pi is roughly 3.142`
  （Pi 约等于 3.142）。为了达到练习目的，使用 `let pi = 3.141592` 作为 Pi 的近似
  值。

> 提示：设置小数位的显示格式可以参考文档 [std::fmt](https://doc.rust-lang.org/std/fmt)。

### 调试（Debug）

所有的类型，若想用 `std::fmt` 的格式化 `trait` 打印出来，都要求实现这个 `trait`。自动的实现只为一些类型提供，比如 `std` 库中的类型。所有其他类型都**必须**手动实现。

`fmt::Debug` 这个 `trait` 使这项工作变得相当简单。所有类型都能推导（`derive`，即自
动创建）`fmt::Debug` 的实现。但是 `fmt::Display` 需要手动实现。

```rust
// 这个结构体不能使用 `fmt::Display` 或 `fmt::Debug` 来进行打印。
struct UnPrintable(i32);

// `derive` 属性会自动创建所需的实现，使这个 `struct` 能使用 `fmt::Debug` 打印。
#[derive(Debug)]
struct DebugPrintable(i32);
```

所有 `std` 库类型都天生可以使用 `{:?}` 来打印。新建 `format1.rs` 文件，编写代码如下：

```rust
// 推导 `Structure` 的 `fmt::Debug` 实现。
// `Structure` 是一个包含单个 `i32` 的结构体。
#[derive(Debug)]
struct Structure(i32);

// 将 `Structure` 放到结构体 `Deep` 中。然后使 `Deep` 也能够打印。
#[derive(Debug)]
struct Deep(Structure);

fn main() {
    // 使用 `{:?}` 打印和使用 `{}` 类似。
    println!("{:?} months in a year.", 12);
    println!("{1:?} {0:?} is the {actor:?} name.",
             "Slater",
             "Christian",
             actor="actor's");

    // `Structure` 也可以打印！
    println!("Now {:?} will print!", Structure(3));

    // 使用 `derive` 的一个问题是不能控制输出的形式。
    // 假如我只想展示一个 `7` 怎么办？
    println!("Now {:?} will print!", Deep(Structure(7)));
}
```

在实验楼 WebIDE 终端中编译并运行程序。

```bash
$ rustc format1.rs
$ ./format1
```

执行结果如下所示：

![format1 执行结果](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383088/wm)

所以 `fmt::Debug` 确实使这些内容可以打印，但是牺牲了一些美感。Rust 也通过 `{:#?}` 提供了「美化打印」的功能：

```rust
#[derive(Debug)]
struct Person<'a> {
    name: &'a str,
    age: u8
}

fn main() {
    let name = "Peter";
    let age = 27;
    let peter = Person { name, age };

    // 美化打印
    println!("{:#?}", peter);
}
```

将上面代码添加到 `format1.rs` 中后，编译并运行的结果如下所示：

![format1 执行结果 2](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383089/wm)

你可以通过手动实现 `fmt::Display` 来控制显示效果。

### 显示（Display）

`fmt::Debug` 通常看起来不太简洁，因此自定义输出的外观经常是更可取的。这需要通过手动实现 `fmt::Display` 来做到。`fmt::Display` 采用 `{}` 标记。实现方式看起来像这样：

```rust
// （使用 `use`）导入 `fmt` 模块使 `fmt::Display` 可用
use std::fmt;

// 定义一个结构体，咱们会为它实现 `fmt::Display`。以下是个简单的元组结构体
// `Structure`，包含一个 `i32` 元素。
struct Structure(i32);

// 为了使用 `{}` 标记，必须手动为类型实现 `fmt::Display` trait。
impl fmt::Display for Structure {
    // 这个 trait 要求 `fmt` 使用与下面的函数完全一致的函数签名
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // 仅将 self 的第一个元素写入到给定的输出流 `f`。返回 `fmt:Result`，此
        // 结果表明操作成功或失败。注意 `write!` 的用法和 `println!` 很相似。
        write!(f, "{}", self.0)
    }
}
```

`fmt::Display` 的效果可能比 `fmt::Debug` 简洁，但对于 `std` 库来说，这就有一个问题。模棱两可的类型该如何显示呢？举个例子，假设标准库对所有的 `Vec<T>` 都实现了同一种输出样式，那么它应该是哪种样式？下面两种中的一种吗？

- `Vec<path>`：`/:/etc:/home/username:/bin`（使用 `:` 分割）
- `Vec<number>`：`1,2,3`（使用 `,` 分割）

我们没有这样做，因为没有一种合适的样式适用于所有类型，标准库也并不擅自规定一种样式。对于 `Vec<T>` 或其他任意*泛型容器*（generic container），`fmt::Display` 都没有实现。因此在这些泛型的情况下要用 `fmt::Debug`。

这并不是一个问题，因为对于任何**非**泛型的**容器**类型， `fmt::Display` 都能够实现。新建 `display.rs` 文件，编写代码如下：

```rust
use std::fmt; // （使用 `use`）导入 `fmt` 模块使 `fmt::Display` 可用

// 带有两个数字的结构体。推导出 `Debug`，以便与 `Display` 的输出进行比较。
#[derive(Debug)]
struct MinMax(i64, i64);

// 实现 `MinMax` 的 `Display`。
impl fmt::Display for MinMax {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // 使用 `self.number` 来表示各个数据。
        write!(f, "({}, {})", self.0, self.1)
    }
}

// 为了比较，定义一个含有具名字段的结构体。
#[derive(Debug)]
struct Point2D {
    x: f64,
    y: f64,
}

// 类似地对 `Point2D` 实现 `Display`
impl fmt::Display for Point2D {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // 自定义格式，使得仅显示 `x` 和 `y` 的值。
        write!(f, "x: {}, y: {}", self.x, self.y)
    }
}

fn main() {
    let minmax = MinMax(0, 14);

    println!("Compare structures:");
    println!("Display: {}", minmax);
    println!("Debug: {:?}", minmax);

    let big_range =   MinMax(-300, 300);
    let small_range = MinMax(-3, 3);

    println!("The big range is {big} and the small is {small}",
             small = small_range,
             big = big_range);

    let point = Point2D { x: 3.3, y: 7.2 };

    println!("Compare points:");
    println!("Display: {}", point);
    println!("Debug: {:?}", point);

    // 报错。`Debug` 和 `Display` 都被实现了，但 `{:b}` 需要 `fmt::Binary`
    // 得到实现。这语句不能运行。
    // println!("What does Point2D look like in binary: {:b}?", point);
}
```

程序运行的结果如下所示：

![display 执行结果](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383091/wm)

`fmt::Display` 被实现了，而 `fmt::Binary` 没有，因此 `fmt::Binary` 不能使用。
`std::fmt` 有很多这样的 `trait`，它们都要求有各自的实现。这些内容将在
后面的 `std::fmt` 章节中详细介绍。

#### 动手试一试

```checker
- name: check if file exist
  script: |
    #!/bin/bash
    ls /home/project/display.rs
  error: 未创建 display.rs 文件！
  timeout: 2
```

检验上面例子的输出，然后在示例程序中，仿照 `Point2D` 结构体增加一个复数结构体。使用一样的方式打印，输出结果要求是这个样子：

```txt
Display: 3.3 + 7.2i
Debug: Complex { real: 3.3, imag: 7.2 }
```

### 测试实例：List

对一个结构体实现 `fmt::Display`，其中的元素需要一个接一个地处理到，这可能会很麻烦。问题在于每个 `write!` 都要生成一个 `fmt::Result`。正确的实现需要处理**所有**的 Result。Rust 专门为解决这个问题提供了 `?` 操作符。

在 `write!` 上使用 `?` 会像是这样：

```rust
// 对 `write!` 进行尝试（try），观察是否出错。若发生错误，返回相应的错误。
// 否则（没有出错）继续执行后面的语句。
write!(f, "{}", value)?;
```

另外，你也可以使用 `try!` 宏，它和 `?` 是一样的。这种写法比较罗嗦，故不再推荐，
但在老一些的 Rust 代码中仍会看到。使用 `try!` 看起来像这样：

```rust
try!(write!(f, "{}", value));
```

有了 `?`，对一个 `Vec` 实现 `fmt::Display` 就很简单了。新建 `vector.rs` 文件，编写代码如下：

```rust
use std::fmt; // 导入 `fmt` 模块。

// 定义一个包含单个 `Vec` 的结构体 `List`。
struct List(Vec<i32>);

impl fmt::Display for List {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // 使用元组的下标获取值，并创建一个 `vec` 的引用。
        let vec = &self.0;

        write!(f, "[")?;

        // 使用 `v` 对 `vec` 进行迭代，并用 `count` 记录迭代次数。
        for (count, v) in vec.iter().enumerate() {
            // 对每个元素（第一个元素除外）加上逗号。
            // 使用 `?` 或 `try!` 来返回错误。
            if count != 0 { write!(f, ", ")?; }
            write!(f, "{}", v)?;
        }

        // 加上配对中括号，并返回一个 fmt::Result 值。
        write!(f, "]")
    }
}

fn main() {
    let v = List(vec![1, 2, 3]);
    println!("{}", v);
}
```

程序运行的结果如下：

![vector 执行结果](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383093/wm)

#### 动手试一试

```checker
- name: check if file exist
  script: |
    #!/bin/bash
    ls /home/project/vector.rs
  error: 未创建 vector.rs 文件！
  timeout: 2
```

更改程序使 vector 里面每个元素的下标也能够打印出来。新的结果如下：

```rust
[0: 1, 1: 2, 2: 3]
```

### 格式化

我们已经看到，格式化的方式是通过**格式字符串**来指定的：

- `format!("{}", foo)` -> `"3735928559"`
- `format!("0x{:X}", foo)` ->
  `"0xDEADBEEF"`
- `format!("0o{:o}", foo)` -> `"0o33653337357"`

根据使用的**参数类型**是 `X`、`o` 还是**未指定**，同样的变量（`foo`）能够格式化成不同的形式。

这个格式化的功能是通过 trait 实现的，每种参数类型都对应一种 trait。最常见的格式化 trait 就是 `Display`，它可以处理参数类型为未指定的情况，比如 `{}`。

新建 `format2.rs` 文件，编写代码如下：

```rust
use std::fmt::{self, Formatter, Display};

struct City {
    name: &'static str,
    // 纬度
    lat: f32,
    // 经度
    lon: f32,
}

impl Display for City {
    // `f` 是一个缓冲区（buffer），此方法必须将格式化后的字符串写入其中
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        let lat_c = if self.lat >= 0.0 { 'N' } else { 'S' };
        let lon_c = if self.lon >= 0.0 { 'E' } else { 'W' };

        // `write!` 和 `format!` 类似，但它会将格式化后的字符串写入
        // 一个缓冲区（即第一个参数f）中。
        write!(f, "{}: {:.3}°{} {:.3}°{}",
               self.name, self.lat.abs(), lat_c, self.lon.abs(), lon_c)
    }
}

#[derive(Debug)]
struct Color {
    red: u8,
    green: u8,
    blue: u8,
}

fn main() {
    for city in [
        City { name: "Dublin", lat: 53.347778, lon: -6.259722 },
        City { name: "Oslo", lat: 59.95, lon: 10.75 },
        City { name: "Vancouver", lat: 49.25, lon: -123.1 },
    ].iter() {
        println!("{}", *city);
    }
    for color in [
        Color { red: 128, green: 255, blue: 90 },
        Color { red: 0, green: 3, blue: 254 },
        Color { red: 0, green: 0, blue: 0 },
    ].iter() {
        // 在添加了针对 fmt::Display 的实现后，请改用 {} 检验效果。
        println!("{:?}", *color)
    }
}
```

程序运行的结果如下：

![format2 执行结果](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383094/wm)

在 `fmt::fmt` 文档中可以查看格式化 traits 一览表和它们的参数类型。

#### 动手试一试

```checker
- name: check if file exist
  script: |
    #!/bin/bash
    ls /home/project/format2.rs
  error: 未创建 format2.rs 文件！
  timeout: 2
```

为上面的 `Color` 结构体实现 `fmt::Display`，应得到如下的输出结果：

```text
RGB (128, 255, 90) 0x80FF5A
RGB (0, 3, 254) 0x0003FE
RGB (0, 0, 0) 0x000000
```

如果感到疑惑，可看下面两条提示：

- [你可能需要多次列出每个颜色](http://doc.rust-lang.org/std/fmt/#argument-types)，
- 你可以使用 `:02` [补零使位数为 2 位](http://doc.rust-lang.org/std/fmt/#width)。

## 实验总结

本节实验中我们学习了以下的内容：

- 课程介绍
- 如何编写第一个程序
- Hello World 程序详解
- 注释
- 格式化输出

请务必按照实验步骤将示例代码在实验环境中完整输入一遍，并完成动手练习题目。只有真正动手去做才会有最大的收获，遇到问题欢迎在 [实验楼讨论区](https://www.shiyanlou.com/questions/) 中发帖与同学讨论交流。
