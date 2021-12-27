---
show: step
version: 1.0
enable_checker: true
---

# crate

## 实验介绍

crate（中文有 “包，包装箱” 之意）是 Rust 的编译单元。当调用 `rustc some_file.rs` 时，`some_file.rs` 被当作 **crate 文件**。如果 `some_file.rs` 里面含有 `mod` 声明，那么模块文件的内容将在编译之前被插入 crate 文件的相应声明处。换句话说，模块**不会**单独被编译，只有 crate 才会被编译。

#### 知识点

本节实验的主要内容包括以下知识点：

- `crate` 概念
- 创建方式
- `extern crate`

## `crate` 概念

crate（中文有 “包，包装箱” 之意）是 Rust 的编译单元。当调用 `rustc some_file.rs` 时，`some_file.rs` 被当作 **crate 文件**。如果 `some_file.rs` 里面含有 `mod` 声明，那么模块文件的内容将在编译之前被插入 crate 文件的相应声明处。换句话说，模块**不会**单独被编译，只有 crate 才会被编译。

crate 可以编译成二进制可执行文件（binary）或库文件（library）。默认情况下，`rustc` 将从 crate 产生二进制可执行文件。这种行为可以通过 `rustc` 的选项 `--crate-type` 重载。

## 创建方式

让我们创建一个库，然后看看如何把它链接到另一个 crate。

新建 `rary.rs` 文件，编写代码如下：

```rust
pub fn public_function() {
    println!("called rary's `public_function()`");
}

fn private_function() {
    println!("called rary's `private_function()`");
}

pub fn indirect_access() {
    print!("called rary's `indirect_access()`, that\n> ");

    private_function();
}
```

在实验楼 WebIDE 终端中执行以下命令：

```bash
$ rustc --crate-type=lib rary.rs
```

执行的结果如下所示：

![执行结果](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383066/wm)

默认情况下，库会使用 crate 文件的名字，前面加上 “lib” 前缀，但这个默认名称可以使用 `crate_name` 属性覆盖。

## `extern crate`

要把上一节创建的库链接到一个 crate，必须使用 `extern crate` 声明。这不仅会链接库，还会用一个与库名相同的模块来存放库里面的所有项。于模块的可见性规则也适用于库。

```rust
// 链接到 `rary` 库，导入其中的项
extern crate rary;

fn main() {
    rary::public_function();

    // 报错！ `private_function` 是私有的
    //rary::private_function();

    rary::indirect_access();
}
```

在实验楼 WebIDE 终端中执行以下命令：

```bash
# library.rlib 是已编译好的库的路径，这里假设它在同一目录下：
$ rustc executable.rs --extern rary=./library.rlib && ./executable
```

执行的结果如下：

![执行结果](https://doc.shiyanlou.com/courses/uid1172186-20200107-1578383067/wm)

## 实验总结

本节实验中我们学习了以下的内容：

- `crate` 概念
- 创建方式
- `extern crate`

请务必按照实验步骤将示例代码在实验环境中完整输入一遍，并完成动手练习题目。只有真正动手去做才会有最大的收获，遇到问题欢迎在 [实验楼讨论区](https://www.shiyanlou.com/questions/) 中发帖与同学讨论交流。
