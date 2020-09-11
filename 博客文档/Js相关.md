## js对象

删除对象中的属性

```js
let obj = { id: 1, name:'xiaoxiao'}
delete obj.name
console.log(obj)//{id: 1}
delete obj  //false
```



# 前端开发各种高端操作

## 类型转换

### 快速转 Number

```js
var a = '1'

console.log(typeof a)
console.log(typeof Number(a)) // 普通写法
console.log(typeof +a) // 高端写法
```



### 快速转 Boolean

```js
var a = 0

console.log(typeof a)
console.log(typeof Boolean(a)) // 普通写法
console.log(typeof !!a) // 高端写法
```



### 混写

先转为 Number 再转为 Boolean

```js
var a = '0'

console.log(!!a) // 直接转将得到 true，不符合预期
console.log(!!+a) // 先转为 Number 再转为 Boolean，符合预期
```



在侧边栏中用到了这种用法：

```js
!!+Cookies.get('sidebarStatus')
```



## js 和 css 两用样式

template 中需要动态定义样式，通常做法：

```html
<template>
  <div :style="{ color: textColor }">Text</div>
</template>

<script>
export default {
  data() {
    return {
      textColor: '#ff5000'
    }
  }
}
</script>
```



高端做法：

- 定义 scss 文件

```scss
$menuActiveText:#409EFF;

:export {
  menuActiveText: $menuActiveText;
}
```



- 在 js 中引用：
  - 使用 import 引用 scss 文件
  - 定义 computed 将 styles 对象变成响应式对象
  - 在 template 中使用 styles 对象

```html
<template>
  <div :style="{ color: styles.menuActiveText }">Text</div>
</template>

<script>
import styles from '@/styles/variables.scss'

export default {
  computed: {
    styles() {
      return styles
    }
  }
}
</script>
```



## 连续解构

从数组第一个对象元素中提取某个属性，比如：err 对象中包含一个 errors 数组，errors 数组每一个对象都包含一个 msg 属性

```js
err = {
  errors: [
    {
      msg: 'this is a message'
    }
  ]
}
```



快速的提取方法为：

```js
const [{ msg }] = err.errors
```



如果不用解构写法为：

```js
const msg = err.errors[0].msg
```