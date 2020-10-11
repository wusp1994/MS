

## UIN-APP入坑指南

最后我给有意入坑UNI-APP的其他开发者分享两条个人使用经验：

1. 尽量用基础的、简单的Vue语法实现功能，尽量用官方推荐的、现有的组件实现业务，开发时多妥协，不要吹毛求疵，谨慎封装自定义组件。
2. 如果你的应用以展示静态数据为主，那么在注意第1点的前提下用uniapp还是比较推荐的，真的能节约不少成本，可能局部需要注意一下长列表的优化。但是如果你的应用数据更新频繁，交互较为复杂，页面节点较多，做H5和小程序还是可以的，但是做APP请三思。

免费教程

https://ke.qq.com/course/343370?taid=2788142445051210

## 创建项目

 安装vue/cli 全局

> npm install -g @vue/cli

创建项目

> vue create -p dcloudio/uni-preset-vue my-project



## 条件编译

### css的条件编译

:::warning:  build后sass 文件中不能实现条件编译，只能在  .vue 文件中才能生效。

```css
/*  #ifdef MP-WEIXIN  */
微信平台特有样式
/*  #endif  */

/*  #ifndef MP-WEIXIN  */
除了微信平台特有样式
/*  #endif  */
```

### JSON的条件编译

```json
// #ifndef MP-WEIXIN
{
    "path": "pages/online-running/online-running",
    "style": {
        "navigationBarTitleText": "线上跑",
        "navigationStyle": "custom"
    }
},
// #endif

// #ifdef MP-WEIXIN
{
    "path": "pages/online-running/online-running",
    "style": {
        "navigationBarTitleText": "线上跑"
    }
},
// #endif
```

### 组件的条件编译

```vue
<view>
    <view>微信公众号关注组件</view>
    <view>
        <!-- uni-app未封装，但可直接使用微信原生的official-account组件-->
        <!-- #ifdef MP-WEIXIN -->
                <official-account></official-account>
        <!-- #endif -->
    </view>
</view>
```

### JS的条件编译

```js
// #ifdef  MP-WEIXIN
平台特有的API实现
// #endif

// #ifndef  MP-WEIXIN
除了微信平台特有js
// #endif
```

### [static 目录的条件编译](https://uniapp.dcloud.io/platform?id=static-目录的条件编译)

在不同平台，引用的静态资源可能也存在差异，通过 static 的的条件编译可以解决此问题，static 目录下新建不同平台的专有目录（目录名称同 `%PLATFORM%` 值域,但字母均为小写），专有目录下的静态资源只有在特定平台才会编译进去。

如以下目录结构，`a.png` 只有在微信小程序平台才会编译进去，`b.png` 在所有平台都会被编译。

```
┌─static                
│  ├─mp-weixin
│  │  └─a.png     
│  └─b.png
├─main.js        
├─App.vue      
├─manifest.json 
└─pages.json     
```

### [整体目录条件编译](https://uniapp.dcloud.io/platform?id=整体目录条件编译)

如果想把各平台的页面文件更彻底的分开，也可以在uni-app项目根目录创建`platforms`目录，然后在下面进一步创建`app-plus`、`mp-weixin`等子目录，存放不同平台的文件。

**注意**

- `platforms`目录下只支持放置页面文件（即页面vue文件），如果需要对其他资源条件编译建议使用[static 目录的条件编译](https://uniapp.dcloud.io/platform?id=static-目录的条件编译)

## vue 使用注意事项

uni-app 在发布到H5时支持所有vue的语法；发布到App和小程序时，由于平台限制，无法实现全部vue语法，但uni-app仍是是对vue语法支持度最高的跨端框架。本文将详细讲解差异。

只要根据vue的规范来，使用简单的vue语法不会有什么大问题。

#### 生命周期

`uni-app` 完整支持 `Vue` 实例的生命周期，同时还新增 [应用生命周期](https://links.jianshu.com/go?to=https%3A%2F%2Funiapp.dcloud.io%2Fframe%3Fid%3D%e5%ba%94%e7%94%a8%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f) 及 [页面生命周期](https://links.jianshu.com/go?to=https%3A%2F%2Funiapp.dcloud.io%2Fframe%3Fid%3D%e9%a1%b5%e9%9d%a2%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f)。

详见Vue官方文档：[生命周期钩子](https://links.jianshu.com/go?to=https%3A%2F%2Fcn.vuejs.org%2Fv2%2Fapi%2F%23%E9%80%89%E9%A1%B9-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)。

**注意**

- 不要在选项属性或回调上使用箭头函数，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数是和父级上下文绑定在一起的，`this` 不会是如你做预期的 `Vue` 实例，且 `this.a` 或 `this.myMethod` 也会是未定义的。
- 建议使用 `uni-app` 的 `onReady`代替 `vue` 的 `mounted`。
- 建议使用 `uni-app` 的 `onLoad` 代替 `vue` 的 `created`。

#### 模板语法

`uni-app` 完整支持 `Vue` 模板语法。

详见Vue官方文档：[模板语法](https://links.jianshu.com/go?to=https%3A%2F%2Fcn.vuejs.org%2Fv2%2Fguide%2Fsyntax.html)。

**注意** 如果使用老版的非自定义组件模式，即manifest中`"usingComponents":false`，部分模版语法不支持，但此模式已不再推荐使用，[详见](https://links.jianshu.com/go?to=https%3A%2F%2Fask.dcloud.net.cn%2Farticle%2F35699)。

老版非自定义组件模式不支持情况：

- 不支持纯 HTML
- 不支持部分复杂的 JavaScript 渲染表达式
- 不支持过滤器

#### data属性

`data` 必须声明为返回一个初始数据对象的函数；否则页面关闭时，数据不会自动销毁，再次打开该页面时，会显示上次数据。

```js
//正确用法，使用函数返回对象
data() {
    return {
        title: 'Hello'
    }
}
```

#### 计算属性

