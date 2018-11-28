---
title: vue-event
tags:
  - vue
  - event-object
comments: true
abbrlink: 6864
categories: vue
date: 2018-04-25 16:35:36
---

### 本文将介绍在使用vue进行项目开发的时候，再给DOM加click之类的事件时，获取DOM事件源

```bash
1.vue事件获取事件对象,vue获取事件源
    例：定义一事件 @click=targetEvn($event, tag)
    在事件声明 targetEvn(evn, tag) {
        console.log(evn) // evn即表示当前事件
        console.log(evn.target) // evn.target即表示当前事件源，获取该DOM节点进行业务逻辑上的功能开发
    }
```
