## CSS相关

1. #### 居中

1.margin: 0 auto;定宽。--水平  
2.text-align: center; display:inline-block; -- 水平  
3.行高等于高度，-- 垂直  
4.绝对定位，50%减自身宽高。--垂直居中  
5.绝对定位，上下左右全0，margin:auto；--垂直居中  
6, 父级盒子(垂直居中) 不需要知道宽高  

```css
.box{
    display: flex;
    justify-content: center;
    align-items: center;
}   
```

7, 父级元素，(垂直居中)

```css
.box{
    display: table-cell;
    vertical-align: middle;
}
```

子元素（水平居中）

```css
.child{
    margin: 0 auto;
    width: 100px;
}
```



2. #### BFC优化

BFC全称”Block Formatting Context”, 中文为“块级格式化上下文”。特性:

- 内部子元素不会影响外部元素。
- 流体特点，水平方向自动填满容器。

