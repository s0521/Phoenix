---
title: "nonmem-cn Extension for Visual Studio Code"
date: 2021-02-18T16:40:14+08:00
draft: false
typora-root-url: ..\..\static
---

This extension implements basic **language features** of **NONMEM** for [Visual Studio Code](https://code.visualstudio.com/).

这个扩展在[Visual Studio Code](https://code.visualstudio.com/)实现了**NONMEM**的一些基础的**语言功能**。

# Content 目录

<!--toc-->

# Features 功能

- Syntax highlighting 语法高亮


![](/images/nonmem_cn/Highlight.png)

- Completion (snippets) 自动补全(片段)


![](/images/nonmem_cn/snippets.gif)

- Code block folding 代码块折叠


![](/images/nonmem_cn/Folding.gif)

## Extension Settings 扩展设置

If you expect the suggested auto-completion to appear at the top of the suggestion list, you need to make the following settings:

Search for editor.snippetSuggestions in the settings, there are four options top, bottom, inline, none four options, select top.

如果你期望建议的自动补全出现在建议列表的最上方，则你需要进行如下设置：

在设置中搜索到editor.snippetSuggestions这个设置，其中有四个选项top、bottom、inline、none四个选项，选择top即可。

## Known Issues 已知问题

It is recommended that the ordering of completed content is not very appropriate, and it takes a lot of time to modify the sorting. So is temporarily abandoned for the time being.

建议补全的内容排序不是很恰当，修改排序需要花费大量的时间深入学习VSCode，目前暂时放弃。

## Future Planning feature 未来计划功能

- Outline 代码大纲

- Hover 悬停提示


# Purpose 目的

At present, the existing syntax aids for writing NONMEM control files are too weak to provide users with a good environment for getting started, and there are no tools for Chinese annotations, so this tool is developed. Another purpose of developing this tool is to make some contribution to the community so that people have better tools to use.

目前NONMEM控制文件编写的语法辅助工具都太弱了，没有提供给用户一个好的上手环境，而且也没有提供中文注释说明的工具，所以开发了此工具，为社区做出一些贡献，让大家有更好的工具可供供使用。

# participate and contribute 参与并贡献

The implementation of completion hints, hover hints, syntax highlighting, etc., needs to be done by traversing all enumerations, which is a time-consuming task, and you can participate and contribute your knowledge to make the tool better.

In this extended github repository [https://github.com/s0521/nonmem_cn](https://github.com/s0521/nonmem_cn), I provide two CSV files "Highlight.csv" and "snippets.csv", which can add new lines to the existing content to add content.

I can add your supplementary content directly to the corresponding JOSN file by merging it into JSON format through Excel formula.

补全提示、悬停提示、语法高亮等的实现需要通过遍历所有枚举的方式来完成，这是一个很耗时的工作，你可以参与进来，贡献你的知识使得该工具变的更好。

在此扩展的github仓库 [https://github.com/s0521/nonmem_cn](https://github.com/s0521/nonmem_cn)中我提供了两个CSV文件“Highlight.csv”、“snippets.csv”，其可以参照已有的内容补充新的行，来增加内容。

你补充的内容我可以通过Excel公式合并为JSON格式直接添加到对用的JOSN文件中。

# More NONMEM Learning Resources 更多NONMEM学习资源

## Pirana:

- NONMEM's perfect partner. NONMEM的绝佳好搭档。

- http://www.tri-ibiotech.com/pkpd/product_112.html

- https://www.certara.com/software/pirana-modeling-workbench/

- QQ群：563236369




## Perl speaks NONMEM:

- Common function combination toolkit. 常用功能组合工具包。

- https://uupharmacometrics.github.io/PsN/




## Introduction of common NONMEM models(常见NONMEM模型介绍)：

- https://support.certara.com/forums/forum/35-nlme-nonmem-model-comparisons/