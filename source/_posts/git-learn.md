---
title: git-learn
tags:
  - git
comments: true
abbrlink: 6864
categories: git
date: 2018-05-09 10:15:00
---


#### 1.git参考学习文章：  
- 01.廖雪峰的[git官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000)
- 02.git多人协作式 git指令操作 及 常见问题---[参考文献](http://www.cnblogs.com/wufangfang/p/6085767.html)
- 03.使用git克隆指定分支的代码--[参考文献](http://www.cnblogs.com/nylcy/p/6569284.html)
- 04.git命令－切换分支--[参考文献](http://blog.csdn.net/u014540717/article/details/54314126)
- 05.[使用git pull文件时和本地文件冲突时的解决办法](http://www.01happy.com/git-resolve-conflicts/)
---------------------------------------
#### 2.自己使用git的一些总结

- 1.git创建本地分支并推送到远程仓库
```bash
// git创建本地分支并推送到远程仓库
  git checkout -b development
  git branch -a    // 查看本机和远程全部分支列表
  git push origin development:development
```

- 2.git分支合并操作
![分支合并冲突命令窗口展示](http://i68.tinypic.com/vsgtmu.jpg)
![分支合并冲突文件内容展示](http://i68.tinypic.com/23m999i.jpg)
```bash
// 例：git将本地development分支合并到远程beta环境【gie merge <branch-name>】
  git checkout beta
  git pull origin beta
  git merge development
  git commit -m ''
  git push origin beta

--> 在合并分支时遇到的冲突问题
1.如下命令行窗口信息所示，当前为beta分支，要执行将develop分支内容合并到beta分支，其中 css/mobile.css 文件发生冲突并自动合并时出现了问题，需要手动更改
》 admin@DESKTOP-K2QDGBL MINGW64 /c/MyFiles/testgit (beta)
》 $ git merge develop
》 Auto-merging css/mobile.css
》 CONFLICT (content): Merge conflict in css/mobile.css
》 Automatic merge failed; fix conflicts and then commit the result.
- 下面时css/mobile.css 文件冲突内容展示
》 body {
》   background-color: #f00;
》 }
》 <<<<<<< HEAD
》 p {
》   font-size: 24px;
》 }
》 =======
》 div, h1, h2, h3, h4, h5, h6, p {
》   font-size: 24px;
》 }
》 ul, li {
》   list-style: none;
》 }
》 >>>>>>> develop
》 
其中“<<<<<<< HEAD” 与 “=======” 之间的内容为当前分支(即：beta分支)的内容，“=======” 和 “>>>>>>> develop” 之间的内容为要合并分支(即：develop分支)的内容
  -- 手动修改需要保存的内容后再正常提交就可以了
```

- 3.git 想在当前分支(beta)基础上恢复另一分支(development)中的某个文件
```bash
// git 想在当前分支(beta)基础上恢复另一分支(development)中的某个文件
  git checkout development <file-name>

```
--同上：git 想在当前分支(develop)合并(或恢复)另一分支(beta)中的某个指定文件
```bash
// git 想在当前分支(例：develop分支)合并(或恢复)另一分支(例：beta分支)中的某个指定文件(例：README.md文件)
  > $ git checkout beta README.md

```

- 4.取消对文件的修改
```bash
// 例：某文件的本地修改完全没有必要，取消修改
    git checkout file_name(文件的路径)
```

- 5.git add 添加错文件 撤销
```bash
git add 添加 多余文件 
这样的错误是由于， 有的时候 可能
    git add . （空格+ 点） 表示当前目录所有文件，不小心就会提交其他文件
    git add 如果添加了错误的文件的话

> 撤销操作
    git status 先看一下add 中的文件 
    git reset HEAD 如果后面什么都不跟的话 就是上一次add 里面的全部撤销了 
    git reset HEAD XXX/XXX/XXX.java 就是对某个文件进行撤销了
```

- 6.git commit 后撤销操作
```bash
如果不小心弄错了 git commit 了。 
先使用 
    git log 查看节点 
        commit xxxxxxxxxxxxxxxxxxxxxxxxxx 
        Merge: 
        Author: 
        Date:
    (可以看到第一个是我刚刚commit的，此时想要撤销该错误的commit) -> 
然后 
    git reset --soft HEAD^   --将最近一次的commit撤销  （不推荐：git reset commit_id）
结束

PS：还没有 push 也就是 repo upload 的时候
    git reset commit_id （回退到上一个 提交的节点 代码还是原来你修改的） 
    git reset –hard commit_id （回退到上一个commit节点， 代码也发生了改变，变成上一次的）
    ***注意：*** 千万的谨慎的用：git reset --hard HEAD，{该指令会将你本地修改的所有文件撤销，活儿就白干了}
```


- 7.git push 添加到远程仓库后的 撤销操作
```bash
// *1.第一种方法：reset模式， git reset --mixed，即其默认的模式，即git reset 版本号。
  > $ git rest HEAD^
-执行上述操作，git将提交回滚到了上个提交记录, 同时清空了暂存区(也称stage或index，下文用stage代替暂存区)，但是工作区仍然保留，所以git status时，显示时当前工作区相对于当前记录的变动。使用这种模式不用害怕吧，他并不会清除你的工作区，你在撤销前做的任何操作都不会消失。
-此时可以用> git status  查看你更改的操作文件

// *2.第二种方法：soft模式，git reset --soft 版本号
  > $ git reset --soft HEAD^
-采用这种模式，git不回清除你的stage区，因此，git status时就显示了stage区相对于second commit的变化。此时工作区是clean 的，而stage区则有变化。 
-用> git status  查看你更改的操作文件

// 3.第三种方式：--hard，git reset --hard 版本号  【需谨慎操作，因为该指令会将工作区，stage区清理干净】

**综上：
git reset –-soft 不会改变stage区，仅仅将commit回退到了指定的提交
git reset –-mixed 不回改变工作区，但是会用指定的commit覆盖stage 区，之前所有暂存的内容都变为未暂存(即工作区)的状态
git reset –-hard 使用指定的commit的内容覆盖stage区和工作区，即此时工作区和暂存区都是干净的。
```

- 8.git查看commit提交记录详情：git log
```bash
1. > git log   --查看所有的commit提交记录【按 向下键 来查看更多，按 Q 键退出查看日志】
2. > git log -p -2
  -p 选项展开显示每次提交的内容差异，
  用 -2 则仅显示最近的两次更新
3. > git log --stat
  --stat，仅显示简要的增改行数统计
4.查看指定commit hashID的所有修改：
    > git show commitId
5.查看某次commit中具体某个文件的修改：
    > git show commitId fileName
6.查看最新的commit
    > git show
```

- 9.当前的分支落后于线上分支2个提交，应执行> git pull origin 分支名称
![git pull 时有可能出现的问题展示](http://i63.tinypic.com/b9ion8.jpg)
```bash
1.当前的分支落后于线上分支2个提交，应执行
  > $ git pull origin 分支名称
来拉取代码
2.此时有可能出现上面截图展示的样子，或下面类似语句描述
    》 mer xxxxx   【为黄色字体】
    》 Please enter a commit message to explain why this merge is necessary,
    》 especially if it merges an updated upstream into a topic branch.
    》 Lines starting with '#' will be ignored, and an empty message aborts
    》 the commit.
解决方法：git 在pull或者合并分支的时候有时会遇到这个界面。可以不管(直接下面3,4步)，如果要输入解释的话就需要:
  1）按键盘字母 i 进入insert模式
  2）修改最上面那行黄色合并信息,可以不修改
  3）按键盘左上角"Esc"
  4）输入":wq",注意是冒号+wq,按回车键即可

```
