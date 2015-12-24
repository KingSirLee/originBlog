title: git基本配置信息
date: 2015-12-23 11:17:42
tags: git
---
**配置用户名**
`
git config --global user.name userName
`
<!-- more -->
**配置用户email**
`
git config --global user.email youEmail
`

**查看配置**
```
git config -l

color.ui=auto
user.email=youEmail
user.name=userName
core.editor=vim
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master
```

这样子,当你git log时可以看到相应的提交信息