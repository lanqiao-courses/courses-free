---
show: step
version: 1.0
enable_checker: true
---

# Python 文本解释器

## 实验介绍

本节实验我们学习使用 Python 来解析纯文本文件，并生成 HTML 页面。本节实验只是一个简单实现，支持了较少部分的 Markdown 语法，但通过实验中的解析实现思路，同学们可以完成更多更复杂的语法解析实现。

#### 知识点

- Python 基本语法
- Python 面向对象编程
- yield 关键字
- 正则表达式
- HTML 标记语言

## 实验步骤

一共有文本块生成器、处理程序、规则、解析和运行与测试五个步骤，本课程中将创建以下的代码文件，每个文件的作用简介如下：

- `util.py`：实现文本块生成器把纯文本分成一个一个的文本块，以便接下来对每一个文本块进行解析
- `handlers.py`：为文本块打上合适的 HTML 标记
- `rules.py`：设计一定的规则来判断每个文本块交给处理程序将要加什么标记
- `markup.py`：对整个文本进行解析的程序

### 文本生成块

首先我们需要有一个文本块生成器，来把纯文本分成一个一个的文本块，以便接下来对每一个文本块进行解析，将以下代码写入 `util.py` 文件中：

```py
# -*- coding: UTF-8 -*-
'''处理 TXT 文本，创建返回文本块的生成器
'''

# 生成器函数，函数调用为生成器
# 调用此函数时，file 参数一定是 IOWrapper 对象，IOWrapper 对象是迭代器对象
# 该函数的作用是给文本文件的 IOWrapper 迭代器的末尾增加一个换行符
def lines(file):
    """生成器，在文本最后加一空行
    """
    for line in file:
        yield line
    yield '\n'

# 同上一个函数 lines ，它也是个生成器函数
# 调用此函数时，file 参数一定是 IOWrapper 对象，IOWrapper 对象是迭代器对象
# 函数的返回值是生成器，生成器的每次迭代都会返回一个文本块
def blocks(file):
    """生成器，将 TXT 文件内容生成一个个单独的文本块，按空行分
    """
    block = []
    # 使用 for 循环调用生成器
    for line in lines(file):
        # 如果不是空行
        if line.strip():
            # 将该行数据添加到 block 列表里
            block.append(line)
        # 如果是空行，且 block 列表里有内容
        # 这里可以看出 lines 生成器函数的作用了，如果最后一行不是空行
        # 那么最后一个文本块就不会作为 yield 的参数被生成器返回
        elif block:
            yield ''.join(block).strip()
            # 每次生成文本块后，要清空 block 列表
            block = []
```

文件中的 `blocks` 方法在调用时，会将打开的文件内容生成的 `IOWrapper` 对象作为参数，也就是我们平时使用 `open` 方法时生成的 `f` 变量：

```py
>>> with open('test.txt') as f:
...     print(f)
...
<_io.TextIOWrapper name='test.txt' mode='r' encoding='UTF-8'>
```

