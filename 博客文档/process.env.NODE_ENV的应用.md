## process.env.NODE_ENV的应用

### vue-cli 3中

根目录下配置文件

`.env.development` 模式用于serve，开发环境，就是开始环境的时候会引用这个文件里面的配置

 .env.development文件对应开发环境

```sh
# just a flag,获取process.env.ENV
ENV = 'development'

# base api，获取 process.env.VUE_APP_BASE_API
VUE_APP_BASE_API = 'http://192.168.1.252:8182'

# vue-cli 使用 VUE_CLI_BABEL_TRANSPILE_MODULES 作为环境变量,
# 来控制 babel-plugin-dynamic-import-node 插件是否生效.
# 这个插件只做一件事情把所有的 import() 变成 require().
# 通过这种配置，当你有大量的页面时可以显著提高热更新的速度,
# Detail:  https://github.com/vuejs/vue-cli/blob/dev/packages/@vue/babel-preset-app/index.js

VUE_CLI_BABEL_TRANSPILE_MODULES = true

```



.env.production模式用于build，线上环境。

`.env.production`文件对应生产环境

```sh
just a flag
ENV = 'production'

# base api
VUE_APP_BASE_API = '/prod-api'

```

.env.staging 文件对应测试环境

```sh
NODE_ENV = production

# just a flag
ENV = 'staging'

# base api
VUE_APP_BASE_API = '/stage-api'
```

