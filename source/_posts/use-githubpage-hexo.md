---
title: how-use-githubPage
comments: true
tags:
  - github-page
  - hexo
abbrlink: 45856
date: 2018-04-13 17:29:29
---

### 本文将介绍如何利用hexo脚手架搭建基于github的个人博客

### 个人博客搭建之github+hexo

#### 前篇：准备工作

- 项目开始前先确保已安装 nodejs-[nodejs下载官网](https://nodejs.org/en/download/) 和 git-[git下载官网](https://git-scm.com/download/win)

#### 一、github篇

- 1.注册github账号

<!-- more -->

- 2.新建项目，创建项目名为：username.github.io 【其中username为你github的用户名】，如下图

![创建username.github.io项目](https://agxr.gitlab.io/save-img/image/github/cereate/github11.jpg)  

- 3.创建成功后，点击项目进入到项目里面。选择settings tab栏，下滑找到 github pages ，如下图，然后点击 choose a theme 按钮

![寻找settings下的 github pages](https://agxr.gitlab.io/save-img/image/github/cereate/github12.jpg)  

- 4.随意选择一个主题，然后点击 select theme 按钮进入下一步，继续点击 commit changes 按钮，如下图

![select theme](https://agxr.gitlab.io/save-img/image/github/cereate/github13.jpg)  

> commit theme  

![commit theme](https://agxr.gitlab.io/save-img/image/github/cereate/github14.jpg)  

- 5.返回到项目里面，继续选择 settings tab栏，下滑到github pages，可以看到此时内容发生了些许变化，点击或者在地址栏地址里面输入：https://username.github.io 即可预览此时属于你的博客项目
 
![继续选择settings](https://agxr.gitlab.io/save-img/image/github/cereate/github15.jpg)  

> 博客项目预览    

![github pages 的主题的博客预览](https://agxr.gitlab.io/save-img/image/github/cereate/github16.jpg)  

#### 二、hexo篇

- 1.准备，先将上面的github项目clone到本地，然后将里面的文件全部删掉（除.git文件夹外）然后提交，目的就是清空你的username.github.io项目，为接下来的hexo博客文件作准备

![git提交过程](https://agxr.gitlab.io/save-img/image/github/cereate/github18.jpg)  

> 期间遇到一个push失败问题是因为我有两个github账号，当前git配置和github不对应，因此要现在github该username.github.io项目里面添加合作者  

![github项目添加合作者](https://agxr.gitlab.io/save-img/image/github/cereate/github17.jpg)  

- 2.全局安装hexo。$ npm install -g hexo-cli

> $ npm install -g hexo-cli  

- 3.**初始化**hexo博客项目 $ hexo init <folder_name>

![hexo初始化](https://agxr.gitlab.io/save-img/image/github/cereate/github20.jpg) 

> 到这里停一停，此时将上面生成的初始化hexo文件项目中的所有文件 复制-->粘贴到之前clone到本地的username.github.io项目下，然后  

- 4.打开cmd或者PowerShell。【win+R键出入cmd回车，然后cd到具体目录下，或者在username.github.io项目根目录下按 shift+鼠标右键选择在此处打开Power'Shell窗口】，输入指令：$ hexo serve

> $ hexo serve  

> 等跑起来后打开浏览器，地址栏输入：http://localhost:4000/  即可预览hexo博客  

![localhost:4000](https://agxr.gitlab.io/save-img/image/github/cereate/github21.jpg)

- 5.将hexo博客项目推到 username.github.io 中

```bash
大于hexo 3.0的上传到github的方法： 
安装部署到github插件依赖
  > $ npm install hexo-deployer-git -–save-dev
```

> 然后修改hexo的配置文件 **_config.yml** 【项目根目录下】中的 **deploy** 配置项  

![_config.yml配置文件](https://agxr.gitlab.io/save-img/image/github/cereate/github22.jpg)

> 然后执行hexo 部署指令 $ hexo deploy -g  

```bash
部署网站。
  > $ hexo deploy -g

  参数  描述
  -g, --generate  部署之前预先生成静态文件
```

![_config.yml配置文件](https://agxr.gitlab.io/save-img/image/github/cereate/github23.jpg)

> 预览username.github.io  

![预览username.github.io](https://agxr.gitlab.io/save-img/image/github/cereate/github24.jpg)

#### 深造

- 1.新建博客文章 $ hexo new [layout] <title>

> $ hexo new [layout] <title>  

- 2.修改博客主题--[参考文章](https://www.jianshu.com/p/33bc0a0a6e90)

> 就是自己选好主题后，将clone到的主题文件夹复制到 themes 文件夹下，然后修改 _config.yml配置文件 中的  theme配置项

- 3.绑定自己的域名，指向username.github.io --[参考文章](https://blog.csdn.net/wgshun616/article/details/81019739)

> 自己购买的域名的控制台，操作域名管理 --> 域名解析 ，新添加一条 username.github.io 的解析记录   

![域名管理](https://agxr.gitlab.io/save-img/image/github/cereate/github25.jpg)

> 然后进入username.github.io的项目，选择settings tab栏，找到github pages里面的custom domain 输入你的域名，点击save（保存）按钮，静态会儿然后访问你的域名就直接指向原来的https://username.github.io，高大上完毕    

![github-->custom domain](https://agxr.gitlab.io/save-img/image/github/cereate/github26.jpg)

