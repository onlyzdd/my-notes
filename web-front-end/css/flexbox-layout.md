# Flexbox布局

传统的布局解决方案，基于盒子模型，依赖`display`、`position`、`float`属性，这对于特殊布局非常不方便，而且很容易破坏文档流。

2009年，W3C提出了一种新的方案——Flexbox布局，可以简便、完整、响应式地实现各种页面布局。目前，已得到所有浏览器的支持。

## Flexbox布局是什么？

Flexbox是Flexible Box的缩写，意为“弹性布局”，用来为盒子模型提供最大的灵活性。

任何一个块级元素都可以指定为Flex布局。

```css
.box {
    display: flex;
}
```

行内元素也可以使用Flex布局：

```css
.box {
    display: inline-flex;
}
```

设置为Flex布局以后，子元素的`float`、`clear`、`vertical-align`属性将失效。

## 基本概念

采用Flex布局的元素，称为Flex容器（flex container），简称容器。它的所有子元素自动称为容器成员，称为Flex项目（flex item），简称项目。

![基本概念](./flexbox-layout/flex-concept.png)

容器默认存在两根轴：水平的主轴（_main axis_）和垂直的交叉轴（_cross axis_）。主轴的开始位置（与边框的交叉点）叫做 _main start_，结束位置叫做 _main end_；交叉轴的开始位置叫做 _cross start_，结束位置叫做 _cross end_。

项目默认沿主轴排列。单个项目占据的主轴空间叫做 _main size_，占据的交叉轴空间叫做 _cross size_。

## 容器的属性

以下6个属性设置在容器上。

- flex-direction
- flex-wrap
- flex-flow
- justigy-content
- align-items
- align-content

### flex-direction属性

`flex-direction`属性决定主轴的方向（即项目的排列方向）。

```css
.box {
    flex-direction: column-reverse | column | row | row-reverse;
}
```

![flex-direction](./flexbox-layout/flex-direction.png)

- row（默认值）：主轴为水平方向，起点在左端。
- row-reverse：主轴为水平方向，起点在右端。
- column：主轴为垂直方向，起点在上沿。
- column-reverse：主轴为垂直方向，起点在下沿。

### flex-wrap属性

默认情况下，项目都排在一条线（又称"轴线"）上。`flex-wrap`属性定义，如果一条轴线排不下，如何换行。

![flex-wrap](./flexbox-layout/flex-wrap.png)

```css
.box {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```

- nowrap（默认值）：不换行。
- wrap：换行，第一行在上方。
- wrap-reverse：换行，第一行在下方。

### flex-flow属性

`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

```css
.box {
    flex-flow: <flex-direction> || <flex-wrap>;
}
```

### justify-content属性

`justify-content`属性定义了项目在主轴上的对齐方式。

![justify-content](./flexbox-layout/justify-content.png)

```css
.box {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

- flex-start（默认值）：左对齐。
- flex-end：右对齐。
- center：居中。
- space-between：两端对齐，项目之间的间隔都相等。
- space-round：每个项目两侧的间隔相等，所以，项目的间隔比项目与边框的间隔大一倍。

### align-items属性

`align-items`属性定义项目在交叉轴上如何对齐。

![align-items](./flexbox-layout/align-items.png)

```css
.box {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

- flex-start：交叉轴的起点对齐。
- flex-end：交叉轴的终点对齐。
- center：交叉轴的中点对齐。
- baseline：项目的第一行文字的基线对齐。
- stretch（默认值）：如果项目未设置高度或设为`auto`，将沾满整个容器的高度。

### align-content属性

`align-content`属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

![align-content](./flexbox-layout/align-content.png)

```css
.box {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

- flex-start：与交叉轴的起点对齐。
- flex-end：与交叉轴的终点对齐。
- center：与交叉轴的中点对齐。
- space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
- space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
- stretch（默认值）：轴线占满整个交叉轴。

## 项目的属性

以下6个属性设置在项目上。

- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self

### order属性

`order`属性定义项目的排列顺序。数值越小，排列越靠前，默认为`0`。

```css
.item {
    order: <integer>;
}

```

### flex-grow属性

`flex-grow`属性定义项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大。

```css
.item {
    flex-grow: <number>; /* default 0 */
}
```

如果所有项目的`flex-grow`属性都为`1`，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为`2`，其他项目都为`1`，则前者占据的剩余空间将比其他项多一倍。

### flex-shrink属性

`flex-shrink`属性定义了项目的缩小比例，默认为`1`，即如果空间不足，该项目将缩小。

```css
.item {
    flex-shrink: <number>; /* default 1 */
}
```

如果所有项目的`flex-shrink`属性都为`1`，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为`0`，其他项目都为`1`，则空间不足时，前者不缩小。

负值对该属性无效。

### flex-basis属性

`flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。

```css
.item {
    flex-basis: <length> | auto; /* default auto */
}
```

它可以设为跟`width`或`height`属性一样的值（比如`350px`），则项目将占据固定空间。


### flex属性

`flex`属性是`flex-grow`, `flex-shrink`和`flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。

```css
.item {
    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

该属性有两个快捷值：`auto` (`1 1 auto`) 和 `none` (`0 0 auto`)。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

### align-self属性

`align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

```css
.item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```
