Less 是一门 CSS 预处理语言，使得 CSS 更容易维护与扩充。

## 变量

``` less
@zhihu-color: #0767b8;

#header {
    background-color: @zhihu-color;
}
```

编译结果：

``` less
#header {
    background-color: #0767b8;
}
```

### 作用域

``` less
@var: red;

#page {
    @var: white;
    #header {
        color: @var; // white
    }
}
```

### 变量懒加载

Less 中的变量不需要在使用之前定义，只需要在作用域可访问即可。

``` less
.lazy-eval {
    width: @var;
}

@var: 9%;
```

或者：

``` less
.lazy-eval-scope {
    width: @var;
}

@var: 100%;
```

### 变量插值

Less 中的变量不仅可以表示 CSS 中的属性值，同时也能用于其他地方，比如选择器名、属性名、URL 拼接、`@import` 拼接等。

#### 选择器名

``` less
@mySelector: banner;

.@{mySelector} {
    font-weight: bold;
}
```

编译结果：

``` less
.banner {
    font-weight: bold;
}
```

#### 属性名

``` less
@property: color;

.widget {
    @{property}: #0ee;
    background-@{property}: #999;
}
```

编译结果：

``` less
.widget {
    color: #0ee;
    background-color: #999;
}
```

#### URL

``` less
@images: "../img";

body {
    background: url("@{images}/white-sand.png");
}
```

#### `@import`

``` less
@themes: "../../src/themes";

@import "@{themes}/tidal-wave.less";
```

## 函数

Less 内置了多种函数用于转换颜色、处理字符串、算术运算等。

更多的函数请参考[Less 函数参考手册](http://lesscss.org/functions/)

## 混合

Less 中的混合是指将 CSS 中的一组属性混合到另外一组属性中。符合 CSS 语法规则的属性组都可以混合到另外一组属性中。

``` less
.class, #id {
    color: #0767b8;
}

.mixin-class {
    .class; // 写成 ".class();" 亦可，两者无任何区别
}

.mixin-id {
    #id; // 写成 "#id();" 亦可，两者无任何区别
}
```

编译结果：
``` less
.class, #id {
    color: #0767b8;
}
.mixin-class {
    color: #0767b8;
}
.mixin-id {
    color: #0767b8;
}
```

如果不希望输出混合体，则在生命混合体时在后面添加 `()`。

``` less
.my-mixin {
    color: black;
}
.my-other-mixin() {
    background: white;
}
.class {
    .my-mixin;
    .my-other-mixin;
}
```

编译结果：

``` less
.my-mixin {
    color: black;
}
.class {
    color: black;
    background: white;
}
```

混合体内不仅可以包含属性集，同时也能包含选择器。

``` less
.my-hover-mixin() {
    &:hover {
        border: 1px solid red;
    }
}
button {
.   my-hover-mixin;
}
```

编译结果：

``` less
button:hover {
    border: 1px solid red;
}
```

从上面例子中不难总结，所谓的“混合”便是利用选择器声明了一段 CSS 块，选择器被调用时将会被替换为其所代表的 CSS 块。

### 带参数的混合

``` less
.border-radius(@radius) {
    border-radius: @radius;
}

#header {
  .border-radius(6px);
}
```

给参数设定默认值：

``` less
.border-radius(@radius: 6px) {
    border-radius: @radius;
}

#header {
  .border-radius();
}
```

多个参数之间用 `;` 隔开。

`@arguments` 在混合中表示所有的参数。

``` less
.box-shadow(@x: 0; @y: 0; @blur: 1px; @color: #000) {
    box-shadow: @arguments;
}
.big-block {
    .box-shadow(2px; 5px);
}
```

编译结果：

``` less
.big-block {
    box-shadow: 2px 5px 1px #000;
}
```

`@rest` 在混合中表示剩余的参数。

``` less
.box-shadow(@margin: 6px; ...) {
    margin: @margin;
    box-shadow: @rest;
}
.big-block {
    .box-shadow(8px; 2px; 5px; 1px; #000);
}
```

编译结果：

``` less
.big-block {
    margin: 8px;
    box-shadow: 2px 5px 1px #000;
}
```

## 嵌套

在 Less 中，选择器可以嵌套。

``` less
.container {
    #item {
        color: #0767b8;
    }
}
```

编译结果：

``` less
.container #item {
    color: #0767b8;
}
```
还有一个很常见的例子。

``` less
.button {
    &:hover {
        color: #0767b8;
    }
}
```

编译结果：

``` less
.button:hover {
    color: #0767b8;
}
```

## 运算

Less 中定义的变量可以进行运算。

``` less
@container-height: 800px;
@header-height: 200px;
.main {
    height: @container-height - @header-height;
}
```

任何数字、颜色等变量都可以进行运算。

## 命名空间

``` less
#bundle {
    .button {
        background-color: #0767b8;
        &:hover {
            background-color: white
        }
    }
    .tab { ... }
    .citation { ... }
}
```

当把 `.button` 混合进 `#header a`中时，我们可以这么做：

``` less
#header a {
    color: orange;
    #bundle > .button;
}
```

## 注释

Less 中块注释和行注释均可以使用，当时要注意块注释的内容在编译后仍然存在，而行注释中的内容在编译后则会被删除。

``` less
/* 一个注释块
style comment! */
@var: red;

// 这一行被注释掉了！
@var: white;
```

编译结果：
``` less
/* 一个注释块
style comment! */
@var: red;

@var: white;
```

## 导入

可以导入 `.less` 或者 `.css` 文件，如果导入 `.less` 文件，则可以省略拓展名。需要注意的是，变量的使用与导入的顺序无关。

``` less
@import "library"; // library.less
@import "typo.css";
```
