## NodeJS相关

### 1.node的全局对象global

 JavaScript 中，通常 window 是全局对象， 而 Node.js 中的全局对象是 global，所有全局变量（除了 global 本身以外）都是 global 对象的属性。

**常用的全局变量**：

**__filename**  :表示当前正在执行的脚本的文件名。它将输出文件所在位置的绝对路径，且和命令行参数所指定的文件名不一定相同。 如果在模块中，返回的值是模块文件的路径。

main.js

```js
// 输出全局变量 __filename 的值
console.log( __filename );
```

```shell
$ node main.js
/web/com/runoob/nodejs/main.js
```

**__dirname：** 表示当前执行脚本所在的目录。

main.js

```js
// 输出全局变量 __dirname 的值
console.log( __dirname );
```

```sh
$ node main.js
/web/com/runoob/nodejs
```

**process**:它用于描述当前Node.js 进程状态的对象，提供了一个与操作系统的简单接口。

​			process常用属性：

- process.env   返回一个对象，成员为当前 shell 的环境变量

- process.config,  它与运行 ./configure 脚本生成的 "config.gypi" 文件相同。
- process.pid, 当前进程的进程号。

**setTimeout(cb, ms)**

**clearTimeout(t)**

**setInterval(cb, ms)**

**console**



### 2，AMD ， CMD ，CommonJS 和 ES6的对比

`AMD`：是RequireJS**在推广过程中**对模块定义的规范化**产出，它是一个概念，RequireJS是对这个概念的实现，就好比JavaScript语言是对ECMAScript规范的实现。AMD是一个组织，RequireJS是在这个组织下自定义的一套脚本语言。

`RequireJS`：是一个AMD框架，可以异步加载JS文件，按照模块加载方法，通过define()函数定义，第一个参数是一个数组，里面定义一些需要依赖的包，第二个参数是一个回调函数，通过变量来引用模块里面的方法，最后通过return来输出。

是一个依赖前置、异步定义的AMD框架（在参数里面引入js文件），在定义的同时如果需要用到别的模块，在最前面定义好即在参数数组里面进行引入，在回调里面加载。

`CMD`:   是**SeaJS**在推广过程中对模块定义的规范化产出，是一个同步模块定义，是SeaJS的一个标准，SeaJS是CMD概念的一个实现，SeaJS是淘宝团队提供的一个模块开发的js框架。

`CommonJS规范`---是通过module.exports定义的，在前端浏览器里面并不支持module.exports,通过node.js后端使用的。Nodejs端是使用CommonJS规范的，前端浏览器一般使用AMD、CMD、ES6等定义模块化开发的。

`ES6特性，模块化`---export/import对模块进行导出导入的。



**AMD规范与CMD规范的区别是什么？**

