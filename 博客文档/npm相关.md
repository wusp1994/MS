## npm安装换源

#### 安装的时候换源

npm install --registry=https://registry.npm.taobao.org

#### cnpm换源

npm install -g cnpm --registry=https://registry.npm.taobao.org

#### set config换源

npm config set registry https://registry.npm.taobao.org

npm config get registry #获取配置

## node-sass安装太慢或失败

```powershell
#设置变量 sass_binary_site，指向淘宝镜像地址
npm i node-sass --sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
#或设置全局镜像源
npm config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/
```

**下载 .node 到本地**

到这里去根据版本号、系统环境，选择下载 .node 文件：

https://github.com/sass/node-sass/releases

然后安装时，指定变量 sass_binary_path为本地地址，如：

```powershell
npm i node-sass --sass_binary_path=/Users/lzwme/Downloads/darwin-x64-48_binding.node
```

## yarn常用的命令

现在 Yarn 已经 [安装完毕](https://yarn.bootcss.com/docs/install)，可以开始使用了。 以下是一些你需要的最常用的命令：

**初始化一个新项目**

```
yarn init
```

**添加依赖包**

```
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

**将依赖项添加到不同依赖项类别中**

分别添加到 `devDependencies`、`peerDependencies` 和 `optionalDependencies` 类别中：

```
yarn add [package] --dev
yarn add [package] --peer
yarn add [package] --optional
```

**升级依赖包**

```
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

**移除依赖包**

```
yarn remove [package]
```

**安装项目的全部依赖**

```
yarn --registry=https://registry.npm.taobao.org
```

或者

```
yarn install --registry=https://registry.npm.taobao.org
```