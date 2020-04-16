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