## HTML相关

#### 1，XHTML和HTML有什么区别？
HTML是一种基本的web网页设计语言，XHTML是基于XML的标记语言。
主要的不同点： 
XHTML元素必须正确的嵌套 
XHTML元素必须关闭，标签名必须用小写 
XHTML文档必须拥有根元素

#### 2，前端页面有哪三层？分别是什么？作用是什么？
结构层html、显示层css、行为层js
html的标签对网页的内容的语义含义做出描述，但是不包含任何有关内容的信息。比如P标签表达：“这是一个文本段”。
CSS对标签进行渲染
js根据事件对DOM进行操作。Js事件：鼠标，键盘，窗口，用户改变区域内容。

#### 3，你做的页面在哪些流览器测试过?这些浏览器的内核分别是什么?
IE（ie内核）、火狐（Gecko 读音：该扣）谷歌（webkit）Opera（Persto读音：普弱斯头）Safari（webkit）

#### 4，什么是语义化得HTML？
html语义化就是使用相应的语义标签。比如p标签表示段落，h1，h2表示标题等等。

优点：

- 便于浏览器和搜索引擎分析，利于SEO
- 使页面结构化，便于开发阅读

#### 5，HTML5为什么只需要些！DOCTYPE HTML?
HTML5不基于SGML(标记语言),因此不需要DTD进行引用，但是需要doctype来规范浏览器的行为。而html4.01基于SGML，所以需要DTD进行引用，才能告诉浏览器文档所使用的文档类型。

#### 6，Doctype作用？标准模式和兼容模式有什么区别？
作用：
！Doctype声明位于HTML文档中的第一行，处于html标签之前。告知浏览器解析器用什么文档标准解析这个文档。Doctype不存在或格式不正确会导致文档以兼容模式呈现。
区别：
标准模式中页面的排版和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松向后兼容的方式显示，模仿老式浏览器的行为防止页面无法正常工作。

#### 7，Html5有哪些新特性，移除了哪些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分HTML和HTML5?
HTML5现在已经不是SGML的子集，主要是关于图像，位置，存储，多任务等功能的增加。
新增：
    绘画canvas
    用于媒介回放的video和audio元素
    本地离线存储localStorage长期存储数据，浏览器关闭后数据不丢失；
    sessionStorage的数据在浏览器关闭后自动删除
    内容元素：article（文章，读：阿提扣）、footer、header、nav、section（部分，读：赛克新）
    表单控件：calendar、date、time、email、url、search
    控件元素：webworker、websocket、Deolocation
移除：
    纯表现元素：basefont、big、center、font、s、strike、tt、u、
    性能较差的元素：frame、frameset、noframes
兼容：
    1.IE8/IE7/IE6支持通过document.createElement方法产生的标签。利用这一特性支持HTML5标签。
    2.使用html5shim框架
区分：根据DOCTYPE声明的方式区分。根据新增的标签区分。

#### 8，cookies、sessionStorage 和localStorage的区别？
cookie在浏览器和服务器间来回传递。sessionStorage 和 localStorage不会
    sessionStorage和localStorage的存储空间更大
    sessionStorage和localStorage有更丰富易用的接口
    sessionStorage和localStorage各自独立的存储空间

localStorage长期存储数据，浏览器关闭后数据不丢失；
sessionStorage的数据在浏览器关闭后自动删除

#### 9，实现浏览器内多个标签页之间的通信？
调用localStorage、cookie等本地存储方式