---
title: vue-filter
tags:
  - vue
  - filter
comments: true
abbrlink: 234
categories: vue
date: 2018-04-23 13:10:36
---


### vue自定义的过滤器

##### Vue2 全局过滤器（vue-cli）

- 1.先看官方简介：当前组件注册

```javascript
export default {
  data () {
    return {}
  },
  filters: {
    orderBy() {
      // doSomething
    },
    uppercase() {
      // doSomething
    }
  }
}
```

> 但是我们做项目来说，大部分的过滤器是要全局使用的，不会每每用到就在组件里面去写，嗯，还是抽成全局的会更好些。  

<!-- more -->

- 2.全局注册：（官网: https://cn.vuejs.org/v2/api/#filters）

```javascript
// 注册
Vue.filter('my-filter', function (value) {
// 返回处理后的值
})
 
// getter，返回已注册的过滤器
var myFilter = Vue.filter('my-filter')
```

> 当项目所用到的过滤器比较多时，就想试着把所有的方法定义在一个文件里面导出，嗯，毕竟还是有分点层次的。    

```javascript
// /src/common/filters/custom.js文件

let dateServer = value => {
  return value.replace(/(\d{4})(\d{2})(\d{2})/g, '$1-$2-$3')
}
export { dateServer }
```

```javascript
// /src/main.js文件

import * as custom from './common/filters/custom'

Object.keys(custom).forEach(key => {
  Vue.filter(key, custom[key])
})
```

> 然后在其他的.vue 文件中就可愉快地使用这些我们定义好的全局过滤器了  

```javascript
<template>
  <section class="content">
    <p>{{ time | dateServer }}</p> <!-- 2016-01-01 -->
  </section>
</template>
<script>
  export default {
    data () {
      return {
        time: 20160101
      }
    }
  }
</script>
```

---  
- 参考文章：[vue自定义过滤器的参考文献](http://www.cnblogs.com/xiterjia/p/6701324.html)

