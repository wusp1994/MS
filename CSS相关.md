## CSS相关

###  1. 块级元素与行内元素

块级元素: 默认情况下，块级元素会新起一行。[MDN地址](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Block-level_elements)

- div , canvas,  h1,  h2,   p,  ul,  ol,  form,  table (常用)


行内元素: 一个行内元素只占据它对应标签的边框所包含的空间。

- `b ，i , a , span , input , button , label , code , tt , select , textarea , img , strong` 等(常用)

###  2. 水平居中

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

- （不定宽）方式二inline-block:  父级 ，`text-align: center;` 当前元素：`display: inline-block;`

- (不定宽)方式三flex：父级: `display:flex; justify-content:center:`

- （定宽）方式四 绝对定位：父级：`position: relative;`当前元素：`position:absolute; top:0; left: calc(50% - 50px);`50% 减去自身宽度。

### 3. 垂直居中

**单行行内元素：** `line-height`等于盒子 `height`"，即行高等于盒子的高"

**多行行内元素：** 父元素设置 `display:table-cell`;和`vertical-align: middle`;

**块级元素：**

- （不用定高）**flex**：父级：`display: flex; align-item: center;`

- （定高）绝对定位 : 父级：`position: relative;`当前元素：`position:absolute; top:calc(50% - 50px); left: 0`  50% 减去自身高度。

- （不用定高）transform：父级：`position: relative;`当前元素：`position:absolute; top:50%; transform: translateY(- 50%)



### 4,BFC优化

BFC全称”Block Formatting Context”, 中文为“块级格式化上下文”。  
特性:

- 内部子元素不会影响外部元素。

- 流体特点，水平方向自动填满容器。


### 5, css菊花图



4. **CSS3实现环形进度条**

5. **css优先级/权重**

6. **关于vh, vw**

7. **设置一段文字的大小为6px**

8. **至少两种方式实现自适应搜索**

9. **纯css实现三角形**

10. **盒模型哪两种模式？什么区别？如何设置**

11. **常用清除浮动的方法，如不清除浮动会怎样？**

12. **删格化的原理**

13. **高度不定，宽100%，内一p高不确定，如何实现垂直居中？**

14. **关于em和rem**

15. **Flex布局**

16. **overflow原理**

17. **实现自适应的正方形:**





