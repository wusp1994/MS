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



## vue中使用节流和防抖

```js
//tool.js
/**
 * @desc 函数防抖
 * @param func 函数
 * @param wait 延迟执行毫秒数
 * @param immediate true 表立即执行，false 表非立即执行
 */
export function debounce (func, wait, immediate = true) {
  let timeout
  return function () {
    if (timeout) clearTimeout(timeout)
    if (immediate) {
      var callNow = !timeout
      timeout = setTimeout(() => {
        timeout = null
      }, wait)
      if (callNow) func.apply(this, arguments)
    } else {
      timeout = setTimeout(function () {
        func.apply(context, args)
      }, wait)
    }
  }
}
```

#### 方式一:import到每个组件单独使用

```js
import { debounce } from '@/utils/tool'//引入函数
export default {
    methods: {
        debounceClickTest: debounce(function () {
            this.clickTest()
        }, 1000),
            clickTest () {
            console.log(this.form.uname, this.form.upwd)
        }
    }
}
```

:::warning: 其回调函数不能使用箭头函数,否则拿不到 `this`,因为箭头函数绑定this,导致this为undefined.

```html
 <button @click="debounceClickTest">登陆</button>
```

#### 方式二:封装成vue函数组件,(不推荐)

#### 方式三:封装成指令(推荐)

```js
//directive.js
import Vue from 'vue'
import { debounce } from '@/utils/tool'
// 防抖自定义指令
Vue.directive('debounce', {
  bind (el, binding) {
    const executeFunction = debounce(binding.value, 1000)
    el.addEventListener('click', executeFunction)
  }
})
```

```html
  <button v-debounce="login">登陆</button>
```