`yield` 关键字使得函数变成生成器函数，函数调用即为生成器（Generator）。如果对 `yield` 关键字和生成器的概念不太熟悉，建议先阅读 [yield 解释](http://pyzh.readthedocs.io/en/latest/the-python-yield-keyword-explained.html)。

### 处理程序

假设我们已经知道一个文本块是 `title/paragraph/heading/list`，通过 `handlers.py` 给它们打上合适的 HTML 标记。将以下代码写入 `handlers.py` 文件：

```py
# -*- coding: UTF-8 -*-
'''
HTML 文本处理类，用于打印各种 HTML 标签
'''

class Handler:
    """
    处理程序父类
    """

    def callback(self, prefix, name, *args):
        # 获取处理方法，getattr 接收三个参数
        # 1、Python 对象
        # 2、属性字符串
        # 3、缺省值
        # 返回值为 Python 对象的属性值，如果没有此属性值，返回第三个参数
        method = getattr(self, prefix + name, None)
        # 调用处理方法，将其返回值作为当前函数的返回值
        if callable(method): return method(*args)

    def start(self, name):
        self.callback('start_', name)

    def end(self, name):
        self.callback('end_', name)

    # 参数 name 的值为过滤器名字，是字符串
    def sub(self, name):
        def substitution(match):
            result = self.callback('sub_', name, match)
            if result is None:
                result = match.group(0)
            return result
        return substitution


class HTMLRenderer(Handler):
    """
    HTML 处理程序，给文本块加相应的 HTML 标记
    """

    # 以下各个方法分别在符合条件时被调用，打印标签或标签的 text 值

    def start_document(self):
        print('<html><head><title>ShiYanLou</title></head><body>')

    def end_document(self):
        print('</body></html>')

    def start_paragraph(self):
        print('<p style="color: #444;">')

    def end_paragraph(self):
        print('</p>')

    def start_heading(self):
        print('<h2 style="color: #68BE5D;">')

    def end_heading(self):
        print('</h2>')

    def start_list(self):
        print('<ul style="color: #363736;">')

    def end_list(self):
        print('</ul>')

    def start_listitem(self):
        print('<li>')

    def end_listitem(self):
        print('</li>')

    def start_title(self):
        print('<h1 style="color: #1ABC9C;">')

    def end_title(self):
        print('</h1>')

    def sub_emphasis(self, match):
        return('<em>%s</em>' % match.group(1))

    def sub_url(self, match):
        s = ('<a target="_blank" style="text-decoration: none;'
                'color: #BC1A4B;" href="{}">{}</a>')
        return s.format(match.group(1), match.group(1))

    def sub_mail(self, match):
        s = ('<a style="text-decoration: none;color: #BC1A4B;" '
                'href="mailto:{}">{}</a>')
        return s.format(match.group(1), match.group(1))

    def feed(self, data):
        print(data)
```

在上面的代码中 `callable` 方法用于检查一个函数是否能够被调用，Python 内置方法 `gerattr` 用于返回一个对象的属性值。举例来说，`getattr(x, 'foo', None)` 就相当于是 `x.foo`，而如果没有这个属性值 `foo`，则返回我们设定的默认值 `None`。

### 规则

有了处理程序和文本块生成器，接下来就需要一定的规则来判断每个文本块交给处理程序将要加什么标记，`rules.py` 代码如下：

```py
# -*- coding: UTF-8 -*-
'''处理文本块的规则类，所有类均为单例模式，在程序运行时除了 Rule 每个类仅创建一个实例
'''

class Rule:
    """
    所有规则类的父类
    """

    def action(self, block, handler):
        """
        加标记，以下三行执行打印 HTML 标签的功能
        """
        handler.start(self.type)    # 打印标签头
        handler.feed(block)         # 打印标签 text 部分
        handler.end(self.type)      # 打印标签尾
        return True                 # 打印完成，返回 True


class HeadingRule(Rule):
    """
    一号标题规则，HTML 文件的一级标题规则（最大字号）<h1> 标签
    """

    type = 'heading'    # 文本块类型

    def condition(self, block):
        """
        判断文本块是否符合规则，返回值为布尔值 True 或 False
        """
        return not '\n' in block and len(block) <= 70 and not block[-1] == ':'


class TitleRule(HeadingRule):
    """
    二号标题规则，次级标题规则，继承一号标题规则类 <h2> 标签
    """

    type = 'title'  # 文本块类型
    first = True    # 这是一个浮动值
                    # 首次调用该类的实例，该值为 True
                    # 之后调用该类的实例，该值为 False

    def condition(self, block):
        # 以下三行代码保证在首次调用该类实例和后续调用时 first 的值不同
        # 符合一号标题规则的文本块一定符合二号标题规则，它们只有先后次序这一个区别
        # 首次调用时返回 False ，即代码块不符合二号标题规则
        # 之后调用返回 True ，即使用该类的 action 方法进行处理
        if not self.first:
            return False
        self.first = False
        return super().condition(block)


class ListItemRule(Rule):
    """
    列表项规则，<li> 标签
    """

    type = 'listitem'   # 文本块类型

    def condition(self, block):
        # 行首为减号，则该代码块符合列表项规则
        return block[0] == '-'

    def action(self, block, handler):
        handler.start(self.type)        # 打印 <li> 标签头
        handler.feed(block[1:].strip()) # 打印 <li> 标签的 text 部分，注意去掉减号
        handler.end(self.type)          # 打印 <li> 标签尾
        return True                     # 处理完毕，返回 True


class ListRule(ListItemRule):
    """
    列表规则，<ul> 标签
    """
    type = 'list'   # 文本块类型
    inside = False  # 该值亦为浮动值，判断是否为列表规则

    def condition(self, block):
        # 判断代码块是否符合规则这里返回 True
        # 在 action 方法中调用父类的同名方法再次判断
        return True

    def action(self, block, handler):
        # 如果 self.inside 为 False 且父类的 condition 方法返回值为 True
        # 第一次出现符合列表项规则的文本块时，满足这两个要求
        if not self.inside and super().condition(block):
            # 打印 <ul> 标签头
            handler.start(self.type)
            # 将 inside 属性值改为 True
            self.inside = True
        # 打印一堆连续 li 标签后，出现非列表项规则的文本块
        elif self.inside and not super().condition(block):
            # 打印 <ul> 标签尾
            handler.end(self.type)
            # 再次修改 inside 属性为 False
            self.inside = False
        # 该方法只用于在合适的条件下打印 <ul> 标签
        # 永远返回 False ，以调用其它规则实例继续处理
        return False


class ParagraphRule(Rule):
    """
    段落规则，<p> 标签
    """

    type = 'paragraph'  # 文本块类型

    def condition(self, block):
        # 不符合以上各类的判断规则的代码块一律按此规则处理
        return True


# 注意这个列表中实例的顺序不能随意改动，原因参见相应类中的注释说明
rule_list = [ListRule(), ListItemRule(), TitleRule(), HeadingRule(),
        ParagraphRule()]
```

### 解析

当我们知道每一个文本块进行怎么样的处理，交给谁去处理之后，我们就可以对整个文本进行解析了，`markup.py` 代码如下：

```py
# -*- coding: UTF-8 -*-
import sys, re
from handlers import HTMLRenderer
from util import blocks
from rules import rule_list


class Parser:
    """
    解析器父类
    """
    def __init__(self, handler):
        self.handler = handler
        self.rules = []     # 判断文本块规则的实例列表
        self.filters = []   # 过滤方法列表

    def addRule(self, rule):
        """
        向 self.rules 列表中添加规则类的实例
        """
        self.rules.append(rule)

    def addFilter(self, pattern, name):
        """
        向 self.filters 列表中添加过滤函数
        """
        # 该过滤器函数接收两个参数：文本块和 HTMLRenderer 类的实例
        def filter(block, handler):
            # re.sub 接收三个参数 a b c ，将字符串 c 中的 a 字段替换成 b
            # b 的值可以是字符串或函数
            # 在下一行代码中 b 参数的值为 handler 的 sub 方法的返回值
            # sub 方法的参数 name 的值为字符串，它和 'sub_' 组成一个大字符串
            # 大字符串就是 handler 的一个方法名
            return re.sub(pattern, handler.sub(name), block)
        self.filters.append(filter)

    def parse(self, file):
        """
        核心方法，解析文本，打印符合要求的标签，写入新的文件中
        """
        # 调用 handler 实例的 start 方法，参数为 handler 实例的某个方法名的一部分
        # 结果就是调用 handler 的 start_document 方法，打印文档的 head 标签
        self.handler.start('document')
        # blocks 是从 urti.py 文件引入的生成器函数
        # blocks(file) 就是一个生成器
        # 使用 for 循环生成器
        for block in blocks(file):
            # 调用过滤器，对每个文本块进行处理
            for filter in self.filters:
                block = filter(block, self.handler)
            # 循环规则类的实例
            for rule in self.rules:
                # 如果符合规则，调用实例的 action 方法打印标签
                if rule.condition(block):
                    last = rule.action(block, self.handler)
                    # 如果 action 方法的返回值为 True
                    # 表示该文本块处理完毕，结束循环
                    if last:
                        break
        # 同 self.handler.start
        # 调用 handler 的 end_document 方法打印 '</body></html>'
        self.handler.end('document')


class BasicTextParser(Parser):
    """
    纯文本解析器
    """

    def __init__(self, handler):
        # 运行父类 Parser 的同名方法
        super().__init__(handler)
        # 增加规则类的实例到 self.urls 列表
        for rule in rule_list:
            self.addRule(rule)
        # 增加三个过滤函数，分别处理斜体字段、链接和邮箱
        self.addFilter(r'\*(.+?)\*', 'emphasis')
        self.addFilter(r'(http://[\.a-zA-Z/]+)', 'url')
        self.addFilter(r'([\.a-zA-Z]+@[\.a-zA-Z]+[a-zA-Z]+)', 'mail')


def main():
    '''
    主函数，控制整个程序的运行
    '''
    handler = HTMLRenderer()
    parser = BasicTextParser(handler)
    # 将文件内容作为标准输入，sys.stdin 获取标准输入的内容，生成 IOWrapper 迭代器对象
    parser.parse(sys.stdin)


if __name__ == '__main__':
    main()
```

### 运行与测试

可以通过下面命令将代码下载并解压到实验环境中，进入相关文件夹中：

```sh
$ wget https://labfile.oss.aliyuncs.com/courses/70/python_markup.zip
$ unzip python_markup.zip
$ cd python_markup
```

下载的文件夹中已经包含了测试所用的 `.txt` 文件，同学们也可以使用自己编写的 `.txt` 文件，只要保证 `.txt` 文件与之前编写的四部分代码处于同一目录下即可，并进入该目录下即可。
接下来我们运行代码，在命令行终端输入以下命令运行程序（纯文本文件为 `test.txt`，生成 HTML 文件为 `test.html`）：

```
python3 markup.py < test.txt > test.html
```

![python](https://doc.shiyanlou.com/courses/1523/1211796/6408b23b5eb1260060c3cce46bd763a6-0/wm)

命令说明：

- `<` 为重定向命令符，将 `test.txt` 文件内容作为标准输入
- `<` 也是重定向命令符，将标准输出（即程序运行时 `print` 方法打印到终端的内容）重定向到文件 `test.html` 中

获得 `test.html` 文件之后，在浏览器中看一下我们的文本解释效果，注意在这里我们没有打开端口，所以不是打开 Web 服务，而是通过右击文件，点击 Open，选择 Preview 来查看效果。

![text](https://doc.shiyanlou.com/courses/1523/1211796/904a6bb0b9d0c93633f111db19436709-0/wm)
![web](https://doc.shiyanlou.com/courses/1523/1211796/861b6f8d4c3ed09d170344ff9d433402-0/wm)

## 实验总结

在这个小程序中，我们使用了 Python 来解析纯文本文件并生成 HTML 文件，这个只是简单实现，支持了很少部分的 Markdown 语法，通过这个案例大家可以动手试试解析完整的 Markdown 文件。
