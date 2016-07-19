# 深入理解 margin

## margin 与容器尺寸

标准盒模型与元素尺寸

![](./res/margin/css-box.png)

元素尺寸：

- 可视尺寸： clientWidth(标准)
- 占据尺寸： outerWidth

### margin 与可视尺寸

1. 适用于没有设定 width / height 的普通 block 水平元素；
2. 只适用于水平方向尺寸（即 margin 只会修改水平方向尺寸）

`margin: 50px;`

![](./res/margin/margin-50px.png)

`margin: 50px -50px;`

![](./res/margin/margin-hor-minus50px.png)

`margin: 30px 100px;`

![](./res/margin/margin-ver-30px.png)

应用场景：

- 一侧定宽的自适应布局

```
<img width="150" style="float">
<p>图片左浮动...</p>
```

![](./res/margin/margin-left1.png)

对 `<p></p>` 加一个 `margin` 属性，可以达到一侧定宽的效果。

```
<img width="150" style="float">
<p style="margin-left: 180px">图片左浮动...</p>
```

![](./res/margin/margin-left2.png)

### margin 与占据尺寸（元素占据的空间）

1. block / inline-block 水平元素均适用；
2. 与有没有设定 width / height 值无关；
3. 适用于水平方向和垂直方向

`margin-bottom: 0;`

![](./res/margin/margin-bottom1.png)

`margin-bottom: -50px;`

![](./res/margin/margin-bottom2.png)

`margin-bottom: 50px;`

![](./res/margin/margin-bottom3.png)

应用场景：

- 滚动容器上下留白

![](./res/margin/scroll.png)

注：此处 `padding` 只有在chrome 浏览器中才能实现上下留白，其他浏览器只能用 `margin` 来实现。

- 等高布局


## margin 与百分比单位

![](./res/margin/percent1.png)

![](./res/margin/percent2.png)

应用场景：

- 宽高 2:1 自适应矩形

```css
.box {
    background-color: olive;
    overflow: hidden;
}

.box > div {
    margin: 50%;
}
```

![](./res/margin/percent3.png)

## margin 重叠

特性：

- block 水平元素（不包括 float 和 absolute 元素）
-  不考虑 writing-mode， 只发生在垂直方向（margin-top 和 margin-bottom）

margin 重叠 3 种情境：

1. 相邻的兄弟元素

    ![](./res/margin/overlap1.png)

2. 父级和第一个 / 最后一个子元素

    ![](./res/margin/overlap2-1.png)

    ![](./res/margin/overlap2-2.png)

    ![](./res/margin/overlap2-3.png)
    发现上面三段的代码效果是一样的。

    父子 margin 重叠其他条件：

    - margin-top 重叠：
        - 父元素非块状格式化上下文元素
        - 父元素没有 border-top 设置
        - 父元素没有 padding-top 值
        - 父元素和第一个子元素之间没有 inline 元素分割
    - margin-bottom 重叠：
        - 父元素非块状格式化上下文元素
        - 父元素没有 border-bottom 设置
        - 父元素没有 padding-bottom 值
        - 父元素和最后一个子元素之间没有 inline 元素分割
        - 父元素没有 height， min-height, max-height 限制

3. 空的 block 元素

    ![](./res/margin/overlap3.png)

    空 block 元素 margin 重叠其他条件：

    - 元素没有 border 设置
    - 元素没有 padding 值
    - 里面没有 inline 元素
    - 没有 height，或者 min-height

### margin 重叠的计算规则

- 正正取大值
- 正负值相加
- 负负最负值

## margin auto

实例： 元素有时候就算没有设置 width 和 height 值，也会自动填充

![](./res/margin/auto1.png)

如果此时设置 width 或者 height， 自动填充特性就会被覆盖

![](./res/margin/auto2.png)

原本应该填充的尺寸被 width / height 强制变更，而 `margin: auto` 就是为了填充这个变更的尺寸而设计的。

![](./res/margin/auto3.png)

如果一侧定值，一侧 auto， auto 为剩余空间大小（上例），如果两个均是 auto，则平分剩余空间（本例）

![](./res/margin/auto4.png)

### 实际问题解析

1. 图片为何不居中

    ![](./res/margin/auto5.png)

    修改方式：

    ![](./res/margin/auto6.png)

2. 容器定高、元素定高， `margin: auto 0` 无法垂直居中

    ![](./res/margin/auto7.png)

3. 用 `margin: auto` 来设置元素垂直居中

    - writing-mode： 副作用是水平无法居中
        ![](./res/margin/auto8.png)

    - absolute：

        ![](./res/margin/auto9.png)

        ![](./res/margin/auto10.png)

        ![](./res/margin/auto11.png)


## margin 负值应用

### margin 负值下的两端对齐 -- margin 可以改变元素尺寸

![](./res/margin/minus1.png)

![](./res/margin/minus2.png)

### margin 负值下的等高布局 -- margin 改变元素占据空间

![](./res/margin/minus3.png)

## margin

### inline 水平元素的垂直 margin 失效

![](./res/margin/notwork1.png)

![](./res/margin/notwork2.png)

### margin 重叠

### `diaplay: table-cell`

![](./res/margin/notwork3.png)

### `position: absolute`

决定定位元素非定位方位的 margin 值“无效”

```
img {
    top: 10%;
    left: 30%;
}
```

则设置 `margin-bottom` 或者 `margin-right` 无效

决定定位的 margin 值一直有效，但因为元素脱离了文档流，所以看起来无效

### 鞭长莫及导致的无效

![](./res/margin/notwork4.png)

![](./res/margin/notwork5.png)

`float: left` 破坏了文档流，文字的 margin-left 其实不是相对了图片右边，所以如果 margin-left 足够大，那么就可以显示出效果。

![](./res/margin/notwork6.png)

### 内联特性导致的无效

![](./res/margin/notwork7.png)

![](./res/margin/notwork8.png)

当 margin-top 小与一定值后，图片位置则不会变化

![](./res/margin/notwork9.png)

![](./res/margin/notwork10.png)

此时，添加一个文字来辅助理解。

![](./res/margin/notwork11.png)

此时，可以看出来，内联元素受制于其所在的父元素，所以 margin 失效
