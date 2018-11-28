# agxr.github.io
Using GitHub to build personal blog, welcome you to correct me, thank you.

### 在线浏览Agxr的博客文件
- 浏览器地址栏输入url: **https://agxr.github.io/**
- 或者点击这里**[agxr的个人博客](https://agxr.github.io/)**

---
### 新建hexo的博客文件
$ hexo new file_name
> hexo也提供了更组织化的方式来管理资源。这个稍微有些复杂但是管理资源非常方便的功能可以通过将 config.yml 文件中的 post_asset_folder 选项设为 true 来打开。当资源文件管理功能打开后，Hexo将会在你每一次通过 hexo new [layout] <title> 命令创建新文章时自动创建一个文件夹。  

- hexo d 失败的可能原因
安装依赖包： npm install hexo-deployer-git --save
重新执行： $ hexo deploy 即可


---
### 拷贝自己的hexo博客文件

``` bash
$ git clone git@github.com:Agxr/agxr.github.io.git
$ cd agxr.github.io
$ git install
切换hexo分支: $ git checkout -b hexo
拉取远程代码: $ git pull origin hexo
提交本地文件: $ git add .
暂存文件:    $ git commit -m "注释"
push文件:    $ git push origin hexo
```
