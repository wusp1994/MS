## Nginx 服务器搭建

### 安装 nginx

- windows 通过下载官网安装包，下载地址：http://nginx.org/en/download.html
- mac 通过 brew 安装，参考：https://www.jianshu.com/p/c3294887c6b6

WARNING

使用 macOS 的同学可能会碰到无法写入 /usr/local 问题，后面会提供解决方法

### 修改配置文件

打开配置文件 nginx.conf：

- windows 位于安装目录下
- macOS 位于：`/usr/local/etc/nginx/nginx.conf`

修改一：添加当前登录用户为owner

```bash
user sam owner;
```



修改二：在结尾大括号之前添加：

```bash
include /Users/sam/upload/upload.conf;
```



这里 `/Users/sam/upload` 是资源文件路径，`/Users/sam/upload/upload.conf` 是额外的配置文件，当前把 `/Users/sam/upload/upload.conf` 配置文件的内容加入 nginx.conf 也是可行的！

WARNING

使用 windows 的同学可能会碰到路径配置错误导致 500 的情况，最后一节有解法

修改三：添加 `/Users/sam/upload/upload.conf` 文件，配置如下：

```bash
server
{ 
  charset utf-8;
  listen 8089;
  server_name http_host;
  root /Users/sam/upload/;
  autoindex on;
  add_header Cache-Control "no-cache, must-revalidate";
  location / { 
    add_header Access-Control-Allow-Origin *;
  }
}
```



如果需要加入 https 服务，可以再添加一个 server：

```bash
server
{
  listen 443 default ssl;
  server_name https_host;
  root /Users/sam/upload/;
  autoindex on;
  add_header Cache-Control "no-cache, must-revalidate";
  location / {
    add_header Access-Control-Allow-Origin *;
  }
  ssl_certificate /Users/sam/Desktop/https/book_youbaobao_xyz.pem;
  ssl_certificate_key /Users/sam/Desktop/https/book_youbaobao_xyz.key;
  ssl_session_timeout  5m;
  ssl_protocols  SSLv3 TLSv1;
  ssl_ciphers  ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
  ssl_prefer_server_ciphers  on;
}
```



- https证书：/Users/sam/Desktop/https/book_youbaobao_xyz.pem
- https:私钥：/Users/sam/Desktop/https/book_youbaobao_xyz.key

### 启动服务

启动 nginx 服务：

```bash
sudo nginx
```



重启 nginx 服务：

```bash
sudo nginx -s reload
```



停止 nginx 服务：

```bash
sudo nginx -s stop
```



检查配置文件是否存在语法错误：

```bash
sudo nginx -t
```



访问地址：

- http: `http://localhost:8089`
- https: `https://localhost`

> https 会提示证书有风险，不用理会，直接选择继续访问即可

### 资源文件

资源文件下载地址：

https://pan.baidu.com/s/1x2N7vl8nd2x6x7FnlQH3Cg#list/path=%2F

提取码：ksjv

将 epub 和 epub2 目录放入 `/Users/sam/upload/`

### 常见问题

#### 解决 macOS operation not permitted 问题

macOS 从 El Capitan（10.11）后加入了 Rootless 机制，很多系统目录不再能够随心所欲的读写了，即使设置 root 权限也不行，解决方法：

重启按住 Command+R，进入恢复模式，打开 Terminal：

```bash
csrutil disable
```



之后再次进入系统就可以获得修改 /usr 的写入权限了，打开 csrutil 方法是进入恢复模式，在 Terminal 中：

```bash
csrutil enable
```



#### 解决 Windows 同学路径配置错误启动出现 500 异常

windows 中不允许在 nginx 配置文件中出现转义字符，比如 `\resource` 这样的路径会被编译为：`esrouce`，从而导致 nginx 启动异常，我们可以更换文件夹名称来解决这个问题。

#### 如何申请 https 证书

阿里云提供免费的 https 证书申请，首先需要申请域名，并完成域名实名认证