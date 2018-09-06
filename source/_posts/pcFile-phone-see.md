---
title: pcFile-phone-see
tags: 
  - browser-sync
  - js
abbrlink: 61336
date: 2018-04-28 16:38:49
---

### 本文介绍如何在手机上预览pc电脑上的文件
- 参考链接---[Browsersync能让浏览器实时、快速响应您的文件更改，同时在PC、平板、手机等设备下进项调试。](http://www.browsersync.cn/#install)

```bash
1.全局安装browser-sync
    npm install -g browser-sync
2.打开要执行的项目 (快捷键 shift+右键)
    browser-sync start --server
这个时候不需要用browser-sync提供的ip，用自己本机pc的ip+端口号即可访问，同时也可以在手机上进行访问
```