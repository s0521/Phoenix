---
title: "R包开发_检查报错"
date: 2020-04-23T15:35:11+08:00
draft: false
typora-root-url: ../../../../static/

---



我正在尝试向CRAN提交一个我自己编写的R包“guiplot”，我写的包的功能与代码质量暂且按下不谈，聊一聊我在该过程中所遇到的一些报错与问题，

本文目的是记录过往遇到的报错用于以后翻看查询，和向其他与我一样的R包初次提交者分享给提交的经验。

 <!--toc-->

# 向CRAN提交R包流程：

1.编写R包

2.在本地使用devtools自检

3.在CRAN页面提交R包

4.CRAN自动检查程序检查

5.CRAN志愿者人工审核

。。。

我当前在第5步这里，之后的流程暂时还未经历

# 第2步所遇到的一些错误

## 1

### 警告信息：

安装加载R包时，出现警告“replacing previous import(取代先前的导入)”

### 简答：

应尽量使用importFrom，替代import以避免依赖的包，不同包间函数名的冲突。

### 参考资料网址：

https://stat.ethz.ch/pipermail/bioc-devel/2017-June/011129.html

## 2

### 报错信息：

执行检查时，检查到“checking package dependencies ...”，发生该报错Namespace dependencies not required :

### 简答：

此报错是由于R包开发规范中要求，在“Namespace ”中导入的R包必须在R包的“DESCRIPTION”文件的“Imports”中也进行声明。

### 参考资料网址：

https://stackoverflow.com/questions/13085481/namespace-dependencies-not-required

## 3

### 报错信息：

在执行检查时，检查到“checking DESCRIPTION meta-information”，发生该报错not with patchlevel 0。

### 简答：

此报错是由于R包开发规范中要求，在Depends: R (>= 3.6.0)依赖的R版本，不要包含最后一位数字所表示的补丁版本，即R的版本号第3位应为0。

### 参考资料网址：

https://stackoverflow.com/questions/48433412/cran-check-warning-dependence-on-r-version-3-4-3-not-with-patchlevel-0

# 在第5步所遇到的一些回复：

## “说明(Description)”文件：

- 不能出现贬低R语言的文本描述（评论其缺点、或者是评价R语言使用上的糟糕感受），
  - CRAN包是由热爱R语言的志愿者进行审核的，所以需要客气点。
- 不能出现“此包”，“本包”， 等之类的称呼当前包，或者表达类似含义的语句。（This package' or similar.）
  - 移除即可

## R代码：

- 函数不能修改全局环境（比如使用<<-赋值符号），
  - 可以使用在一个新的环境中赋值然后修改环境中的指的方式替代
- 函数不能默认情况下将文件写入本地工作目录、包目录等目录中，如果确实需要，可以写入tempdir()中。 
  - 可以在参数中引入输出目录参数，或者在互动UI中提供设置选项

## 许可文件：

- 如果使用MIT许可方式，则应使用CRAN的模板

# R包开发与提交参考书籍与资料：

## 中文：

R包开发的标准姿势

https://www.jianshu.com/p/98d091c33c6f

郑宝童 简书 博客

https://www.jianshu.com/p/5705cedc14e1

## 英文：

R packages by Hadley Wickham

http://r-pkgs.had.co.nz/

Writing R Extensions by R Core Team

https://cran.r-project.org/doc/manuals/r-release/R-exts.html