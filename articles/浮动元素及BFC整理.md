date: 2020.01.21

# 浮动元素及BFC学习整理

## float和clear

float CSS属性指定一个元素应沿其容器的左侧或右侧放置，允许文本和内联元素环绕它。该元素从网页的正常流动(文档流)中移除，尽管仍然保持部分的流动性

浮动元素是如何定位的？

正如我们前面提到的那样，当一个元素浮动之后，它会被移出正常的文档流，然后向左或者向右平移，一直平移直到碰到了所处的容器的边框，或者碰到另外一个浮动的元素。

```css
<section>
  <div class="left">1</div>
  <div class="left">2</div>
  <div class="right">3</div>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.
     Morbi tristique sapien ac erat tincidunt, sit amet dignissim
     lectus vulputate. Donec id iaculis velit. Aliquam vel
     malesuada erat. Praesent non magna ac massa aliquet tincidunt
     vel in massa. Phasellus feugiat est vel leo finibus congue.</p>
</section>

// css
section {
  border: 1px solid blue;
}

div {
  margin: 5px;
  width: 50px;
  height: 50px;
}

.left {
  float: left;
  background: pink;
}

.right {
  height: 90px;
  float: right;
  background: cyan;
}

```
可以看到两个红色的方块向左浮动贴在一起，蓝色方块向右浮动。`p`标签里的文本在浮动元素周围环绕，这里要注意的是，`p`标签的实际宽度还是父容器的宽度。且浮动元素具有块元素的特性。
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.21/01.png?raw=true)


当我们把`p`标签删除后，会发现`section`高度坍塌。这是因为浮动元素是会脱离文档流的(绝对定位元素会脱离文档流)。如果一个没有高度或者height是auto的容器的子元素是浮动元素，则该容器的高度是不会被撑开的。
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.21/02.png?raw=true)

可能有时候需要让文本不围绕浮动元素，这时候可以使用`clear`属性。     
 `clear `CSS 属性指定一个元素是否必须移动(清除浮动后)到在它之前的浮动元素下面。clear 属性适用于浮动和非浮动元素。
 当给p标签添加`clear: left;`后,可以看见文字没有围绕左边的浮块元素排列，而右边依然围绕，这是因为我们只是清除了左边的浮动。
 ![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.21/03.png?raw=true)

 `clear: right;`同上。这里我们在演示一下常用的`clear: both;`，该属性会清除左右浮动。下面同样给p标签清除浮动，看效果图。
 ![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.21/04.png?raw=true)
 这里我们看到，p标签两边都清除了浮动。

注意：如果一个元素里只有浮动元素，那它的高度会是0。如果你想要它自适应即包含所有浮动元素，那你需要清除它的子元素。一种方法叫做clearfix，即clear一个不浮动的 ::after 伪元素。
 修改`<section class=".clearfix"></section>`,

 ```css
.clearfix::after {
  content: '';
  display: block;
  clear: both;
}
 ```
 ![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.21/05.png?raw=true)

可以看见在容器里只有浮动元素的情况下，容器依然被撑开了，利用伪元素就不用新创建标签了。

## BFC

**块格式化上下文（Block Formatting Context，BFC）** 是Web页面的可视化CSS渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。

下列方式会创建BFC:
- 根元素(<html>)
- 浮动元素（元素的 float 不是 none）
- 绝对定位元素（元素的 position 为 absolute 或 fixed）
- 行内块元素（元素的 display 为 inline-block）
- 表格单元格（元素的 display为 table-cell，HTML表格单元格默认为该值）
- 表格标题（元素的 display 为 table-caption，HTML表格标题默认为该值）
- 匿名表格单元格元素（元素的 display为 table、table-row、 table-row-group、table-header-group、- table-footer-group（分别是HTML table、row、tbody、thead、tfoot的默认属性）或 inline-table）
- overflow 值不为 visible 的块元素
- display 值为 flow-root 的元素
- contain 值为 layout、content或 paint 的元素
- 弹性元素（display为 flex 或 inline-flex元素的直接子元素）
- 网格元素（display为 grid 或 inline-grid 元素的直接子元素）
- 多列容器（元素的 column-count 或 column-width 不为 auto，包括 column-count 为 1）
- column-span 为 all 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中（标准变更，Chrome bug）。

浮动定位和清除浮动时只会应用于同一个BFC内的元素。浮动不会影响其它BFC中元素的布局，而清除浮动只能清除同一BFC中在它前面的元素的浮动。外边距折叠（Margin collapsing）也只会发生在属于同一BFC的块级元素之间。

通过设置上面的属性可以显式创建BFC。

```css
<div class="box">
    <div class="float">I am a floated box!</div>
    <p>I am content inside the container.</p>
</div>
      
// css
.box {
    background-color: rgb(224, 206, 247);
    border: 5px solid rebeccapurple;
}

.float {
    float: left;
    width: 200px;
    height: 150px;
    background-color: white;
    border:1px solid black;
    padding: 10px;
}    

```
可以看到浮动元素脱离的文档流。
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.21/06.png?raw=true)

创建一个包含这个浮动元素的BFC，通常的做法是设置父元素 `overflow: auto` 或者设置其他的非默认的 `overflow: visible `的值，个人喜欢用`overflow: hidden;`。根据上面的例子

```css
.box {
    background-color: rgb(224, 206, 247);
    border: 5px solid rebeccapurple;
    overflow: hidden;
}

```
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.21/07.png?raw=true)

添加上述的属性都能创建一个BFC，但都会有副作用：
- display: table 可能引发响应性问题
- overflow: scroll 可能产生多余的滚动条
- float: left 将把元素移至左侧，并被其他元素环绕
- overflow: hidden 将裁切溢出元素

### display: flow-root

一个新的 display 属性的值，它可以创建无副作用的BFC。在父级块中使用 display: flow-root 可以创建新的BFC。

### 外边距塌陷问题
两个处于同一BFC的相邻的元素可能造成外边距重叠的问题，下面举个栗子。
```css

<div class="container">
    <div class="test1">test1</div>
    <div class="test2">test2</div>
  </div>

// css
    .container {
      background-color:blue;
      overflow: hidden;   // 创建BFC
    }

    .test1 {
      background-color: yellow;
      margin-bottom: 10px;
    }

    .test2 {
      background-color: red;
      margin-top: 10px;
    }

```
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.21/08.png?raw=true)

在BFC中，test1和test2之间的外边距理应是20px，但实际上却只是10px(图中蓝色的高度)，这就是外边距重叠。

为解决这一问题，可以创建两个BFC。对上面的列子进行修改。

```css

<div class="container">
  <div class="test1">test1</div>
  <div class="newBFC">
    <div class="test2">test2</div>
  </div>
</div>

// css
  .container {
    background-color:blue;
    overflow: hidden;   // 创建BFC
  }

  .test1 {
    background-color: yellow;
    margin-bottom: 10px;
  }

  .test2 {
    background-color: red;
    margin-top: 10px;
  }
  
  .newBFC {
    overflow: hidden;
  }

```

![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.21/09.png?raw=true)

这样一来test1和test2处于两个不同的BFC，所以两者就不会发生外边距重叠问题。

