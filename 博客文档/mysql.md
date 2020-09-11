## MySQL 数据库搭建

### 安装 MySQL

地址：https://dev.mysql.com/downloads/mysql/

### 安装 Navicat

地址：https://www.navicat.com.cn/products

### 启动 MySQL

windows 同学参考：https://blog.csdn.net/ycxzuoxin/article/details/80908447

mac 同学参考：https://blog.csdn.net/qq_25628891/article/details/88431942

```bash
cd /usr/local/mysql-8.0.13-macos10.14-x86_64/bin
./mysqld
```



### 初始化数据库

创建数据库 book，下载 book.sql：

https://www.youbaobao.xyz/resource/admin/book.sql

执行 book.sql 导入数据

# mysql

## 安装

安装 mysql 库：

```bash
 npm i -S mysql
```



## 配置

创建 db 目录，新建两个文件：

```bash
index.js
config.js
```



config.js 源码如下：

```js
module.exports = {
  host: 'localhost',
  user: 'root',
  password: '12345678',
  database: 'book'
}
```



## 连接

连接数据库需要提供使用 mysql 库的 createConnection 方法，我们在 index.js 中创建如下方法：

```js
function connect() {
  return mysql.createConnection({
    host,
    user,
    password,
    database,
    multipleStatements: true
  })
}
```

> multipleStatements：允许每条 mysql 语句有多条查询.使用它时要非常注意，因为它很容易引起 sql 注入（默认：false）

## 查询

查询需要调用 connection 对象的 query 方法：

```js
function querySql(sql) {
  const conn = connect()
  debug && console.log(sql)
  return new Promise((resolve, reject) => {
    try {
      conn.query(sql, (err, results) => {
        if (err) {
          debug && console.log('查询失败，原因:' + JSON.stringify(err))
          reject(err)
        } else {
          debug && console.log('查询成功', JSON.stringify(results))
          resolve(results)
        }
      })
    } catch (e) {
      reject(e)
    } finally {
      conn.end()
    }
  })
}
```



我们在 constant.js 创建一个 debug 参数控制日志打印：

```js
const debug = require('../utils/constant').debug
```



> 这里需要注意 conn 对象使用完毕后需要调用 end 进行关闭，否则会导致内存泄露

调用方法如下：

```js
db.querySql('select * from book').then(result => {
  console.log(result)
})
```