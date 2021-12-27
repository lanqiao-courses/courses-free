---
show: step
version: 1.0
enable_checker: true
---

# cargo

## 实验介绍

`cargo` 是官方的 Rust 包管理工具。 它有很多非常有用的功能来提高代码质量和开发人员的开发效率！ 这些功能包括：

- 依赖管理和与 [crates.io](https://crates.io)（官方 Rust 包注册服务）集成
- 方便的单元测试
- 方便的基准测试

本章将介绍一些快速入门的基础知识，不过你可以在 [cargo 官方手册](https://doc.rust-lang.org/cargo/)中找到详细内容。

#### 知识点

本节实验的主要内容包括以下 Cargo 相关的知识点：

- 依赖
- 约定规范
- 测试
- 构建脚本

## 依赖

大多数程序都会依赖于某些库。如果你曾经手动管理过库依赖，那么你就知道这会带来的极大的痛苦。幸运的是，Rust 的生态链标配 `cargo` 工具！`cargo` 可以管理项目的依赖关系。

下面创建一个新的 Rust 项目：

```sh
# 二进制可执行文件
cargo new foo

# 或者库
cargo new --lib foo
```

如果在执行 `cargo new foo` 时出现以下错误：

![错误提示](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383069/wm)

只需跟随提示设置 `USER` 即可，在实验楼 WebIDE 终端中执行以下命令：

```bash
export USER=shiyanlou
```

设置成功后，由于之前的错误造成 `foo` 文件夹已被创建，再次使用命令时会出现以下错误：

![错误提示 2](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383071/wm)

解决方法一是删除 `foo` 文件夹，从新使用 `cargo new foo` 创建项目；二是跟随提示使用 `cargo init foo` 在 `foo` 文件夹下初始化项目。

对于本章的其余部分，我们选定创建的都是二进制可执行文件而不是库，但所有的概念都是相同的。

完成上述命令后，将看到如下内容：

![项目结构](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383072/wm)

`main.rs` 是新项目的入口源文件——这里没什么新东西。 `Cargo.toml` 是本项目（`foo`）的 `cargo` 的配置文件。 浏览 `Cargo.toml` 文件，将看到类似以下的的内容：

```toml
[package]
name = "foo"
version = "0.1.0"
authors = ["mark"]

[dependencies]
```

`package` 下面的 `name` 字段表明项目的名称。 如果您发布 crate（后面将做更多介绍），那么 `crates.io` 将使用此字段标明的名称。 这也是编译时输出的二进制可执行文件的名称。

- `version` 字段是使用[语义版本控制](http://semver.org/)（Semantic Versioning）的 crate 版本号。

- `authors` 字段表明发布 crate 时的作者列表。

- `dependencies` 这部分可以让你为项目添加依赖。

举个例子，假设我们希望程序有一个很棒的命令行界面（command-line interface，CLI））。 你可以在 [crates.io](https://crates.io)（官方的 Rust 包注册服务）上找到很多很棒的 Rust 包。其中一个受欢迎的包是 [clap](https://crates.io/crates/clap)（译注：一个命令行参数的解析器）。在撰写本文时，[clap] 最新发布的版本为 `2.27.1`。要在程序中添加依赖，我们可以很简单地在 `Cargo.toml` 文件中的 `dependencies` 项后面将以下内容添加进来 ：`clap = "2.27.1"`。当然，在 `main.rs` 文件中写上 `extern crate clap`，就和平常一样。 就是这样！你就可以在程序中开始使用 `clap` 了。

`cargo` 还支持 [其他类型的依赖][dependencies]。下面是一个简单的示例：

```toml
[package]
name = "foo"
version = "0.1.0"
authors = ["mark"]

[dependencies]
clap = "2.27.1" # 来自 crates.io
rand = { git = "https://github.com/rust-lang-nursery/rand" } # 来自网上的仓库
bar = { path = "../bar" } # 来自本地文件系统的路径
```

`cargo` 不仅仅是一个包依赖管理器。`Cargo.toml` 的所有可用配置选项都列在 [格式规范][manifest]中。

要构建我们的项目，我们可以在项目目录中的任何位置（包括子目录！）执行 `cargo build`。我们也可以执行 `cargo run` 来构建和运行。请注意，这些命令将处理所有依赖，在需要时下载 crate，并构建所有内容，包括 crate。（请注意，它只重新构建尚未构建的内容，这和 `make` 类似）。

瞧！这里的所有都和 `cargo` 有关！

## 约定规范

在上一小节中，我们看到了以下目录层次结构：

```txt
foo
├── Cargo.toml
└── src
    └── main.rs
```

假设我们要在同一个项目中有两个二进制可执行文件。 那要怎样做呢？

很显然，`cargo` 支持这一点。正如我们之前看到的，默认二进制名称是 `main`，但可以通过将文件放在 `bin/` 目录中来添加其他二进制可执行文件：

```txt
foo
├── Cargo.toml
└── src
    ├── main.rs
    └── bin
        └── my_other_bin.rs
```

为了使得 `cargo` 编译或运行这个二进制可执行文件而不是默认或其他二进制可执行文件，我们只需给 `cargo` 增加一个参数 `--bin my_other_bin`，其中 `my_other_bin` 是我们想要使用的二进制可执行文件的名称。

除了可添加其他二进制可执行文件外，`cargo` 还支持更多功能（more features），如基准测试，测试和示例。

在下一节中，我们将更仔细地学习测试的内容。

## 测试

我们知道测试是任何软件不可缺少的一部分！Rust 对单元和集成测试提供一流的支持（参见《Rust 程序设计语言》中的关于 [测试的章节](https://doc.rust-lang.org/book/ch11-00-testing.html)）。

通过上面链接的关于测试章节，我们看到了如何编写单元测试和集成测试。在代码目录组织上，我们可以将单元测试放在需要测试的模块中，并将集成测试放在源码中 `tests/` 目录中：

```txt
foo
├── Cargo.toml
├── src
│   └── main.rs
└── tests
    ├── my_test.rs
    └── my_other_test.rs
```

`tests` 目录下的每个文件都是一个单独的集成测试。

`cargo` 很自然地提供了一种便捷的方法来运行所有测试！

```sh
cargo test
```

你将会看到像这样的输出：

```txt
$ cargo test
   Compiling blah v0.1.0 (file:///nobackup/blah)
    Finished dev [unoptimized + debuginfo] target(s) in 0.89 secs
     Running target/debug/deps/blah-d3b32b97275ec472

running 3 tests
test test_bar ... ok
test test_baz ... ok
test test_foo_bar ... ok
test test_foo ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

你还可以运行如下测试，其中名称匹配一个模式：

```sh
cargo test test_foo
```

```txt
$ cargo test test_foo
   Compiling blah v0.1.0 (file:///nobackup/blah)
    Finished dev [unoptimized + debuginfo] target(s) in 0.35 secs
     Running target/debug/deps/blah-d3b32b97275ec472

running 2 tests
test test_foo ... ok
test test_foo_bar ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out
```

需要注意的一点是：`cargo` 可能同时进行多项测试，因此请确保它们不会相互竞争。例如，如果它们都输出到文件，则应该将它们写入不同的文件。

## 构建脚本

有时使用 `cargo` 正常构建还是不够的。也许你的 crate 在 cargo 成功编译之前需要一些先决条件，比如代码生成或者需要编译的一些本地代码。为了解决这个问题，我们构建了 cargo 可以运行的脚本。

要向包中添加构建脚本，可以在 `Cargo.toml` 中指定它，如下所示：

```toml
[package]
...
build = "build.rs"
```

跟默认情况不同，这里 cargo 将在项目目录中优先查找 `build.rs` 文件。（本句采用意译，英文原文为：Otherwise Cargo will look for a `build.rs` file in the project directory by default.）

### 怎么使用构建脚本

构建脚本只是另一个 Rust 文件，此文件将在编译包中的任何其他内容之前，优先进行编译和调用。 因此，此文件可实现满足 crate 的先决条件。

cargo 通过 [此处指定](https://doc.rust-lang.org/cargo/reference/environment-variables.html#environment-variables-cargo-sets-for-build-scripts) 的可以使用的环境变量为脚本提供输入。（英文原文：Cargo provides the script with inputs via environment variables [specified
here](https://doc.rust-lang.org/cargo/reference/environment-variables.html#environment-variables-cargo-sets-for-build-scripts) that can be used.）

此脚本通过 stdout （标准输出）提供输出。打印的所有行都写入到 `target/debug/build/<pkg>/output`。另外，以 `cargo:` 为前缀的行将由 cargo 直接解析，因此可用于定义包编译的参数。

有关进一步的说明和示例，请阅读 [cargo 规定说明文档](https://doc.rust-lang.org/cargo/reference/build-scripts.html)。

## 实验总结

本节实验中我们学习了以下 Cargo 相关的内容：

- 依赖
- 约定规范
- 测试
- 构建脚本

请务必按照实验步骤将示例代码在实验环境中完整输入一遍，并完成动手练习题目。只有真正动手去做才会有最大的收获，遇到问题欢迎在 [实验楼讨论区](https://www.shiyanlou.com/questions/) 中发帖与同学讨论交流。
