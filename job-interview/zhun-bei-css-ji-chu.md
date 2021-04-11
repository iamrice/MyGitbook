# 准备：CSS 基础

## 1. flex 弹性布局

1. flex-direction: row \| row-reverse \| column \| column-reverse
2. justify-content: flex-start \| flex-end \| center \| space-around \| space-between
3. align-items: flex-start \| flex-end \| center \| baseline \| skretch
4. align-content: flex-start \| flex-end \| center \| space-around \| space-between \| skretch
5. flex-warp : warp \| nowarp \| warp-reverse

## 2. BFC: 块级格式化上下文

### 简介

1. BFC: block formatting context。独立的一块渲染区域，不影响外界元素。

### 触发条件

1. 触发条件一：浮动元素 -&gt; float 非none
2. 触发条件二：块级元素 -&gt; overflow 非 visible
3. 触发条件三：绝对定位元素 -&gt; position 为 absolute 或 fixed
4. 触发条件四：根元素 HTML，这个不用出发啦，本身存在的，因此他的整个页面都有BFC特性，例如上下margin重叠。
5. 触发条件五：display 不为 block, inline, none

### 应用场景

1. 应用场景一：解决父元素高度塌陷。浮动元素脱离文档流，进入浮动流（VIP），导致父元素检测不到子元素的高度，因此产生高度塌陷。
2. 应用场景二：左右两栏自适应布局
3. 应用场景三：上下两栏外边距重叠，为每个元素包裹一个bfc，这样他们就分属不同的bfc了

