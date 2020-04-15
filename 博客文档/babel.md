# Babel7.x快速上手使用指南

> 在刚开始使用babel的时候，相信很多同学应该和我一样，对于babel的使用配置一知半解，babel相关的包@babel/core，@babel/cli，babel-loader，@babel/polyfill，@babel/plugin-transform-runtime，@babel/runtime何时引入又有什么作用和区别会感到疑惑。更深入了解babel的推荐官方的文档：[中文](https://www.babeljs.cn/)，[英文](https://babeljs.io/)。
>
> **说明：本文使用babel版本为7，webpack版本为4，不同版本安装和配置存在差异。**

## 使用方式一@babel/cli

@babel/cli 是 babel 提供的命令行工具，用于命令行（npx babel xx.js）下编译源代码。

首先需要安装 @babel/cli 和 @babel/core（核心包）

```
npm install --save-dev @babel/core @babel/cli
```

现在script.js 是用来es6的箭头函数和let

```
//script.js
let fun = () => console.log('hello babel.js')
```

我们试试运行 `npx babel script.js`

```
$ npx babel script.js
let fun = () => console.log('hello babel.js');
```

发现还是原来的代码，没有任何变化。说好的编译呢？

这个调整则是在 [babel 6](https://babeljs.io/blog/2015/10/29/6.0.0) 里发生的。Babel 6 做了大量模块化的工作，将原来集成一体的各种编译功能分离出去，独立成插件。这意味着，默认情况下，当下版本的 babel 不会编译代码。接下来我们使用插件

\##babel 插件

我们要将上面的箭头函数编译成 ES5 函数，需要安装额外的 babel 插件。

首先，在之前基础上，我们安装 [@babel/plugin-transform-arrow-functions：](https://github.com/babel/babel/tree/master/packages/babel-plugin-transform-arrow-functions)

```
npm install --save-dev @babel/plugin-transform-arrow-functions
```

然后，在命令行编译时指定使用该插件验证下:

```
$ npx babel script.js --plugins @babel/plugin-transform-arrow-functions
let fun = function () {
  return console.log('hello babel.js');
};
```

编译成功

构建成文件： `npx babel script.js -d dist` 会将script.js文件编译好输出到dist

 

## 使用方式二webpack中使用

需要 `npm i @babel/core babel-loader -D`核心包和webpack的loader,然后根据webpack的模块化配置，

[参照配置](https://github.com/wusp1994/h5-webpack4.x/blob/master/build/webpack.base.config.js#L18-L25)

## 配置

随着各种新插件的加入，我们的命令行参数会越来越长。这时，我们可以使用配置文件的方式代替命令行参数
配置文件的方式有以下几种：  

- 1，在package.json中设置babel字段。
- 2，.babelrc文件或.babelrc.js
- 3，babel.config.js文件

 下面我们分别用几种方式，实现编译箭头函数

#### 配置方式一，在package.json中设置babel字段。

 首先，不用创建文件，package.json加入babel的配置信息就行。

 //package.json

```
{
  "name": "babelDemo",
  "version": "1.0.0",
  "description": "babel7使用教程验证",
  "author": "wusp1994",
  "private": true,
  "devDependencies": {
    "@babel/cli": "^7.7.5",
    "@babel/core": "^7.7.5",
    "@babel/plugin-transform-arrow-functions": "^7.7.4"
  },
  "babel": {
    "plugins": [
      "@babel/plugin-transform-arrow-functions"
    ]
  }
}
```

之后，在命令行只要运行 `npx babel script.js` 即可，`babel` 会自动读取 `package.json` 里的配置并应用到编译中：

```
$ npx babel script.js
let fun = function () {
  return console.log('hello babel.js');
};
```

编译成功

#### 配置方式二，在项目根目录下创建babelrc或者.babelrc.js

babelrc和.babelrc.js是同一种配置方式，只是文件格式不同，一个是json文件，一个是js文件。  

我们把之前pack.json 中的babel配置删掉，然后新建文件 .babelrc

```
{
  "plugins": [
    "@babel/plugin-transform-arrow-functions"
  ]
}
```

.babelrc.js

```
//webpack的配置文件也是这种写法
module.exports = {
  "plugins": [
    "@babel/plugin-transform-arrow-functions"
  ]
};
```

之后，在命令行只要运行 `npx babel script.js` 即可，`babel` 会自动读取 `babelrc或者.babelrc.js` 里的配置并应用到编译中：

```
$ npx babel script.js
let fun = function () {
  return console.log('hello babel.js');
};
```

编译成功

#### 配置方式三，在项目根目录下创建babel.config.js

这种方式写法和.babelrc.js一样，但是babel.config.js是针对整个项目，一个项目只有一个放在项目根目录。

##### 注意1：`.babelrc`文件放置在项目根目录和 `babel.config.js`效果一致，如果两种类型的配置文件都存在，.babelrc会覆盖babel.config.js的配置。

##### 注意2：在package.json里面写配置还是创建配置文件都没有什么区别，看个人习惯。react官方脚手架create-react-app创建的react项目babel配置是写在package.json里面的，而vue官方脚手架@vue/cli创建的vue项目，则是通过babel.config.js设置。

## Plugins和Presets

Plugins 和 Presets 是使用babel插件的两种方式。有了配置文件，接下来就是要通过配置文件告诉babel编译哪些内容，引入对应的编译插件（Plugins）。拿箭头函数举例，箭头函数 @babel/plugin-transform-arrow-functions 这个插件。

##### Plugins方式

先安装插件包 @babel/plugin-transform-arrow-functions

> npm i @babel/plugin-transform-arrow-functions -D

然后配置插件

```
// babel.config.js
module.exports = {
    plugins: ['@babel/plugin-transform-arrow-functions']
};
```

现在我们的箭头函数就能编译成es2015了，但是我们可能还需要编译 class（`@babel/plugin-transform-classes`）等很多，每个都对应一个。不便于我们管理，所以就有了 presets 预设（[@babel/preset-env](https://babeljs.io/docs/en/babel-preset-env/)）

##### Presets(@babel/preset-env)方式

@babel/preset-env 其实就是把 babel 插件打包到一起，让你可以使用配置.babelrc等来管理使用插件，而不是一个一个去安装到pack.json中使用。 我们来看看怎样使用 @babel/preset-env。

首先安装 @babel/preset-env，在此基础上你需要有babel核心包 @babel/core

```
npm install --save-dev @babel/preset-env @babel/core
```

然后修改 .babelrc：

```
{
  "presets": ["@babel/preset-env"]
}
```

官方文档说，默认情况下 @babel/preset-env 已经包含了三个插件集合 babel-preset-es2015, babel-preset-es2016, and babel-preset-es2017 。 而我们上面的转化箭头函数的插件已经包含在内了。而我们需要 **通过控制配置文件中的配置浏览器列表** 来实现，箭头函数、promise、let、await等等编译。
下面附上 浏览器列表配置

```
{
  "presets" : [
    ["@babel/preset-env",{
      "targets": {
        "browsers": [
          "last 2 versions",
          "not ie <= 9"
        ]
      },
      "modules": false,
      "debug": false,
      "useBuiltIns": "usage"
    }]
  ]
}
```

**值得提一句的上，如果该浏览器 已经支持箭头函数、class、const，所以 babel 在编译过程中，不会编译它们。**

## @babel/runtime&@babel/plugin-transform-runtime

Object.assign 为例，
我们知道，IE 11 不支持 Object.assign，此时，我们有俩种候选方案：

- 1,引入 babel-polyfill，补丁一打，Object.assign 就被创造出来
- 2,配置 @babel/plugin-transform-object-assign

当采用第二种方案的时候 在script.js中加入

```
Object.assign({}, {})
```

安装

> npm i @babel/plugin-transform-object-assign  

配置.babelrc

```
{
  "presets" : [
    ["@babel/preset-env",{
      "targets": {
        "browsers": [
          "last 2 versions",
          "not ie <= 9"
        ]
      },
      "modules": false,
      "debug": false,
      "useBuiltIns": "usage"
    }]
  ],
+ "plugins":["@babel/plugin-transform-object-assign"]
}   
```

运行 `npx babel script.js` ,babel 会将所有的 Object.assign 替换成 `_extends` 这样一个辅助函数。

```
import "core-js/modules/es6.object.assign";

function _extends() { _extends = Object.assign || function (target) { for (var i = 1; i < arguments.length; i++) { var source = arguments[i]; for (var key in source) { if (Object.pro
totype.hasOwnProperty.call(source, key)) { target[key] = source[key]; } } } return target; }; return _extends.apply(this, arguments); }

// let fun = ()=> console.log('hello babel.js')
_extends({}, {});
```

如果我们有一百个文件，其中有 50 个文件里写了 Object.assign，那么，坏消息来了，`_extends` 辅助函数会出现 50 次。

对于这种工具函数/辅助函数，我们一般的做法是分离、引入、复用。`@babel/runtime`的作用就是封装了这些工具函数。 使用如下

```
var _extends = require("@babel/runtime/helpers/extends");
_extends({}, {});
```

但是，手动导入，如此还是麻烦，于是 babel 提供了 `@babel/plugin-transform-runtimes` 来替我们做这些转换。

##### `@babel/plugin-transform-runtimes` 如何使用：

plugin-transform-runtime主要是负责将工具函数转换成引入的方式，减少重复代码.

> 注意:babel/runtime并不是开发依赖，而是项目生产依赖。编译时使用了plugin-transform-runtime，你的项目就要依赖于babel/runtime，所以这两个东西是一起使用的

安装  `@babel/plugin-transform-runtimes`（开发依赖）和`@babel/runtime`(生产依赖)

> npm install --save-dev @babel/plugin-transform-runtime
> npm install @babel/runtime

在.babelrc配置

```
{
  "plugins": [
    "@babel/plugin-transform-object-assign",
    "@babel/plugin-transform-runtime"
  ]
}
```

然后运行 编译命令

> npx babel script.js

输出如下：

```
import _extends from "@babel/runtime/helpers/extends";
_extends({}, {});
```

这样的结果跟我们，期望手动导入的结果是一致的,但我们并没有手动导入。

## babel-polyfill

babel-polyfill则是引入相关文件模拟兼容环境.

> babel可以转化一些新的特性，但是对于新的内置函数（Promise,Set,Map），静态方法（Array.from,Object.assign），实例方法（Array.prototype.includes）这些就需要babel-polyfill来解决，babel-polyfill会完整模拟一个 ES2015+环境。

比如你的代码中用到了Array.from，但是目标浏览器不支持Array.from，引入babel-polyfill就会给Array添加一个from方法，代码执行的时候使用的Array.from其实是babel-polyfill模拟出来功能，这样虽然污染了Array的静态方法，但是确实实现了兼容。

之前的使用方式：npm安装@babel/polyfill，并在项目入口处引入@babel/polyfill包。

```
npm i @babel/polyfill  
```

入口文件

```
// entry.js
import "@babel/polyfill";
```

但是这种方式已经被废弃不推荐使用，因为`@babel/polyfill`体积比较大，整体引入既增加项目体积，又污染了过多的变量，所以更推荐是配合 @babel/preset-env 的 useBuiltIns(按需引入) 配置。

```
// babel.config.js
module.exports = {
    presets: [
        [
            '@babel/preset-env',
            {
                useBuiltIns: 'usage', // usage-按需引入; entry-入口引入（整体引入）; false-不引入polyfill;
                corejs: 2  // 2-corejs@2  3-corejs@3
            }
        ]
    ]
};
```

corejs 是一个给低版本的浏览器提供接口的库，也是polyfill功能实现的核心，此处指定的是引入corejs的版本，需要通过npm安装指定版本的`core-js`库作为生产依赖。如果不配置项目`build`的时候会报警告，默认是2版本

在script.js中加入代码

```
const a = Array.from([1])
```

运行命令编译

> npx babel script.js

编译结果

```
import "core-js/modules/es6.string.iterator";
import "core-js/modules/es6.array.from";

var a = Array.from([1]);
```

可以看到在使用Array.from之前，提前从core-js引入了相应的polyfill，根据文件名，我们大概猜到它们的功能是什么。如果不使用 @babel/polyfill，这些文件需要你手动写了。

## @babel/polyfill与@babel/runtime 的区别？

babel-polyfill 与 babel-runtime 的一大区别，前者改造目标浏览器，让你的浏览器拥有本来不支持的特性；后者改造你的代码，让你的代码能在所有目标浏览器上运行，但不改造浏览器。

如果你还是困惑，我推荐一个非常简单的区分方法 - 打开浏览器开发者工具，在 console 里执行代码：

- 引入 babel-polyfill 后的 IE 11，你可以在 console 下执行 Object.assign({}, {})
- 而引入 babel-runtime 后的 IE 11，仍然提示你：Object doesn't support property or method 'assign';

babel-polyfill有一个问题就是引入文件会污染变量，其实plugin-transform-runtime也提供了一种runtime的polyfill。 首先修改配置文件，随意选择方式

```
module.exports = {
    plugins: [['@babel/plugin-transform-runtime', { corejs: 2 }]]
};
```

然后，安装包.这个地方的corejs是指定了一个叫runtime-corejs库的版本，使用时也需要用npm安装对应的包。

> npm i @babel/runtime-corejs2

执行编译

> npx babel script.js

编译后

```
import  _Array$from from "@babel/runtime-corejs2/core-js/array/from";
var a = _Array$from([1]);
```

 

此方法和使用babel-polyfill的区别是，并没有改变Array.from,而是创建了一个 `_Array$from`来模拟Array.from的功能，调用Array.from会被编译成调用 `_Array$from`，这样做的好处很明显就是不会污染Array上的静态方法from，plugin-transform-runtime提供的runtime形式的polyfill都是这种形式。

经过我的测试，除了实例上的方法如Array.prototype.includes这种的，其它之前提到的内置函数（Promise,Set,Map），静态方法（Array.from,Object.assign）都可以采用plugin-transform-runtime的这种形式。

> runtime 不污染全局变量，但是会导致多个文件出现重复代码。 写类库的时候用runtime，系统项目还是用polyfill。 写库使用 runtime 最安全，如果我们使用了 includes，但是我们的依赖库 B 也定义了这个函数，这时我们全局引入 polyfill 就会出问题：覆盖掉了依赖库 B 的 includes。如果用 runtime 就安全了，会默认创建一个沙盒,这种情况 Promise 尤其明显，很多库会依赖于 bluebird 或者其他的 Promise 实现,一般写库的时候不应该提供任何的 polyfill 方案，而是在使用手册中说明用到了哪些新特性，让使用者自己去 polyfill。

 

**该用哪种形式是看项目类型了，不过通常对于一般业务项目来说，还是plugin-transform-runtime处理工具函数，babel-polyfill处理兼容。**

## 总结

发依赖 npm i --save-dev

生产依赖 npm i

| **包名**                        | **功能**                          | **说明**                                                     | 依赖包                                      |
| :------------------------------ | :-------------------------------- | :----------------------------------------------------------- | :------------------------------------------ |
| @babel/core                     | babel编译核心包                   | 必装开发依赖 --save-dev                                      |                                             |
| @babel/cli                      | babel 提供的命令行工具，npx babel | 非必装开发依赖，webpack不需要装                              | @babel/core                                 |
| babel-loader                    | webpack的loader,允许babel传输文件 | 非必装开发依赖，webpack项目中使用                            | @babel/core                                 |
| @babel/plugin-*                 | babel 插件                        | 开发依赖，按照需要的功能安装                                 | @babel/core                                 |
| @babel/preset-*                 | 插件预设,理解为插件套餐           | 开发依赖，js语言新特性转换推荐使用preset-env                 | @babel/core                                 |
| @babel/plugin-transform-runtime | 复用工具函数                      | 非必装开发依赖，和@babel/runtime同时存在                     | @babel/core,@babel/runtime                  |
| @babel/runtime                  | 工具函数库                        | 非必装生产依赖，和@babel/plugin-transform-runtime同时存在    | @babel/plugin-transform-runtime,@babel/core |
| @babel/polyfill                 | 低版本浏览器兼容库                | 非必装生产依赖，已不推荐使用，推荐通过preset-env的useBuiltIns属性按需引入 | @babel/core,@babel/preset-env               |
| core-js@*                       | 低版本浏览器兼容库                | 非必装生产依赖，通过preset-env引入polyfill需安装此包，并通过corejs指定版本 | @babel/core,@babel/preset-env               |
| @babel/runtime-corejs*          | 不污染变量的低版本浏览器兼容库    | 非必装生产依赖，plugin-transform-runtime设置开启后，可以不污染变量的引入polyfill | @babel/core,@babel/plugin-transform-runtime |

 