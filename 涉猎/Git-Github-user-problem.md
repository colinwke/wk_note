---
title: Git/Github user problem
urlname: git-github-user-problem
categories: 涉猎
tags: [git, github]
date: 2016-06-22 17:36:35
---
Past few days, I create a new Github account for devloping, when I use Git pushing my work on Github, something go wrong.

`remote: Permission to USER1/REPOSITORY.git denied to USER2.`

the `USER1` is my new github account, and the `USER2` is my old github account.

I tried a variety of methods, but did not work.

when I skim through the problem [Git's famous “ERROR: Permission to .git denied to user”][1], bottom of problem, a comment note that: [I am using 'Github for windows' ...][2] I tried, and solved my problem.

the problem comes in I login the Github for windows at one time, and do not log out. And it impact the Git working.

the solution is:

1. install the Github for windows

2. click setting -> option -> log out

3. restart Git and have a try

[1]: http://stackoverflow.com/questions/5335197/gits-famous-error-permission-to-git-denied-to-user?answertab=active#tab-top
[2]: http://stackoverflow.com/questions/18565876/cannot-access-remote-git-repository/31010578#31010578