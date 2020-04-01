## VUE相关

1. **MVC与MVVM**

   MVC模式：model - 模型（数据保存），view - 视图（用户界面），controller-控制器（业务逻辑）。

   MVVM模式：

   ​	`model`-数据层。

   ​	`view`-视图层。

   ​	`viewModel`-连接 model和view。

   区别：

   - mvc是单向通信，V跟M 必须通过 C 来承上启下。mvvm 是双向通信，V和M都可以和VM进行双向通，相互调用。
   - mvvm是 数据驱动视图，view的变动自动反应在viewModel,反过来也一样。mvc 则没有。

2. **双向绑定v-model原理？**（超高频率，最好手写一个简单定的双向绑定，面试官可能会直接问 compile,watcher,Oberver, deps 的功能和之间的联系）？

   ![图片描述](https://segmentfault.com/img/bVBQYu?w=730&h=390)

   vue.js 则是采用 <u>数据劫持结合发布者-订阅者模式</u> 的方式，通过`Object.defineProperty()`来劫持各个属性的`setter`，`getter`，在数据变动时发布消息给订阅者（Wacther）, 触发相应的监听回调。

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

3. **vue渐进式**

   

4. vue 生命周期？

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

5. **vue 父子组件传参方式？**

   子组件取得父组件的属性或方法

   - 子组件通过 this.$parent.event 直接调用
   - 子组件里使用 `$emit` 向父组件触发一个事件，父组件在引用子组件中监听这个事件。
   - 父组件属性通过props传递到子组件 ,方法也可以。子组件中直接使用。

   父组件调用子组件的属性或方法

   - 父组件中直接访问子组件的ref。`this.$refs['childRefNmae'].属性/方法`

6. **vue自定义组件**

   每个 .vue 文件就是一个组件，封装独立可复用的组件构成大型应用。

7. **vue自定义指令**



1. **vuex组成和原理**
2. vue-router的实现原理，history和hash模式有什么区别？
3. 虚拟 dom 为什么效率高？
4. vue实现seo?
5. computed 如何得知数据变化？computed 有缓存吗？
6. $nextTick原理？
7. v-if 和 v-show 有什么区别？
8. keep-alive 是否与普通组件有一样的生命周期，如果不是，有哪些钩子？
9. vue3.0和2.0双向绑定的区别，这样的改动有什么好处？

