## Node.js

node.js 是一个基于 Chrome JavaScript 运行时建立的平台，用于方便搭建相应快速、易于扩展的网络应用，Node.js使用**事件驱动**，**非阻塞I/O**模型而得以轻量高效，非常适合在分布式设备上运行的数据密集的实时应用。

node.js 的特点

- 事件驱动
- 非阻塞I/O
- 事件轮询机制

优点：适合大量的I/O计算

缺点：不适合密集的cpu计算



#### 事件驱动

事件驱动模型的三大要素：

**事件源:** 能够接受外部事件的源体。

**侦听器：** 能够接受事件源通知的对象。

**事件处理程序：**用于处理事件的对象。



#### 非阻塞I/O

**阻塞调用**是指调用结果返回之前，当前线程会被挂起。调用线程只有在得到结果之后才能返回。

**非阻塞调用**是指在不能立刻得到结果之前，该调用不会阻塞当前线程。



#### 事件轮询机制Event Loop

一个事件轮询拥有下面三个组件

**事件队列：** 这是一个FIFO模型的队列，一方推入事件，一方推出事件。

**队列的读取轮询线程组件**，也就是主角Event Loop

**单独的线程池**：用来执行长任务（thread Pool, node底层，用c++写的，不会阻塞）

![img](http://n.sinaimg.cn/mobileh5/01345b8f/20170214/96ea1c33gy1fcl7bdc7pxj20f807k745.jpg)

`Nodejs`中只有一个主线程,也就是单线程来不断读取轮询（也是`调用I/O观察者`）队列中是否有事件。而对于读取文件，http请求等比较容易堵塞的事件，在这个单线程中执行肯定会容易造成堵塞，所以Event Loop会把这类型的事件交给底层的线程池执行，并给予线程池一个回调函数，当线程池操作完成这阻塞任务后，便把结果和回调函数一起在放入轮询队列中。当单线程从队列中不断读取事件，读取到这些堵塞的操作结果后，会将这些操作结果作为回调函数的输入参数，然后激活运行回到函数。

> Node.js的这个单线程不只是负责读取队列事件，还会执行运行回调函数。



## express三大基础概念

### 中间件

中间件是一个函数，在请求和响应周期中被顺序调用。

```js
function myLogger(req,res,next){
    console.log("myLogger")
    next()
}
app.use(myLogger)
```



> 中间需要在响应结束前被调用。

### 路由

应用如何响应请求的一种规则

响应 '/' 路径的get请求

```js
app.get('/',function(req,res){
    res.send("hello node")
})
```

响应 '/' 路径的post请求

```js
app.post('/',function(req,res){
    res.send("hello node")
})
```

规则主要分为两部分：

- 请求方法： get，post 等
- 请求的路径：/ 、/user 、/.*fly$/ ......

### 异常处理

通过自定义异常处理中间件处理请求中产生的异常

```js
app.get('/',function(req,res){
    throw new Error("somethings has error...")
})

const errorHandler = function(err,req,res,next){
    console.log("errorHandler...")
    res.status(500)
    res.send('down...')
}

app.use(errorHandler)
```



> 使用时需要注意两个点
>
> - 第一：参数一个都不能少，否则会视为普通的中间件
> - 第二：中间件需要在请求之后引用



## express项目搭建优化

### 路由

安装boom依赖:主要用来处理异常

```shell
npm i -s boom
```



创建 router 文件夹，创建router/index.js

```js
const express = require('express')
const boom = require('boom')
const userRouter = require('./user')
const bookRouter = require('./book')
const jwtAuth = require('./jwt')
const Result = require('../models/Result')

// 注册路由
const router = express.Router()

router.use(jwtAuth)

router.get('/', function(req, res) {
  res.send('欢迎学习小慕读书管理后台')
})

router.use('/user', userRouter)

router.use('/book', bookRouter)

/**
 * 集中处理404请求的中间件
 * 注意：该中间件必须放在正常处理流程之后
 * 否则，会拦截正常请求
 */
router.use((req, res, next) => {
  next(boom.notFound('接口不存在'))
})

/**
 * 自定义路由异常处理中间件
 * 注意两点：
 * 第一，方法的参数不能减少
 * 第二，方法的必须放在路由最后
 */
router.use((err, req, res, next) => {
  console.log(err)
  if (err.name && err.name === 'UnauthorizedError') {
    const { status = 401, message } = err
    new Result(null, 'Token验证失败', {
      error: status,
      errMsg: message
    }).jwtError(res.status(status))
  } else {
    const msg = (err && err.message) || '系统错误'
    const statusCode = (err.output && err.output.statusCode) || 500;
    const errorMsg = (err.output && err.output.payload && err.output.payload.error) || err.message
    new Result(null, msg, {
      error: statusCode,
      errorMsg
    }).fail(res.status(statusCode))
  }
})

module.exports = router

```

创建 router/user.js

```js
const express = require("express")
const router = express.Router()
router.get("/info",function(req,res,next){
    res.json("user info ...")
})

module.exports = router
```

验证 /user/info：

```js
module.exports = {
  CODE_ERROR: -1
}
```

验证 /user/login：

```json
{"code":-1,"msg":"接口不存在","error":404,"errorMsg":"Not Found"}
```