title: git常用命令
date: 2015-11-30 16:19:54
tags: git
---
### 1.查看分支
查看本地分支:
```
$ git branch
* master
```
<!-- more -->
查看远程分支
```
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/public
```
### 2.新建分支
新建本地分支
```
$git branch test
* master
   test
```
新建远程分支
```
$git push origin test:test   // git push origin test等价
```
远程新建public分支,并把本地master推上去
```
$git push origin master:public
```
### 3.本地切换分支
```
$git checkout test
切换到分支 'test'
```
### 4.删除分支
删除本地分支
```
$git branch -d xx
```
注意:不能删除当前所在分支,例如:当前在test分支
```
$git branch -d test
error: 无法删除您当前所在的分支 'test'。
```
删除远程分支
```
$git push origin  :public
### 5.版本回退
首选确定回退到哪一个版本
<pre><code>$git log
  commit 41d3027a49999a280da3fcc413601cfa4345ca35
  Author: xxxxxx
  Date:  xxxxxx
  xxx
  commit 98c396e520eaeaf59bb0856ff5ccf1cb74cecc74
  Author: xxxxxxxxx
  Date:   xxxxxxxxx
  xxx
</code></pre>

记下下commitId
<pre><code>git reset --hard 41d3027a49999a280da3fcc413601cfa434
</code></pre>

(待续...)

