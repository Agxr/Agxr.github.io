---
title: Hello World
comments: true
toc: true
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### 利用hexo搭建基于github的个人博客
```bash
1.安装hexo脚手架
    $ npm install -g hexo-cli
2.利用hexo脚手架生成博客项目
    $ hexo init <ProjectName>
    $ cd <ProjectName>
    $ npm install
3.本地运行hexo项目.启动服务器。默认情况下，访问网址为： http://localhost:4000/。
    $ hexo server
```

<!-- more -->

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
启动服务器。默认情况下，访问网址为： http://localhost:4000/
    $ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
生成静态文件。
    $ hexo generate
    缩写 $ hexo g
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
部署网站。
    $ hexo deploy
    缩写 $ hexo d
```
> 并行指令生成静态文件 + 部署网站
**$ hexo g && hexo d**

More info: [Deployment](https://hexo.io/docs/deployment.html)
