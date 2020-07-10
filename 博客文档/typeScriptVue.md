## 说明

>该项目主要是实践在vue中使用typescript，以进行记录和尝试

**集成 `ts` 的体验**

> - template 和 style 跟以前的写法保持一致，只有 script 的变化

#### 什么是typescript

typescript 是 javascript 的超集。也是 ES6 的超集，是JS 的强类型版本。不能在浏览器上运行，但是可以编译后运行。在 javascript 添加了一些扩展， class 、interface 、 module 等。增加代码可读性

- 支持所有的 javascript 、ES6 语法。
- 强类型语言，静态类型检查
- IDE 智能提示
- 可读性
- 易于重构

> 静态类型检查可以避免很多不必要的错误，不用在调试的时候才发现问题。

## 新项目起手主要流程

- vue-cli创建项目
- 安装ts 依赖
- 配置webpack
- 添加 tsconfig.json
- 添加tslint.json
- 让ts 识别 .vue
- 改造 .vue 文件

#### vue-cli创建项目

这步按照官方文档来就行了。

#### 安装ts 依赖

项目创建完成后，安装一些必要的插件和以后要用到的

- [vue-class-component](https://github.com/vuejs/vue-class-component)：强化 Vue 组件，使用 TypeScript/装饰器 增强 Vue 组件
- [vue-property-decorator](https://github.com/kaorun343/vue-property-decorator)：在 `vue-class-component` 上增强更多的结合 Vue 特性的装饰器
- [ts-loader](https://github.com/TypeStrong/ts-loader)：**必装**TypeScript 为 Webpack 提供了 `ts-loader`，其实就是为了让webpack识别 .ts .tsx文件
- [tslint-loader](https://github.com/wbuchwalter/tslint-loader)跟[tslint](https://github.com/palantir/tslint)：我想你也会在`.ts` `.tsx`文件 约束代码格式（作用等同于eslint）
- [tslint-config-standard](https://github.com/blakeembrey/tslint-config-standard)：`tslint` 配置 `standard`风格的约束

命令

```sh
npm i vue-class-component vue-property-decorator --save
npm i ts-loader typescript tslint tslint-loader tslint-config-standard --save-dev
```

#### 配置webpack

vue-cli2 的修改 ./build/webpack.base.conf.js

1. 找到`entry.app` 将`main.js` 改成 `main.ts`, 顺便把项目文件中的`main.js`也改成`main.ts`, 里面内容保持不变

   ```js
   entry: {
     app: './src/main.ts'
   }
   ```

2. 找到`resolve.extensions` 里面加上`.ts` 后缀 （是为了之后引入.ts的时候不写后缀）

   ```js
     resolve: {
       extensions: ['.js', '.vue', '.json', '.ts'],
       alias: {
         '@': resolve('src')
       }
     }
   ```

   

3. 找到`module.rules` 添加webpack对`.ts`的解析

   ```js
   module: {
     rules: [
       {
         test: /\.(js|vue)$/,
         loader: 'eslint-loader',
         enforce: 'pre',
         include: [resolve('src'), resolve('test')],
         options: {
           formatter: require('eslint-friendly-formatter')
         }
       },
   // 从这里复制下面的代码就可以了
       {
         test: /\.ts$/,
         exclude: /node_modules/,
         enforce: 'pre',
         loader: 'tslint-loader'
       },
       {
         test: /\.tsx?$/,
         loader: 'ts-loader',
         exclude: /node_modules/,
         options: {
           appendTsSuffixTo: [/\.vue$/],
         }
       },
   // 复制以上的
     }
   }
   ```

   `ts-loader` 会检索当前目录下的 tsconfig.json 文件，根据里面定义的规则来解析 .ts 文件（跟 .babelrc 一样属于配置文件 ）

   `tslint-loader` 作用等同于 eslint-loader

   

#### 根目录添加 tsconfig.json

   配置参考

   ```json
   {
     // 编译选项
     "compilerOptions": {
       // 输出目录
       "outDir": "./output",
       // 是否包含可以用于 debug 的 sourceMap
       "sourceMap": true,
       // 以严格模式解析
       "strict": true,
       // 采用的模块系统
       "module": "esnext",
       // 如何处理模块
       "moduleResolution": "node",
       // 编译输出目标 ES 版本
       "target": "es5",
       // 允许从没有设置默认导出的模块中默认导入
       "allowSyntheticDefaultImports": true,
       // 将每个文件作为单独的模块
       "isolatedModules": false,
       // 启用装饰器
       "experimentalDecorators": true,
       // 启用设计类型元数据（用于反射）
       "emitDecoratorMetadata": true,
       // 在表达式和声明上有隐含的any类型时报错
       "noImplicitAny": false,
       // 不是函数的所有返回路径都有返回值时报错。
       "noImplicitReturns": true,
       // 从 tslib 导入外部帮助库: 比如__extends，__rest等
       "importHelpers": true,
       // 编译过程中打印文件名
       "listFiles": true,
       // 移除注释
       "removeComments": true,
       "suppressImplicitAnyIndexErrors": true,
       // 允许编译javascript文件
       "allowJs": true,
       // 解析非相对模块名的基准目录
       "baseUrl": "./",
       // 指定特殊模块的路径
       "paths": {
         "jquery": [
           "node_modules/jquery/dist/jquery"
         ]
       },
       // 编译过程中需要引入的库文件的列表
       "lib": [
         "dom",
         "es2015",
         "es2015.promise"
       ]
     }
   }
   
   ```

#### 根目录下添加tslint.json

   ```json
   {
     "extends": "tslint-config-standard",
     "globals": {
       "require": true
     }
   }
   ```

#### 让 ts 识别 .vue

   typescript 默认不支持 .vue 文件，需要在 vue 项目 src 中 创建一个 vue-shim.d.ts 文件，

   ```ts
   declare module "*.vue" {
     import Vue from "vue";
     export default Vue;
   }
   
   ```

   重点！重点!重点!

   项目中在引入 vue文件时候，需要写上 .vue 后缀。例如

   ```js
   import Compontent from 'compontents/component.vue'
   ```

#### 改造 .vue 文件

   **vue-class-compontent** 

   对 vue 组件进行了一层封装，让vue 组件语法在结合了 Typescript 语法之后更加扁平化。

   （下面的内容需要掌握`es7`的[装饰器](http://taobaofed.org/blog/2015/11/16/es7-decorator/), 就是下面使用的@符号）

   ```vue
   <template>
     <div class="demoAsync">
       <div>
         <input type="text" v-model="msg">
         <p>
           msg: {{msg}}
         </p>
         <button @click="greet">
           Greet
         </button>
       </div>
     </div>
   </template>
   
   <script lang="ts">
       import Vue from 'vue'
       import Component from 'vue-class-component'
   
       @Component
       export default class App extends Vue {
           // 初始化数据
           msg = 123
   
           // 声明周期钩子    
           mounted() {
               this.greet()
           }
   
           // 计算属性    
           get computedMsg() {
               return 'computed ' + this.msg
           }
   
           // 方法    
           greet() {
               alert('greeting: ' + this.msg)
           }
       }
   </script>
   
   <style scoped lang="scss">
   
   </style>
   ```

   等同于原来的方式

   ```js
   export default {
           name: "DemoAsync",
           data() {
               return {
                   msg: 123
               }
           },
           // 声明周期钩子  
           mounted() {
               this.greet()
           },
           //  计算属性  
           computed: {
               computedMsg() {
                   return 'computed ' + this.msg
               }
           },
           //  方法  
           methods: {
               greet() {
                   alert('greeting: ' + this.msg)
               }
           }
       }
   ```

   **vue-property-decorator**

   vue-property-decorator 是在 vue-class-compontent 上增强了更多的结合 vue 特性的装饰器，新增了7个装饰器。

   - @Emit

   - @Inject

   - @Model

   - @Prop

   - @Provide

   - @Watch

   - @compontent ( 从 vue-class-compontent 继承而来 )

     

在这里列举几个常用的`@Prop/@Watch/@Component`, 更多信息，详见[官方文档](https://github.com/kaorun343/vue-property-decorator)

```js
  @Component
    export default class DemoAsync extends Vue {
        @Prop()
        propA: number = 1

        @Prop({ default: 'default value'})
        propB: string

        @Prop([ String,Boolean])
        propC: string | boolean

        @Prop({ type: null })
        propD: any

        @Watch('child')
        onChildChange(val: string, oldValue: string ) {

        }
    }
```

相当于

```js
    export default {
        props:{
            checked: Boolean,
            propA: Number,
            propB: {
                type: String,
                default: 'default value'
            },
            propC: [String,Boolean],
            propD: { type: null }
        },
        methods:{
          onChildChanged(val, oldValue){

          }
        },
        watch:{
            'child': {
                handler: 'onChildChanged',
                immediate: false,
                deep: false
            }
        }
    }

```

#### 改造 App.vue 文件

1. 在 script 标签上加上 lang="ts" ,意思是 让 webpack 将这段代码识别为 typescript 

2. **基于类的 Vue 组件**( 跟`react`组件写法有点类似, 详见[官方](https://cn.vuejs.org/v2/guide/typescript.html#基于类的-Vue-组件) )， 如下图

   ```js
   import Vue from 'vue'
   import Component from 'vue-class-component'
   
   // @Component 修饰符注明了此类为一个 Vue 组件
   @Component({
     // 所有的组件选项都可以放在这里
     template: '<button @click="onClick">Click!</button>'
   })
   export default class MyComponent extends Vue {
     // 初始数据可以直接声明为实例的 property
     message: string = 'Hello!'
   
     // 组件方法也可以直接声明为实例的方法
     onClick (): void {
       window.alert(this.message)
     }
   }
   ```

   

3. 用`vue-property-decorator`语法改造之前代码

##  老项目改造



## 错误处理

有时候不想处理不相干的错误

单行忽略，单行上面

```
// @ts-ignore
```

忽略全文，文件顶部

```
// @ts-nocheck
```

取消忽略全文，文件顶部

```
// @ts-check
```