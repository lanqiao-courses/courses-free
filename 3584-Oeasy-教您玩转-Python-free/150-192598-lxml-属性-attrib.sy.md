---
show: step
version: 1.0
enable_checker: true
---

# 语法 html 属性 attrib

## 回忆

- 了解了 html 中的 dom 树
- 树是由节点元素组成的
  - 节点元素可以用 etree.Element()得到
  - 最根源的元素是根元素
  - html 树的根就是 html 元素
  - 或者说 dom-tree 的根就是 html
- html 里面包括子节元素点
  - head
  - body
    - ul
- 重复类型的元素怎么追加呢？🤔

### 继续

- 直接添加列表
- 失败了

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630458028665)

- 看来 etree.Element()元素的子节点
  - 不能是列表 list 对象

### 逐个插入

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630459987697)

- etree 元素是递归的
- etree 元素下面只能是 etree 元素

### 元素索引

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460162190)

- etree 元素很像列表
  - 可以用索引找到下级元素

### 元素切片

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460277972)

- 可以用切片的方式访问元素

### 判断是否是元素

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460368714)

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460447051)

- 如果是元素
  - 有几个子元素
- 可以用 len 得到元素的子元素数量

### 遍历元素

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460561706)

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460569619)

- 这个元素和列表为什么那么相像？

### 元素赋值

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460612269)

- 原来 Element 元素这个类是从 list 列表类派生出来的
- Element 的 tag 属性对应标签的名字
- 不过有些地方和列表不同
  - 赋值的时候
  - 被替换元素会把原来位置的子元素替换掉
  - 被替换元素从原来的位置被删除

### tag

- 验证一下 Element 元素的 tag 属性

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211106-1636192693875)

- 再验证一下子元素的替换

### 子元素替换

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211106-1636192792753)

- 确实把一个子元素掰到别的地方
- 元素替换之后
- 旧的就没有了

### 深拷贝

- 如果想要新建一个类似的 etree 节点
- 可以考虑深拷贝

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460822360)

### 父子兄弟

- 判读是否是父亲

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460949130)

- 判断是否是哥哥或弟弟

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460957729)

### 动手尝试

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630460965035)

- 通过节点得到
  - 父亲
  - 哥哥
  - 弟弟
- 伦理清晰

### 属性

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630461037878)

- etree 元素的属性很像像一个字典 dict
- 我们来试试

### 属性字典

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630461635127)

- 可以为元素设置属性和属性值

### 遍历属性

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630461726672)

- 这真的很像一个字典
- 这就是一个字典

### 属性对象

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630461979008)

- attrib 是节点元素的属性组
  - 属性组是节点元素的成员变量
  - 属性组的类型是一个字典
- 可以用 get 和索引的方式得到属性的值

### 属性操作

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210901-1630462069699)

- 使用 get 的操作更安全
- 索引可能会爆出 key 不存在的 KeyError

## 总结

- 了解 etree 中的元素关系
  - 父子
  - 兄弟
- 了解元素的标签成员
  - tag
- 了解元素中的属性组成员
  - attrib
  - 属性对象本质是一个字典
  - 可以用 get 和索引的方式得到具体的值
  - 使用 get 的方式更安全
- 除了标签和属性组成员之外
  - 元素类还有文本成员
  - 这文本成员怎么理解？🤔
- 下次再说
