## 基础题CSS相关

### 1,盒模型哪两种模式？什么区别？如何设置

### 2,BFC优化

BFC全称”Block Formatting Context”, 中文为“块级格式化上下文”。  
特性:

- 内部子元素不会影响外部元素。
- 流体特点，水平方向自动填满容器。

###  3, 块级元素与行内元素

块级元素: 默认情况下，块级元素会新起一行。[MDN地址](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Block-level_elements)

- div , canvas,  h1,  h2,   p,  ul,  ol,  form,  table (常用)


行内元素: 一个行内元素只占据它对应标签的边框所包含的空间。

- `b ，i , a , span , input , button , label , code , tt , select , textarea , img , strong` 等(常用)

###  4, 水平居中

```HTML
<!--块级元素-->
<div class="box" style="border: #0aa66e solid 1px;height: 200px;width: 400px;">
      <div class="content" style="width:100px; background-color: #0A8CD2">
            我是块级元素，我要水平居中。或者还有垂直居中.我定宽不定高.
      </div>
</div>
<br><br><br>
<!--行内元素-->
<div class="box" style="border: #0aa66e solid 1px;height: 200px;width: 400px;">
      <span class="inlineContent" style="background-color: #0A246A;">我是行内元素，我要水平居中。</span>
</div>
```

**行内元素** ：只需要紧邻父级 `text-align:center;`

**块级元素**：

- （定宽）方式一：`margin: 0 auto;`
- （定宽）方式四 绝对定位：父级：`position: relative;`当前元素：`position:absolute; top:0; left: calc(50% - 50px);`50% 减去自身宽度。
- （不定宽）方式二inline-block:  父级 ，`text-align: center;` 当前元素：`display: inline-block;`
- (不定宽)方式三flex：父级: `display:flex; justify-content:center:`



### 5, 垂直居中

**单行行内元素：** `line-height`等于盒子 `height`"，即行高等于盒子的高"

**多行行内元素：** 父元素设置 `display:table-cell`;和`vertical-align: middle`;

**块级元素：**

- （定高）绝对定位 : 父级：`position: relative;`当前元素：`position:absolute; top:calc(50% - 50px); left: 0`  50% 减去自身高度。
- （不用定高）**flex**：父级：`display: flex; align-item: center;`
- （不用定高）transform：父级：`position: relative;`当前元素：`position:absolute; top:50%; transform: translateY(- 50%)`



### 6, css优先级/权重

 !import (无限大)  > 行内样式( 1000 ) >  id选择器 (100) >	class选择器/伪类(:hover) (10)   >  标签选择器/伪元素(::after)) (1) 

### 7,关于vh, vw



### 8,常用清除浮动的方法，如不清除浮动会怎样？

​	浮动核心就一句话：**浮动元素会脱离文档流并向左/向右浮动，直到碰到父元素或者另一个浮动元素**。请默念3次！

​	`clear` 清除浮动: 在最后一个浮动元素上使用 clear:both;

### 9, 删格化的原理

​	**删格化**是指将页面布局划分为等宽的列，然后通过列数的定义来模块化布局。

栅格系统的大小并不是固定的。它会随着页面的大小比例随之改变，就像最开始学习网页的布局一样，并不是定宽定长的，和网页里的百分比布局是比较相似的。这样，栅格系统就能够与更多的移动设备相匹配。

原理：

- 把网页总宽度平分12份，开发人员可自由组合。
- 通过定义容器大小，平分12份，结合媒体查询，内外边距调整做出响应式栅格。
- 栅格系统用一系列的行（row）和列（column）组合创建页面布局。

### 10,关于em和rem

`em`作为基本单位时，其换算为 `px` 继承父级元素的字体大小。

`rem` 是相对HTML元素的字体大小来决定的。比如HTML字体为100px,则 `1rem = 100px`

### 11,Flex布局



### 12, overflow原理

## 实践题CSS相关


### 1, css菊花图

考的是自定义 css 动画

```html
<body>
	<div class="loader">Loading...</div>
</body>
```

```css
 body{ background-color: #0dc5c1 }

.loader {
    font-size: 10px;
    margin: 50px auto;
    text-indent: -9999em;
    width: 11em;
    height: 11em;
    border-radius: 50%;
    background: #ffffff;
    background: -moz-linear-gradient(left, #ffffff 10%, rgba(255, 255, 255, 0) 42%);
    background: -webkit-linear-gradient(left, #ffffff 10%, rgba(255, 255, 255, 0) 42%);
    background: -o-linear-gradient(left, #ffffff 10%, rgba(255, 255, 255, 0) 42%);
    background: -ms-linear-gradient(left, #ffffff 10%, rgba(255, 255, 255, 0) 42%);
    background: linear-gradient(to right, #ffffff 10%, rgba(255, 255, 255, 0) 42%);
    position: relative;
    -webkit-animation: load3 1.4s infinite linear;
    /*infinite 表示播放次数:无限*/
    /*linear 表示播放速度:线性*/
    animation: load3 1.4s infinite linear;
    -webkit-transform: translateZ(0);
    -ms-transform: translateZ(0);
    transform: translateZ(0);
}
.loader:before {
    width: 50%;
    height: 50%;
    background: #ffffff;
    border-radius: 100% 0 0 0;
    position: absolute;
    top: 0;
    left: 0;
    content: '';
}
.loader:after {
    background: #0dc5c1;
    width: 75%;
    height: 75%;
    border-radius: 50%;
    content: '';
    margin: auto;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
}
@-webkit-keyframes load3 {
    0% {
        -webkit-transform: rotate(0deg);
        transform: rotate(0deg);
    }
    100% {
        -webkit-transform: rotate(360deg);
        transform: rotate(360deg);
    }
}
/*创建动画,0% 开始的时候， 100%结束的时候*/
@keyframes load3 {
    0% {
        -webkit-transform: rotate(0deg);
        transform: rotate(0deg);
    }
    100% {
        -webkit-transform: rotate(360deg);
        transform: rotate(360deg);
    }
}
```

### 2, CSS3实现环形进度条

### 3, 设置一段文字的大小为6px

### 4, 至少两种方式实现自适应搜索

### 5, 纯css实现三角形

### 6, 实现自适应的正方形:





