---
show: step
version: 1.0
enable_checker: true
---

# 初识 TypeScript

## 简介

TypeScript 是 JavaScript 的一个超集，他们两个之间有非常深入的联系，所以在学习 TypeScript 之前，你需要学习 JavaScript 相关教程。在本实验我们将会对 TypeScript 进行简单的介绍并初步使用它。

#### 知识点

- TypeScript 简介
- 为何选择 TypeScript
- 安装使用 TypeScript

## TypeScript 简介

#### 什么是 TypeScript

TypeScript 是一种由微软开发的自由和开源的编程语言。它是 JavaScript 的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。它扩展了 JavaScript 的语法，所以任何现有的 JavaScript 程序可以不加改变的在 TypeScript 下工作。TypeScript 是为大型应用之开发而设计，而编译时它产生 JavaScript 以确保兼容性。

#### TypeScript 与 JavaScript 的区别

- TypeScript 是 JavaScript 的超集，扩展了 JavaScript 的语法。
- TypeScript 可处理已有的 JavaScript 代码，并只对其中的 TypeScript 代码进行编译。
- TypeScript 文件的后缀名 `.ts` （`.ts`，`.tsx`，`.dts`），JavaScript 文件是 `.js`。
- 在编写 TypeScript 的文件的时候就会自动编译成 js 文件。

用一张表格来更清晰的观察两者的区别：

|               | JavaScript                                                     | TypeScript                                                                    |
| ------------- | -------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| 语言          | 脚本语言                                                       | 面向对象编程语言                                                              |
| 学习难度      | 灵活易学                                                       | 需要有脚本编程经验                                                            |
| 类型          | 轻量级解释编程语言                                             | 强类型的面向对象编程语言                                                      |
| 客户端/服务端 | 客户端服务端都有                                               | 侧重客户端                                                                    |
| 拓展名        | .js                                                            | .ts 或 .tsx                                                                   |
| 耗时          | 更快                                                           | 编译代码需要些时间                                                            |
| 数据绑定      | 没有类型和接口的概念                                           | 使用类型和接口表示数据                                                        |
| 语法          | 所有的语句都写在脚本标签内。浏览器将脚本标签内的文本识别为脚本 | 一个 TypeScript 程序由模块、方法、变量、语句、表达式和注释构成                |
| 静态类型      | JS 中没有静态类型的概念                                        | 支持静态类型                                                                  |
| 模块支持      | 不支持模块                                                     | 支持模块                                                                      |
| 接口          | 没有接口                                                       | 支持接口                                                                      |
| 可选参数方法  | 不支持                                                         | 支持                                                                          |
| 原型          | 没有这种特性                                                   | 支持原型特性                                                                  |
| 参考选择      | 小型项目                                                       | TS 是一种面向对象语言，代码更简洁，可读性和复用性强。因此 TS 更适合大型项目。 |

## 为何选择 TypeScript

- TypeScript 增加了代码的可读性和可维护性。
- 新增了其他语言的语法，比如 Class（类）、Interface（接口）、Generics(泛型)、Enums(枚举）等。
- TypeScript 拥抱了 ES6 规范。
- 兼容很多第三方库，即使第三方库不是用 TypeScript 写的，也可以编写单独的类型文件供 TypeScript 读取。
- TypeScript 拥有活跃的社区

更值得一提的是，TypeScript 在开发时就能给出编译错误，而 JavaScript 错误则需要在运行时才能暴露。作为强类型语言，你可以明确知道数据的类型，代码可读性极强，几乎每个人都能理解。TypeScript 被很多业界大佬使用，像 Asana、Circle CL 和 Slack 这些公司都在用 TypeScript。

## 安装使用 TypeScript

打开终端 terminal 输入全局安装命令：

```bash
cnpm install -g typescript
```

新建一个文件`index.ts`，输入以下内容：

```ts
console.log('hello world');
var a: string = '2'; //这是ts写法，暂时不需要掌握，后续会讲到
```

在终端输入`tsc index.ts`编译文件，编译成功则会生成一个同名的 js 文件。

![](https://doc.shiyanlou.com/courses/700/1226977/7461b048cd6f2ea017e1ce050782170c-0/wm)

生成的 js 文件里则将 ts 语法转换成了 js 语法。

## 总结

本小节我们学习了以下知识点：

- TypeScript 简介
- 为何选择 TypeScript
- 安装使用 TypeScript

我相信你已经对 TypeScripty 有了一个初步了解，接下来我们将会对 TypeScript 进行进一步学习。