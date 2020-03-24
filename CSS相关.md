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

BFC全称”Block Formatting Context”, 中文为“块级格式化上下文”。  
特性:
- 内部子元素不会影响外部元素。

- 流体特点，水平方向自动填满容器。

  

3. #### **css菊花图**

4. **CSS3实现环形进度条**

5. **css优先级/权重**

6. **关于vh, vw**

7. **设置一段文字的大小为6px**

8.  **至少两种方式实现自适应搜索**

9.  **纯css实现三角形**

10. **盒模型哪两种模式？什么区别？如何设置**

11. **常用清除浮动的方法，如不清除浮动会怎样？**

12. **删格化的原理**

13. **高度不定，宽100%，内一p高不确定，如何实现垂直居中？**

14. **关于em和rem**

15.  **Flex布局**

16.  **overflow原理**

17. **实现自适应的正方形:**





