## VUE相关

### 1. MVC与MVVM

MVC模式：M-model - 模型（数据保存），V-view - 视图（用户界面），C-controller-控制器（业务逻辑）。

<img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015020105.png" alt="img" style="zoom:50%;" />

1,view 传送指令到 controller  =》2. controller 完成业务逻辑后，要求model改变状态  =》3.model 将新的数据发送给view,用户得到反馈。

MVVM模式：

​	M-`model`-数据层。

​	V-`view`-视图层。

​	VM-`viewModel`-连接 model和view。

区别：

- mvc是单向通信，V跟M 必须通过 C 来承上启下。mvvm 是**双向通信**，V和M都可以和VM进行双向通，相互调用。
- mvvm是 **数据驱动视图**，view的变动自动反应在viewModel,反过来也一样。mvc 则没有。

### 2. 双向绑定v-model原理？

（超高频率，最好手写一个简单定的双向绑定，面试官可能会直接问 compile,watcher,Oberver, deps 的功能和之间的联系）？

![图片描述](https://segmentfault.com/img/bVBQYu?w=730&h=390)

vue.js 则是采用 **<u>数据劫持结合发布者-订阅者模式</u>** 的方式，通过`Object.defineProperty()`来劫持各个属性的`setter`，`getter`，在数据变动时发布消息给订阅者（Wacther）, 触发相应的监听回调。

举例：当把一个 js 对象传给 Vue 实例来作为data 时， Vue 将遍历它的属性，用 Object.defineProperty() 处理一遍，在get 和set 的时候监控访问和修改。



简化版

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <input type="text" id="txt">
    <p id="show-txt"></p>
</div>
<script>
    var obj={}
    Object.defineProperty(obj,'txt',{
        get:function(){
            return obj
        },
        set:function(newValue){
            document.getElementById('txt').value = newValue
            document.getElementById('show-txt').innerHTML = newValue
        }
    })
    document.addEventListener('keyup',function(e){
        obj.txt = e.target.value
    })
</script>
</body>
</html>
```

1，Object.defineProperty(obj, prop, descriptor)

​	  obj：需要定义属性的对象

 	 prop：需要定义的属性

  	descriptor：属性的描述描述符

  	返回值：返回此对象

2，属性描述符（`configurable` ，`writable` ，`emumerable` ，`value` ）与访问器描述符（`	configurable` ，`emumerable`，`get` ，`set` ）。两者不能同时使用否则报错。

### 3. 生命周期vue？

| 生命周期钩子函数       | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| beforeCreate（创建前） | 在数据观测和初始化事件还未开始                               |
| created (创建后)       | 完成数据观测、属性和方法的运算、初始化事件，$el 属性还没有显示出来 |
| beforeMount(载入前)    | 在挂载开始前被调用，相关render函数                           |
| mounted(载入后)        | el被 vm.$el 替换，并挂载到实例后调用。                       |
| beforeUpdate(更新前)   | 数据更新前                                                   |
| updated(更新后)        | 数据更新后                                                   |
| beforeDestroy(销毁前)  | 实例销毁前                                                   |
| destroyed(销毁后)      | 实例销毁后。所有的事件监听器、子实例已被销毁。               |

### 4. 父子vue组件传参方式？

子组件取得父组件的属性或方法

- 子组件通过 this.$parent.event 直接调用
- 子组件里使用 `$emit` 向父组件触发一个事件，父组件在引用子组件中监听这个事件。
- 父组件属性通过props传递到子组件 ,方法也可以。子组件中直接使用。

父组件调用子组件的属性或方法

- 父组件中直接访问子组件的ref。`this.$refs['childRefNmae'].属性/方法`

### 5.自定义vue组件

​	每个 .vue 文件就是一个组件，封装独立可复用的组件构成大型应用。

### 6.自定义vue指令

常用指令` v-for` 、` v-if` 、`v-bind` 、 `v-on` 、`v-show` 、`v-model`等

有的情况下，你仍然需要对普通 DOM 元素进行底层操作，这时候就会用到自定义指令。

```html
<div id="app">
    <input type="text" v-focus>
</div>
```

创建局部自定义指令：直接在组件中定义 directives:{ focus:{}}  

```js
 new Vue({
        el: '#app',
        data: function () {
            return {

            }
        },
        directives:{
            // 输入框自动聚焦
            focus:{
                //钩子函数：被绑定元素插入父节点时调用
                inserted: function(el){
                    //当被绑定的元素插入到 DOM 中时……自动获取input 焦点
                    el.focus()
                }
            },
        },
    })
```



自定义全局指令：使用新的vue 对象 Vue.directive('focus',{ }）

```js
  // 注册一个全局自定义指令 `v-focus`
    Vue.directive('focus', {
        inserted: function (el) {
            // 当被绑定的元素插入到 DOM 中时，聚焦元素
            el.focus()
        }
    })
    new Vue({
        el: '#app',
    })
```

### 7.  自定义vue过滤器？

过滤器常被用于一些常见的文本格式化。比如列表数据默认值。

在 双花括号`{{ }}` 和 `v-bind` 表达式后**使用**。

```html
<!-- 在双花括号中 -->
{{ message | capitalize }}
<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```

**定义过滤器分为：组件过滤器和全局过滤器**

组件过滤器：直接在组件中 使用 ` filters:{ filterName :function(value){ ...code } }`

全局过滤器：在创建vue 实例前定义过滤器 `Vue.filter('filterName ',function(value){ ...code })`

```js
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})
```



### 8，vuex组成和原理

**5个核心概念**

State （单一状态树）

Getter（store的计算属性）

Mutation（更改state的唯一方式就是提交mutation）

```js
store.commit('increment', {param: xx})//js中提交改变状态
this.$store.commit('increment', {param: xx})  //组件中改变状态
```

Action （1,action提交的是mutation，而不是直接改变状态；2,action 可以异步操作）

Module：（）

**原理:**

<img src="https://vuex.vuejs.org/vuex.png" alt="vuex" style="zoom: 80%;" />

### 9，vue-router的实现原理

vue-router原理: 

通过浏览器的`HashHistory`和`HTML5History`两种方式实现前端路由，对应 history 模式和 hash 模式。

- `HashHistory`: 利用URL中的hash（“#”）;  替换路由方法 replace()方法与push()方法不同之处在于，它并不是将新路由添加到浏览器访问历史的栈顶，而是替换掉当前的路由。
- `HTML5History`: 浏览器历史记录栈提供的接口。通过 `back()`, `forward()`, `go(`)等方法，我们可以**读取浏览器历史记录栈的信息**，进行各种跳转操作。

### 10，vue-router的history和hash模式有什么区别？

 `hash` 模式下，仅 hash 符号之前的内容会被包含在请求中，如 http://www.abc.com/#/login，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。

`history` 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，如 http://www.abc.com/book/id。需要后端配置支持。需要在nginx 或 apache 中进行路由配置，否则会 404.

### 11，虚拟 dom 为什么效率高，原理？

**原生 JS** 或者 jquery 操作 DOM时，浏览器会从构建DOM树从头执行一遍`解析流程`。若有一次操作更新10个dom节点，会立即操作dom十次。**缺点：**第一次计算完，紧接着下一个DOM更新请求，这个节点的坐标值就变了，前一次计算为无用功，浪费性能。

**虚拟DOM**中，若一次操作中有10次更新DOM的动作，虚拟DOM不会立即操作DOM，而是将这10次更新的`diff内容`保存到本地一个JS对象中，最终将这个JS对象一次性attch到DOM树上，再进行后续操作，避免大量无谓的计算量。**优点**：页面的更新先反映在JS对象（虚拟dom）,内存中操作对象显然比解析流程快的多，最终的js对象映射成真实DOM,再去绘制。

**原理**：JS对象, diff算法

### 12，vue实现SEO?

seo,是便于搜索引擎爬取。 传统页面在 meta 标签加入关键词。

单页面应用要解决的问题是页面预加载：

- SSR 服务端渲染，
- 使用 vue-meta-info + prerender-spa-plugin 做页面预渲染。或者 [Nuxt.js](https://zh.nuxtjs.org/guide/installation)  框架。

### 13，computed 如何得知数据变化？computed 有缓存吗？

默认getter 获取数据，访问计算属性时会执行 getter。

有缓存，官网文档中有一句 “不同的是**计算属性是基于它们的响应式依赖进行缓存的**

，只在相关响应式依赖发生改变时它们才会重新求值。”不希望使用缓存，就去使用` methods:{}`

### 14，$nextTick原理？

`Vue.nextTick(callback)` 作用：针对在数据更新完成后，等待**dom更新后执行**的问题。比如调用elementUI对话框的ref 时。

vue更新DOM是异步的，数据变更时不会立即重新渲染，而是被推入异步更新队列，使用`promise.then`等。如果不支持 会使用 `setTimeout(fn,0)`。为了在数据变化之后等待 Vue 完成更新 DOM，可以在数据变化之后立即使用 `Vue.nextTick(callback)`。这样回调函数将在 DOM 更新完成后被调用。

**原理**就是：因为 `$nextTick()` 返回一个 `Promise` 对象，根据事件循环，宏任务与微任务的执行顺序来实现的。promise 属于宏任务也是异步任务。更新dom也是异步的，所以会在更新dom后执行。

*附上执行顺序：1，先执行同步主线程，再执行异步任务队列。 2，宏任务队列 和微任务队列都有任务的时候，先微任务后宏任务。



### 14，keep-alive 是否与普通组件有一样的生命周期，如果不是，有哪些钩子？

**keep-alive** 是内置的组件，抽象组件，他没有生命周期，被他包裹的组件有，并会多两个：

- activated： 被 keep-alive 缓存的组件激活时调用.

- deactivated: 被 keep-alive 缓存的组件停用时调用。

**作用是**：主要用于保留组件状态或避免重新渲染。

> 在 2.2.0 及其更高版本中，`activated` 和 `deactivated` 将会在 `keep-alive` 树内的所有嵌套组件中触发。

### 15，vue3.0和2.0双向绑定的区别，这样的改动有什么好处？

观察机制

- 3.0 版本里将有一个基于 Proxy 的观察者，提供全语言覆盖的响应式跟踪。

- 2.x 版本里基于 Object.defineProperty 的观察者

好处：

1. 组件生成快了一倍，减少一般内存的使用。

2. 新实现更加强大：可以检测属性的增加和删除、数组索引、length的变化。支持Map、set、WeakMap、WeakSet



## 简单一句话就能回答的

### 1，css 只能在当前组件使用？

style标签增加 `scoped`，样式会自动添加一个hash值。

如果只修改当前组件的第三方组件样式使用 `/deep/`，同时 /deep/ 也会对当前组件的子组件生效。

```html
<style scoped>
 >>> 第三方组件class { 样式 }
或者
/deep/  第三方组件class { 样式 }
</style>
```

### 2，v-if 和 v-show 区别

v-if 切换时，标签会创建和销毁。v-show 则是初始化时候就加载好了，仅仅只是css display:none的显示隐藏。

### 3，`$route` 和 `$router` 的区别

`$route` 是vue的 **路由信息对象**，包含path、params、query、name、matched等对象属性。

`$router` 是vue的 **路由实例**，包含beforeEach、push、go、back等钩子函数。

### 4，vueJS的两个核心？

数据驱动、组件系统。

### 5，vue常用指令？

v-if、v-else、v-for、v-model、v-show、v-bind、v-on、v-slot (插槽)等

### 6，vue常用修饰符

事件修饰符：v-on:click.stop（阻止单击事件继续传播） 、.once、.pervent(阻止默认事件)

### 7，v-on 可以绑定多个方法？

可以，@是v-on的缩写

```vue
<input v-on:focus='' v-on:input='' v-on:click=''/>
```

### 8，vue 中key 的作用？

key的作用是为了更高效的更新虚拟Dom. 原理：v-for 在更新已渲染过的元素列表时，不移动DOM 元素顺序。

### 9，vue的计算属性？

**计算属性是基于它们的响应式依赖进行缓存的**，只在相关响应式依赖发生变化时，它们才会重新请求值。默认只有 `getter`,可以手动 `setter`。

### 10，vue等单页面应用的优缺点？

优点：

1. 内容该改变不会刷新页面，不会出现闪动和白屏。
2. 前后端分离，前端可以分担部分业务。
3. 路由在前端，方便视图控制。

缺点：

1. 不支持低版本浏览器，最低到 IE9
2. 不利于 SEO 优化。
3. 首屏加载时候较长。





