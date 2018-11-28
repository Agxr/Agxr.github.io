---
title: vue-cli3.x+axios
tags:
  - vue
  - axios
comments: true
abbrlink: 1
categories: vue
date: 2018-04-22 12:35:00
---


#### 一、安装axios和qs 依赖包 
#### 二、配置 axios 全局变量
  在main.js加上依赖的引入
```bash
import qs from 'qs'
import common from '../public/js/common'
import Axios from 'axios'
Vue.prototype.$http = Axios
Vue.prototype.$qs=qs
```

<!-- more -->

在src根目录创建一个 vue.config.js 的文件，这个是因为 cli-3 和 cli-2 创建项目的结构不同。
```javascript
// vue.config.js 配置说明
// 这里只列一部分，具体配置惨考文档啊
module.exports = {
    // baseUrl  type:{string} default:'/'
    // 将部署应用程序的基本URL
    // 将部署应用程序的基本URL。
    // 默认情况下，Vue CLI假设您的应用程序将部署在域的根目录下。
    // https://www.my-app.com/。如果应用程序部署在子路径上，则需要使用此选项指定子路径。例如，如果您的应用程序部署在https://www.foobar.com/my-app/，集baseUrl到'/my-app/'.

    baseUrl: process.env.NODE_ENV === 'production' ? '/online/' : '/',

    // outputDir: 在npm run build时 生成文件的目录 type:string, default:'dist'

    // outputDir: 'dist',

    // pages:{ type:Object,Default:undfind }
    /*
      构建多页面模式的应用程序.每个“页面”都应该有一个相应的JavaScript条目文件。该值应该是一
      个对象，其中键是条目的名称，而该值要么是指定其条目、模板和文件名的对象，要么是指定其条目
      的字符串，
      注意：请保证pages里配置的路径和文件名 在你的文档目录都存在 否则启动服务会报错的
    */
    // pages: {
    // index: {
    // entry for the page
    // entry: 'src/index/main.js',
    // the source template
    // template: 'public/index.html',
    // output as dist/index.html
    // filename: 'index.html'
    // },
    // when using the entry-only string format,
    // template is inferred to be `public/subpage.html`
    // and falls back to `public/index.html` if not found.
    // Output filename is inferred to be `subpage.html`.
    // subpage: 'src/subpage/main.js'
    // },

    //   lintOnSave：{ type:Boolean default:true } 问你是否使用eslint
    lintOnSave: true,
    // productionSourceMap：{ type:Bollean,default:true } 生产源映射
    // 如果您不需要生产时的源映射，那么将此设置为false可以加速生产构建
    productionSourceMap: false,
    // devServer:{type:Object} 3个属性host,port,https
    // 它支持webPack-dev-server的所有选项

    devServer: {
        port: 8080, // 端口号
        host: 'localhost',
        https: false, // https:{type:Boolean}
        open: true, //配置自动启动浏览器
        // proxy: 'http://localhost:4000' // 配置跨域处理,只有一个代理
        proxy: {
            '/sell': {
                target: 'http://127.0.0.1/sell',   // 需要请求的地址
                changeOrigin: true,  // 是否跨域
                pathRewrite: {
                    '^/sell': '/'  // 替换target中的请求地址，也就是说，在请求的时候，url用'/proxy'代替'http://ip.taobao.com'
                }
            }

        },  // 配置多个代理
    }
}
```
#### 三、用法
```javascript
getData() {
    var self = this;
    this.$http.get('/sell/orderMaster/queryOrders', {
        params: {
            current: this.current,
            size: this.size
        }
    }).then(function (response) {
        var data = self.common.handleData(response.data);
        self.records = data.records;
        self.total = data.total;
        self.current = data.current;
    });
},
```
#### 四、axios 几种用法
1：GET
```javascript
// Make a request for a user with a given ID
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// Optionally the request above could also be done as
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```
2：POST
```javascript
 axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```
3：通用
```javascript
axios({
  method:'get',
  url:'http://bit.ly/2mTM3nY',
  responseType:'stream'
}) .then(function(response) {
  response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
});
```
4：axios地址
>https://www.npmjs.com/package/axios
#### 五、表单提交 
表单提交用QS
```javascript
this.$http({
    headers: {'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'},
    transformRequest: [data => self.$qs.stringify(data)],
    method: 'post',
    url: 'sell/login',
    data: {
        userId: self.userInfo.userId,
        password: self.userInfo.password
    }
}).then(function (response) {
    self.$router.push({path: '/index'});
})
    .catch(function (error) {
        console.log(error);
    });
```
axios请求加上下面两句
```javascript
  headers: {'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'},
  transformRequest: [data => self.$qs.stringify(data)],
```
> 转自[vue cli-3 配置axios 跨域请求和表单提交](https://blog.csdn.net/qq_36306590/article/details/81746897)
