---
title: git冲突
tags:
  - git
comments: true
abbrlink: 6864
categories: git
date: 2018-05-10 11:24:36
---


#### git操作之解决冲突

> 使用git pull代码时，经常会碰到有冲突的情况，提示如下信息：  

> error: Your local changes to 'c/environ.c' would be overwritten by merge.  Aborting.  
Please, commit your changes or stash them before you can merge.  

> 这个意思是说更新下来的内容和本地修改的内容有冲突，先提交你的改变或者先将本地修改暂时存储起来。  

- 1.git stash 的方式解决 --[参考博客](http://www.01happy.com/git-resolve-conflicts/)

<!-- more -->

```bash
// stash--隐藏
1、先将本地修改存储起来
    > $ git stash
  - 这样本地的所有修改就都被暂时存储起来。使用git stash list可以看到保存的信息：
  - 其中stash@{0}就是刚才保存的标记。
2、pull内容  // 暂存了本地修改之后，就可以pull了。
    > $ git pull origin master
3、还原暂存的内容
    > $ git stash pop stash@{0}  或者 git stash pop
  - 系统提示如下类似的信息：
    - Auto-merging c/environ.c
    - CONFLICT (content): Merge conflict in c/environ.c
  - 意思就是系统自动合并修改的内容，但是其中有冲突，需要解决其中的冲突。有的需要自己手动更改
4、解决文件中冲突的的部分
  打开冲突的文件，会看到类似如下的内容：
    - 发现文件里出现“<<<<<<< HEAD”，=======，“>>>>>>> log_id ”等符号，Git用<<<<<<<，=======，>>>>>>>标记出不同版本的内容，把这些符号都删除
  - 其中<<<<<<< HEAD 和 =====之间的内容是 本地修改的内容，==== 和>>>>>>> log_id 之间的内容就是 pull下来的内容。碰到这种情况，git也不知道哪行内容是需要的，所以要自行确定需要的内容。
  - 解决完成之后，就可以正常的提交了。
5、然后就是正常 git add .; git commit -m "init"; git push origin master等提交操作了
```

- 2.git冲突文件展示  

![git冲突图](http://i63.tinypic.com/i1xsew.jpg)

```bash
发现文件里出现
  “<<<<<<< HEAD”
    console.log(2222);
  “=======”
    console.log("test")
  “>>>>>>> log_id” 等符号，
- Git用<<<<<<<，=======，>>>>>>>标记出不同版本的内容
- 其中“<<<<<<< HEAD” 和 “=====”之间的内容是 本地修改的内容，“====” 和“>>>>>>> log_id” 之间的内容就是 pull下来的内容。
- “<<<<<<< ” 与 “=======” 之间的内容为本地内容(local)，“=======” 和 “>>>>>>>” 之间的内容为拉取下来的代码(即：仓库里面的代码)
```

- 3.git正常提交冲突后的解决方法

> 像这样先add等push时有冲突再解决的操作，是为了保证自己修改的本地代码不会消失（已经commit过了），就是担心自己修改的代码因为git误操作在拉取等时候被覆盖而丢失，这样就白干了，所以先commit，等push时有冲突再说。 --大佬这么教育我的  

```bash
// git提交代码前，先不拉取内容，直接add
1.先查看本地修改文件的状态
    > git status
2.git添加文件
    > git add -A 或 (git add .) 或 (git add file_name)
3.commit提交
    > git commit -m "描述内容"
4.push操作
    > git push origin master  或 (git push origin develop_name)
** 此时如果没有冲突的话就正常提交完毕，可以坐下喝口咖啡压压惊了
** - 但是，然而我们要说的是有冲突的情况给：如果有冲突，则 push 会失败，提示有多种情况，如：
    > $ git push origin develop_name
    To ssh://xxx.xxue.com.cn/xxxxxxx.git
    ! [rejected]        develop_name -> develop_name (non-fast-forward)
    error: failed to push some refs to 'ssh://git@source.zhenxue.com.cn/zhenxue-tasks.git'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. Integrate the remote changes (e.g.
    hint: 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.
  或：
    > $ git push origin master
    To https://github.com/xxxx/testgit.git
     ! [rejected]        master -> master (fetch first)
    error: failed to push some refs to 'https://github.com/Agxr/testgit.git'
    hint: Updates were rejected because the remote contains work that you do
    hint: not have locally. This is usually caused by another repository pushing
    hint: to the same ref. You may want to first integrate the remote changes
    hint: (e.g., 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.
接下来就是解决冲突的操作了
  1）git fetch
  2）git pull origin develop_name (假如你的分支是master，就写master)
- 这个时候可以再git push就好了，但是这里有个问题，就是git pull的时候会自动合并分支，但是不一定一定成功。自动合并需手动改的冲突如下：
    $ git pull origin develop_jixun
    From ssh://source.zhenxue.com.cn/zhenxue-tasks
     * branch            develop_jixun -> FETCH_HEAD
    Auto-merging jixun-admin/js/search-detail.js
    Auto-merging cq-login/js/index.js
    CONFLICT (add/add): Merge conflict in cq-login/js/index.js
    Automatic merge failed; fix conflicts and then commit the result.
  // CONFLICT (add/add): Merge conflict in cq-login/js/index.js。该语句表示这个文件需要手动改
- 或：
    - 我就是遇到这个问题
    CONFLICT (content): Merge conflict in register_result.php
    Auto-merging php_fns.php
    CONFLICT (content): Merge conflict in php_fns.php
    Auto-merging output_fns.php
    CONFLICT (content): Merge conflict in output_fns.php
    Auto-merging item-details.php
    CONFLICT (content): Merge conflict in item-details.php
    Automatic merge failed; fix conflicts and then commit the result.
  3）your code is here
    <<<<<<<</font> HEAD
    another code is here
    =======
    also code is here
    >>>>>>> 6853e5fxxxxxxxxxxxxxxxxxx330dcc
    在这里：<<<<<<<< HEAD 和 =======之间的文件是你的代码
    ============ 和 >>>>>>>>>>> 6853e5fxxxxxxxxxxxxxxxxxx330dcc是别人的代码。
    手动修改好了就行了！！
  4）手动改好后就可以 git add commit push操作了
```

- 4.强制提交就是干

```bash
1、强推
    > git push -f
- 即利用强覆盖方式用你本地的代码替代git仓库内的内容。
```
