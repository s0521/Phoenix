---
title: "Git的一些常用基本操作"
date: 2019-03-23T20:10:31+08:00
draft: false
---
本文是我整理的一些我个人认为的一些`git常用基础操作`

* 1.`查看`当前远程`连接`的仓库
git remote -v
* 2.`提交`至本地`缓存`
git add 文件名称
* 2.2 提交全部文件
git add .
* 3. `提交`至本地待上传`库`
git commit --m "写上传的备注，必须"
* 3.2 提交本地缓存和提交至本地待上传库步骤合并操作
git commit -am "写上传的备注，必须"
* 4.`推送`本地带上传库至`服务器库`
git push 本地连接的远程仓库所使用的本地的名字常用origin 推送至的目标库的目标分支常用master  
git push origin master
* 5.`设置远程库`地址与本地名称
git remote add 本地名称常用origin  git@github.com:用户名/远程的仓库库名.git  
git remote add origin  git@github.com:s0521/远程的仓库库名.git
* 5.2移除当前本地库与远程仓库的连接
git remote rm 本地库的库名常用origin  
git remote rm origin
* 6.本地库文件状态`查看`
git status -s  
返回标志解释
    未添加至 本地缓存  
    已添加至 本地缓存  
    已添加至 本地缓存，但添加后发生了修改
* 7.其他解释
> git commit、git push、git pull、 git fetch、git merge 的含义与区别
    git commit：是将本地修改过的文件提交到本地库中；
    git push：是将本地库中的最新信息发送给远程库；
    git pull：是从远程获取最新版本到本地，并自动merge；
    git fetch：是从远程获取最新版本到本地，不会自动merge；
    git merge：是用于从指定的commit(s)合并到当前分支，用来合并两个分支。

# 测试下格式
```git
git remote -v
git add '文件名称'
```
[超链接测试必应](http://www.bing.com)