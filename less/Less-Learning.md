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
  .my-hover-mixin;
}
```

编译结果：

``` less
button:hover {
  border: 1px solid red;
}
```

从上面例子中不难总结，所谓的“混合”便是利用选择器声明了一段 CSS 块，选择器被调用时将会被替换为其所代表的 CSS 块。

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
