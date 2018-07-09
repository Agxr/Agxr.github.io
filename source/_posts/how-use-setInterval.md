---
title: how-use-setInterval
date: 2018-05-03 13:42:17
tags: setInterval
---

### 本文将介绍如何在移动端中正确的使用setInterval

- 在正常情况下，使用setInterval就直接利用new Date() 出来的本地时间去进行计时，这在PC端并没有问题，关键是在移动端，特别是ios手机或者safari浏览器中，当当前含有设置setInterval的应用被home切到后台时，这个时候系统机制就将setInterval自动冻结了
- 下面给出一篇参考文章-------[不会被 iOS 停掉的网页定时器](https://imququ.com/post/ios-none-freeze-timer.html)

### 我在项目中使用setInterval的时候，是结合服务器时间 和 本地时间差进行 倒计时计算

```javascript
    // 第一步：从接口获取服务器时间
    var _self = this
    // 首先要拿到接口中给出的过期时间 和 请求接口的当前服务器时间， 作差 moreShopDisTime
    _self.moreShopDisTime = _self.$data.dataList[i].goods3.end_time - _self.$data.dataList[i].goods3.now_time
    _self.setTimeEnevt()
    //if (_self.moreShopDisTime > 0) {
    //    _self.setTimeEnevt()
    //} else {
    //    _self.moreShopFlag = false
    //}
```

<!-- more -->

```javascript
// 第二步：设置setInterval
  function setTimeEnevt () {
      _self.setGetTime(_self.moreShopDisTime) // 渲染DOM界面
      var dateLocal = new Date().getTime()    // 获取本地时间存储在全局变量
      _self.localTime = Math.round(dateLocal / 1000)  // 取秒
      _self.timeSetIntervalObj = setInterval(function () {  // setInterval开启
        var dateLocalTime = new Date().getTime()
        dateLocalTime = Math.round(dateLocalTime / 1000)   //局部变量存储本地时间
        var moreShopServeTime = 0
        if ((dateLocalTime - _self.localTime) <= _self.moreShopDisTime) {
          moreShopServeTime = _self.moreShopDisTime - (dateLocalTime - _self.localTime)
          _self.setGetTime(moreShopServeTime)
        } else {
          _self.moreShopFlag = false
          clearInterval(_self.timeSetIntervalObj)
        }
      }, 1000)
  }
```

```javascript
// 第三步：时间戳格式化
  function setGetTime (timess) {
      var theTime = timess // 秒
      var theTime1 = 0 // 分
      var theTime2 = 0 // 小时
      if (theTime > 60) {
        theTime1 = parseInt(theTime / 60)
        theTime = parseInt(theTime % 60)
        if (theTime1 > 60) {
          theTime2 = parseInt(theTime1 / 60)
          theTime1 = parseInt(theTime1 % 60)
        }
      }
      if (theTime2 > 9) { // 时
        this.timeShopHour = theTime2
      } else if (theTime2 > 0) {
        this.timeShopHour = '0' + theTime2
      } else {
        this.timeShopHour = '00'
      }
      if (theTime1 > 9) { // 分
        this.timeShopMinute = theTime1
      } else if (theTime2 > 0) {
        this.timeShopMinute = '0' + theTime1
      } else {
        this.timeShopMinute = '00'
      }
      if (theTime > 9) { // 秒
        this.timeShopSecond = theTime
      } else if (theTime > 0) {
        this.timeShopSecond = '0' + theTime
      } else {
        this.timeShopSecond = '00'
      }
  }
```
