---
title: Git usage
urlname: git-usage
categories: 涉猎
tags: [git]
date: 2017-11-13 18:47:01
---
## Quick setup — if you’ve done this kind of thing before

We recommend every repository include a README, LICENSE, and .gitignore.

### …or create a new repository on the command line

```
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:colinwke/test.git
git push -u origin master
```

### …or push an existing repository from the command line

```
git remote add origin git@github.com:colinwke/test.git
git push -u origin master
```

## error

```
error:
when run: git push -u origin master
 ! [rejected]        master -> master (non-fast-forward)
solution:
git push origin master --force
```

or like

```
git init
git add .
git commit -m "Initial commit"
git remote add origin <project url>
git push -f origin master
```

The `-f` option on `git push` forces the push. If you don't use it, you'll see an error like this:

```
To git@github.com:roseperrone/project.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:roseperrone/project.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first merge the remote changes (e.g.,
hint: 'git pull') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
url: https://stackoverflow.com/posts/18329034/revisions

## 版本回退

``` git
git log # 查看提交log
git reset [ID | HEAD^^]  # 回退commit
git checkout .  # 回退add(回退了commit需要回退add才能撤销到提交之前的更改)
```

### 分支控制

``` git
git checkout -b XXX  # 新建分支
git branch XXX  # 创建分支
git checkout XXX  # 切换(查看)分支
# refs: https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6
```

--

``` git
#取消对文件的修改。还原到最近的版本，废弃本地做的修改。
git checkout -- <file>

#取消已经暂存的文件。即，撤销先前"git add"的操作
git reset HEAD <file>...

#修改最后一次提交。用于修改上一次的提交信息，或漏提交文件等情况。
git commit --amend

#回退所有内容到上一个版本
git reset HEAD^

#回退a.py这个文件的版本到上一个版本  
git reset HEAD^ a.py  

#向前回退到第3个版本  
git reset –soft HEAD~3  

#将本地的状态回退到和远程的一样  
git reset –hard origin/master  

#回退到某个版本  
git reset 057d  

#回退到上一次提交的状态，按照某一次的commit完全反向的进行一次commit.(代码回滚到上个版本，并提交git)
git revert HEAD
--------------------- 
作者：CankingApp 
来源：CSDN 
原文：https://blog.csdn.net/cankingapp/article/details/18312117 
版权声明：本文为博主原创文章，转载请附上博文链接！
https://blog.csdn.net/ligang2585116/article/details/71094887
```

