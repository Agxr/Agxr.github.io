---
title: sync-to-async
tags: 
  - js
  - callback
comments: true
abbrlink: 16107
categories: js
date: 2018-08-22 10:41:29
---


- 写js逻辑的，特别是调api接口时，这个时候请求都是异步处理的，只能等请求完毕才能做接下来的渲染处理，这个时候就设计到如何将异步问题转为同步问题

```javascript
// 方法一、封装一个callback函数
function callback (fun, data) {
//   console.log(data)
  typeof fun == "function" && fun('回调成功')
}
var newData = {
  'key1': 'aaa',
  'key': 11111
};
callback(function (res) {
  console.log(newData)
  console.log(res)
}, newData)
```

<!-- more -->

- ES6之Promise

```javascript
// 使用Promise
new Promise(function (resolve, reject) {
    ajax
    resolve()
}).then(function () {
  console.log('resolve:请求成功')
}, function () {
  console.log('reject:请求失败')
})
```


