---
show: step
version: 1.0
enable_checker: true
---

# class 与 style 绑定

## 简介

操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是属性，所以我们可以用 `v-bind` 处理它们：只需要通过表达式计算出字符串结果即可。不过，字符串拼接麻烦且易错。因此，在将 `v-bind` 用于 `class` 和 `style` 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。动态的绑定与修改。

#### 知识点

- 元素的 class 对象语法绑定
- 元素的 class 数组语法绑定
- 元素的 style 对象语法绑定
- 元素的 style 数组语法绑定
- 属性的自动前缀

## 元素的 class 绑定

元素的 class 绑定很常见，在 Vue 中它的绑定语法也有很多种

### 对象语法

给 `v-bind:class` 一个对象，以动态地切换 class，语法表示 `active` 这个 class 存在与否将取决于数据属性 `isActive` 的 Boolean 值，大致语法 {className:Boolean}

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>syl-vue-test</title>
    <!-- 引入 vue.js -->
    <script src="https://labfile.oss.aliyuncs.com/courses/1262/vue.min.js"></script>
    <style>
      .active {
        color: pink;
        font-size: 22px;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <!-- 数组语法绑定 class 当 isActive 为 true 时，active 就成 span 标签的 class -->
      <span v-bind:class="{'active':isActive}">syl</span>
      <!-- isActive 为true 渲染结果 <span class="active">syl</span> -->
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          isActive: true,
        },
      });
    </script>
  </body>
</html>
```

运行结果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid940410labid10292timestamp1552638097637.png/wm)

最终标签 class 渲染结果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid940410labid10292timestamp1552638097898.png/wm)

你可以在对象中传入更多属性来动态切换多个 class，`v-bind:class` 指令也可让普通的 class 并存。

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>syl-vue-test</title>
    <!-- 引入 vue.js -->
    <script src="https://labfile.oss.aliyuncs.com/courses/1262/vue.min.js"></script>
    <style>
      .active {
        color: pink;
        font-size: 22px;
      }
      .red-bg {
        background: red;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <!-- 数组语法绑定 class   当isActive为true时，active就成成span标签的class -->
      <span class="static" v-bind:class="{'active':isActive,'red-bg':isRed}"
        >syl</span
      >
      <!-- isActive 为true 渲染后 <span class="static active red-bg">syl</span> -->
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          isActive: true,
          isRed: true,
        },
      });
    </script>
  </body>
</html>
```

运行结果:

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid940410labid10292timestamp1552638098291.png/wm)

### 数组语法

我们可以把一个数组传给 `v-bind:class`，以应用一个 class 列表：

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>syl-vue-test</title>
    <!-- 引入 vue.js -->
    <script src="https://labfile.oss.aliyuncs.com/courses/1262/vue.min.js"></script>
    <style>
      .active {
        color: pink;
        font-size: 22px;
      }
      .red-bg {
        background: red;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <!-- 数组语法绑定 class -->
      <span v-bind:class="[activeClass,bgColorClass]">syl</span>
      <!-- 渲染后 <span class="active red-bg">syl</span> -->
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          activeClass: "active",
          bgColorClass: "red-bg",
        },
      });
    </script>
  </body>
</html>
```

运行结果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid940410labid10292timestamp1552638098542.png/wm)

## 元素 style 绑定

在前端开发中，内联样式经常被使用到，Vue 中内联样式绑定语法灵活。CSS 属性名可以用驼峰式 (camelCase) 或 **短横线分隔 (kebab-case，记得用单引号括起来)** 来命名。

小建议：如果，你曾使用过 React 或者将要学习它，建议采用与 React 同样的 CSS 属性代码风格驼峰式 (camelCase)，本教程中也使用风格。

### 对象语法

使用对象方式绑定 style

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>syl-vue-test</title>
    <!-- 引入 vue.js -->
    <script src="https://labfile.oss.aliyuncs.com/courses/1262/vue.min.js"></script>
  </head>
  <body>
    <div id="app">
      <p v-bind:style="{fontSize:size,backgroundColor:bgColor}">你好，实验楼</p>
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          size: "26px",
          bgColor: "pink",
        },
      });
    </script>
  </body>
</html>
```

运行结果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid940410labid10292timestamp1552638098912.png/wm)

直接绑定到一个样式对象通常更好，这会让模板更清晰，和上面一个例子是同样的效果：

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>syl-vue-test</title>
    <!-- 引入 vue.js -->
    <script src="https://labfile.oss.aliyuncs.com/courses/1262/vue.min.js"></script>
  </head>
  <body>
    <div id="app">
      <p v-bind:style="styleObject">你好，实验楼</p>
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          styleObject: {
            fontSize: "26px",
            backgroundColor: "pink",
          },
        },
      });
    </script>
  </body>
</html>
```

### 数组语法

`v-bind:style` 的数组语法可以将多个样式对象应用到同一个元素上

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>syl-vue-test</title>
    <!-- 引入 vue.js -->
    <script src="https://labfile.oss.aliyuncs.com/courses/1262/vue.min.js"></script>
  </head>
  <body>
    <div id="app">
      <p v-bind:style="[styleObject1,styleObject2]">你好，实验楼</p>
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          //样式一
          styleObject1: {
            fontSize: "26px",
            backgroundColor: "pink",
          },
          //样式二
          styleObject2: {
            marginTop: "200px",
            textAlign: "center",
          },
        },
      });
    </script>
  </body>
</html>
```

运行结果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid940410labid10292timestamp1552638099123.png/wm)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid940410labid10292timestamp1552638099905.png/wm)

### 属性的自动前缀

在开发中，由于浏览器内核不同，使用某些 CSS 属性需要带各浏览器的前缀，然而如果你在 Vue 中使用了 `v-bind:style`你完全不用去考虑，因为 Vue 在编译过程中，会自动给需要前缀的属性加前缀。

**引申知识：**通过[CANIUSE](https://www.caniuse.com/)查看询 CSS 属性兼容性，自动前缀包 Autoprefixer、postcss。

## 综合练习

动态改变 style 和 切换 class，案例：变大变色

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>syl-vue-test</title>
    <!-- 引入 vue.js -->
    <script src="https://labfile.oss.aliyuncs.com/courses/1262/vue.min.js"></script>
    <style>
      .active {
        color: red;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <p
        v-bind:style="{fontSize:fontSize+'px'}"
        v-bind:class="{active:isActive}"
      >
        你好，实验楼
      </p>
      <button @click="handleClick">变大变色</button>
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          fontSize: 20,
          isActive: true,
        },
        methods: {
          handleClick: function () {
            this.fontSize += 2;
            this.isActive = !this.isActive; //取反
          },
        },
      });
    </script>
  </body>
</html>
```

结果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid940410labid10292timestamp1552638100281.png/wm)

## 总结

- 元素的 class 对象语法绑定
- 元素的 class 数组语法绑定
- 元素的 style 对象语法绑定
- 元素的 style 数组语法绑定
- 属性的自动前缀

本小节学习了在 Vue 中元素 class 和 style 的绑定，对象语法，数组语法。动态的绑定使得更好的打造交互效果。
