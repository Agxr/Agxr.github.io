
---
title: vue-learn
tags:
  - vue
comments: true
abbrlink: 1
categories: vue
date: 2018-04-21 16:35:36
---

### vue学习

- 1.vue-resource获取接口数据的方法，参考：[Vue基础知识之vue-resource和axios](https://www.cnblogs.com/Juphy/p/7073027.html)

- 2.vue-cli搭建的架子里面加入mock假数据，[参考文章](https://www.jianshu.com/p/ccd53488a61b)

- 3.关于Vue实例的生命周期created和mounted的区别...[参考文章](http://www.zhimengzhe.com/Javascriptjiaocheng/236707.html)

- 4.Vue如何使用vue-awesome-swiper实现轮播效果...[参考文章](https://www.cnblogs.com/zishang91/p/7600006.html)

<!-- more -->
- 5.[Vue学习借鉴参考文章--爬坑之路](https://www.cnblogs.com/wisewrong/p/6277262.html)

- 6.[在vue-cli中使用vue-router及vuex的例子](https://www.jianshu.com/p/f5bbcbd5b4b5)

- 7.在vue中使用aixos出现跨域的解决方案---参考文章[vue中axios解决跨域问题和拦截器使用](http://blog.csdn.net/kirinlau/article/details/78611774)

- 8.在vue中使用bus，实现非父子组件之间的传参---[参考文章](https://www.cnblogs.com/place-J-P/p/7586819.html)----||||----[vue2.0s中eventBus实现兄弟组件通信](https://blog.csdn.net/u013034014/article/details/54574989?locationNum=2&fps=1)

- 9.vue中使用webpack打包优化。[参考文章](https://www.cnblogs.com/kevin2chen/p/6816693.html)

```bash
例： 对于单页应用，可以使用bundle-loader异步加载各路由对应的组件（懒加载），尽快显示首屏。
- 在vue-router中，使用``const Foo = () => import('./Foo.vue')``懒加载。
routes: [
    {
        path: '/',
        name: 'Hello',
        component: () => import('../module/HelloWorld')
    },
    {
        path: '/check',
        name: 'check',
        component: r => require(['@/pages/spa/module/CheckBox'], r)
    }
]
```

```bash
vue打包优化的思路：
1.路由懒加载组件
2.把第三方库单独打包出来 WebpackDLLPlugin & WebpackDllReferencePlugin
3.第三方依赖打包出来后走cdn
4.服务器nginx开启gzip
5.可以开启WebpackBundleAnalyzer查看依赖关系针对性进行优化 有些依赖其实是没用上的
```

- 10.vue路由的懒加载--[参考文章2](http://www.cnblogs.com/wjunwei/p/9242142.html)

- 11.[vue cli-3 配置axios 跨域请求和表单提交](https://blog.csdn.net/qq_36306590/article/details/81746897)

- 12.父组件往子组件传参通过props（属性）传参时，在子组件中接受时，可设置默认数值

```bash
export default {
  name: "searchInput",
  props: {
    placeholder: {
      type: String, // 数据类型
      default: "请输入关键字" // 默认值
    }
  }
}
```

- 13.



---

### vuex学习

- 1.vuex的----[知识博客](https://blog.csdn.net/u012149969/article/details/80350907)

- 2.[vuex namespaced的作用以及使用方式](https://blog.csdn.net/fuck487/article/details/83411856)

> vuex namespaced的作用以及使用方式  

> vuex中的store分模块管理，需要在store的index.js中引入各个模块，为了解决不同模块命名冲突的问题，将不同模块的namespaced:true，之后在不同页面中引入getter、actions、mutations时，需要加上所属的模块名  

```bash
1、声明分模块的store时加上namespaced:true
    export default {
      namespaced: true,
      state,
      getters,
      actions,
      mutations
    }
2、使用模块中的mutations、getters、actions时候，要加上模块名，例如使用commint执行mutations时
    > 格式：模块名/模块中的mutations
    > xxx/setUserInfo
    > this.$store.commit("userInfo/setUserInfo",userInfo)
3、获取属性时同样加上模块名
    > 格式：store.state.模块名.模块属性
    > $store.state.userInfo.userName

```

- 3.
