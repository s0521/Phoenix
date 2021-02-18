---
title: "About 'guiplot' R Language Package"
date: 2020-04-07T10:45:14+08:00
draft: false
typora-root-url: ..\..\static
---

<!--toc-->

# Visual Studio Code的nonmem-cn扩展

这个扩展在[Visual Studio Code](https://code.visualstudio.com/)实现了NONMEM的一些基础的语言功能。

[英文](/nonmem_cn/nonmem_cn.en)|中文

## 功能

语法高亮

![](/../static/images/nonmem_cn/Highlight.png)

自动补全(片段)

![](/../static/images/nonmem_cn/snippets.gif)

代码块折叠

![](/../static/images/nonmem_cn/Folding.gif)



## 扩展设置

如果你期望建议的自动补全出现在建议列表的最上方，则你需要进行如下设置：

在设置中搜索到editor.snippetSuggestions这个设置，其中有四个选项top、bottom、inline、none四个选项，选择top即可。

## 已知问题

建议补全的内容排序不是很恰当，修改排序需要花费大量的时间深入学习VSCode，目前暂时放弃。

## 未来计划功能

代码大纲Outline

悬停提示Hover

-----------------------------------------------------------------------------------------------------------

## 目的

目前NONMEM控制文件编写的语法辅助工具都太弱了，没有提供给用户一个好的上手环境，而且也没有提供中文注释说明的工具，所以开发了此工具，为社区做出一些贡献，让大家有更好的工具可供供使用。

# 参与并贡献

补全提示、悬停提示、语法高亮等的实现需要通过遍历所有枚举的方式来完成，这是一个很耗时的工作，你可以参与进来，贡献你的知识使得该工具变的更好。

在此扩展的github仓库中我提供了两个CSV文件“Highlight.csv”、“snippets.csv”，其可以参照已有的内容补充新的行，来增加内容。

你补充的内容我可以通过Excel公式合并为JSON格式直接添加到对用的JOSN文件中。