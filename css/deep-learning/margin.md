## 深入理解 margin

### margin 与容器尺寸

标准盒模型与元素尺寸

![](./res/css-box.png)

元素尺寸：

- 可视尺寸： clientWidth(标准)
- 占据尺寸： outerWidth

#### margin 与可视尺寸

1. 适用于没有设定 width / height 的普通 block 水平元素；
2. 只适用于水平方向尺寸（即 margin 只会修改水平方向尺寸）

`margin: 50px;`

![](./res/margin-50px.png)

`margin: 50px -50px;`

![](./res/margin-hor-minus50px.png)

`margin: 30px 100px;`

![](./res/margin-ver-30px.png)

应用场景：

- 一侧定宽的自适应布局

```
<img width="150" style="float">
```
