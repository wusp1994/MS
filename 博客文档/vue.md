## vue路由打开新窗口

**一. 标签实现新窗口打开：**

官方文档中说 v-link 指令被 <router-link> 组件指令替代，且 <router-link> 不支持 target="_blank" 属性，如果需要打开一个新窗口必须要用<a>标签，但事实上vue2版本的 <router-link> 是支持 target="_blank" 属性的(tag="a")，示例如下：

```vue
<router-link tag="a" target="_blank" :to="{name:'searchGoods',params:{catId:0},query:{keywords:'手机'}}">热门好货</router-link>
```

注：只有tag="a"模式下 target="_blank" 属性才会生效。

**二. 编程式导航：**

有些时候需要在单击事件或者在函数中实现页面跳转，那么可以借助router的示例方法，通过编写代码实现。我们常用的是 $router.push 和 $router.go ,但是vue2.0以后，这种方式就不支持新窗口打开的属性了。这两种平常用的都比较多，这里就不再赘述。百度了下，找到了使用 $router.resolve 这种方法能够实现新窗口打开，示例代码如下：

```js
let routeData = this.$router.resolve({
   name: "searchGoods",
   query: params,
   params:{catId:params.catId}
});
window.open(routeData.href, '_blank');
```

