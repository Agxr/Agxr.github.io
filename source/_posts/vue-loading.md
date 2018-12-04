---
title: vue-loading
tags:
  - vue
  - loading
comments: true
abbrlink: 234
categories: vue
date: 2018-04-24 13:30:10
---


### vue项目加载数据时 loding图

- 1.一般项目中，有时候会要求，你在数据请求的时候显示一张gif图片，然后数据加载完后，消失。这个，一般只需要在封装的axios中写入js事件即可。当然，我们首先需要在app.vue中，加入此图片。如下：

```javascrpt
<template>
  <div id="app">
    <loading v-show="fetchLoading"></loading>
    <router-view></router-view>
  </div>
</template>
```

<!-- more -->

```javascript
<script>
  import { mapGetters } from 'vuex';
  import Loading from './components/common/loading';

  export default {
    name: 'app',
    data() {
      return {
      }
    },
    computed: {
      ...mapGetters([
        'fetchLoading',
      ]),
  },
  components: {
    Loading,
  }
  }
</script>

<style>
  #app{
    width: 100%;
    height: 100%;
  }
</style>
```

- 2.这里的fetchLoading是存在vuex里面的一个变量。在store/modules/common.js里需要如下定义：

```javascript
/* 此js文件用于存储公用的vuex状态 */
import api from './../../fetch/api'
import * as types from './../types.js'
const state = {
  // 请求数据时加载状态loading
  fetchLoading: false
}
const getters = {
  // 请求数据时加载状态
  fetchLoading: state => state.fetchLoading
}
const actions = {
  // 请求数据时状态loading
  FETCH_LOADING({
    commit
  }, res) {
    commit(types.FETCH_LOADING, res)
  },
}
const mutations = {
  // 请求数据时loading
  [types.FETCH_LOADING] (state, res) {
    state.fetchLoading = res
  }
}
```

- 3.loading组件如下：

```javascript
<template>
  <div class="loading">
    <img src="./../../assets/main/running.gif" alt="">
  </div>
</template>

<script>
  export default {
    name: 'loading',
    data () {
      return {}
    },
  }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .loading{
    position: fixed;
    top:0;
    left:0;
    z-index:121;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.3);
    display: table-cell;
    vertical-align: middle;
    text-align: center;
  }
  .loading img{
    margin:5rem auto;
  }
</style>
```

- 4.最后在fetch/api.js里封装的axios里写入判断loading事件即可。如下：

```javascript
// axios的请求时间
let axiosDate = new Date()
export function fetch (url, params) {
  return new Promise((resolve, reject) => {
    axios.post(url, params)
      .then(response => {
        // 关闭  loading图片消失
        let oDate = new Date()
        let time = oDate.getTime() - axiosDate.getTime()
        if (time < 500) time = 500
        setTimeout(() => {
          store.dispatch('FETCH_LOADING', false)
        }, time)
        resolve(response.data)
      })
      .catch((error) => {
        // 关闭  loading图片消失
        store.dispatch('FETCH_LOADING', false)
        axiosDate = new Date()
        reject(error)
      })
  })
}
export default {
  // 组件中公共页面请求函数
  commonApi (url, params) {
    if(stringQuery(window.location.href)) {
      store.dispatch('FETCH_LOADING', true);
    }
    axiosDate = new Date();
    return fetch(url, params);
  }
}
```

- 参考博客：[vue项目加载数据时 loding图--- 参考博客文章](http://blog.csdn.net/zhaohaixin0418/article/details/73459662)
