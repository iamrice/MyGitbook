# 准备：CSS 基础

## 1. flex 布局

### 轴向

1. flex-direction: row \| row-reverse \| column \| column-reverse
2. flex-warp : warp \| nowarp \| warp-reverse

### 对齐

1. justify-content: flex-start \| flex-end \| center \| space-around 分散对齐 \| space-between 两端对齐
2. align-items: flex-start \| flex-end \| center \| baseline \| skretch 

   交叉轴只有一行元素时的对齐方式

3. align-content: flex-start \| flex-end \| center \| space-around \| space-between \| skretch 交叉轴有多行元素时的对齐方式

![](../.gitbook/assets/image%20%287%29.png)

![](../.gitbook/assets/image%20%286%29.png)

### 弹性

1. flex-grow: 当容器空间剩余时，用【0，1】的取值来设置当前块相对于剩余空间的拓展比例，默认为0，即不扩充，grow为1时，将空间填满。
2. flex-shrink：当容器空间不足时，收缩的比例。默认为1，即所有同级子元素等比收缩，shrink更大的子元素收缩更多，总之子元素的排布不会超出父元素。
3. flex-basis：设置宽度，优先级高于width

本节参考：[https://www.bilibili.com/video/BV1k441157hd](https://www.bilibili.com/video/BV1k441157hd/?spm_id_from=333.788.recommend_more_video.-1)

### 应用场景

1. 水平垂直居中布局
2. 让页面的header和 footer固定高度，content充满剩余空间。

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

本节参考：[https://www.bilibili.com/video/BV16b411H7P](https://www.bilibili.com/video/BV16b411H7Pz)   
[https://www.zhihu.com/question/35375980](https://www.zhihu.com/question/35375980)  
[https://www.cnblogs.com/xiaohuochai/p/5248536.html](https://www.cnblogs.com/xiaohuochai/p/5248536.html)

## 3. display  的属性值

### block

1. block元素会独占一行，多个block元素会各自新起一行。默认情况下，block元素宽度自动填满其父元素宽度。
2. block元素可以设置width,height属性。块级元素即使设置了宽度,仍然是独占一行。
3. block元素可以设置margin和padding属性。

### inline

1. inline元素不会独占一行，多个相邻的行内元素会排列在同一行里，直到一行排列不下，才会新换一行，其宽度随元素的内容而变化。
2. inline元素设置width,height属性无效。
3. inline元素的margin和padding属性，水平方向的padding-left, padding-right, margin-left, margin-right都产生边距效果；但竖直方向的padding-top, padding-bottom, margin-top, margin-bottom不会产生边距效果。

### inline-block

1. 多个元素在同一行排列
2. 可设置宽高
3. 可设置内外边距

本节参考：[https://www.cnblogs.com/keithwang/p/3139517.html](https://www.cnblogs.com/keithwang/p/3139517.html)

## 4. 文档流、浮动流、定位流

可以认为，元素脱离文档流之后，相当于在文档流的上层进行布局，起到遮盖的效果。

![&#x4E0A;&#x9762;&#x4E09;&#x4E2A;&#x7EA2;&#x5757;&#x5C31;&#x662F;&#x6D6E;&#x52A8;&#x6D41;](../.gitbook/assets/image%20%285%29.png)

## 5. position

### static

默认值，元素在正常的页面流中，top等属性无效。

### relative

以元素的默认位置作为定位基点进行偏移。

### absolute

以父元素的位置作为定位基点进行偏移，如果父元素为static，则找到第一个非static的父元素。

### fixed

相对于视图窗口偏移。

### sticky

相对于视图窗口偏移，在到达定位点之前是relative，到达定位点之后是fixed。定位点为top\bottom\left\right 四者之一。

## 6. 伪元素 & 伪类

### 伪类

指定了元素在特定状态下激活后的样式

伪类选择器属于CSS选择器，CSS选择器还包括ID选择器、属性选择器、类选择器、结构选择器、标签选择器。

#### 结构选择器

根据亲属关系来选择元素

1. \#tony.nick tony的子孙中标签叫nick的所有元素
2. \#tony &gt; nick   tony的儿子中叫nick的元素
3. \#tony + nick   紧挨着tony之后叫nick的元素

### 伪元素

在指定元素后添加独立于DOM之外的元素。

