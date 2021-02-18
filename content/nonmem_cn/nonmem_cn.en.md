# nonmem-cn Extension for Visual Studio Code

This extension implements basic language features of NONMEM for [Visual Studio Code](https://code.visualstudio.com/).

English|[Simplified Chinese](/nonmem_cn/nonmem_cn.chs)

## Features

Syntax highlighting.

![](../../static/images/nonmem_cn/Highlight.png)

Completion (snippets)

![](../../static/images/nonmem_cn/snippets.gif)

Code block folding

![](../../static/images/nonmem_cn/Folding.gif)

## Extension Settings

If you expect the suggested auto-completion to appear at the top of the suggestion list, you need to make the following settings:

Search for editor.snippetSuggestions in the settings, there are four options top, bottom, inline, none four options, select top.

## Known Issues

It is recommended that the ordering of completed content is not very appropriate, and it takes a lot of time to modify the sorting. So is temporarily abandoned for the time being.

# Future Planning feature

Outline

Hover

# Purpose

At present, the existing syntax aids for writing NONMEM control files are too weak to provide users with a good environment for getting started, and there are no tools for Chinese annotations, so this tool is developed. Another purpose of developing this tool is to make some contribution to the community so that people have better tools to use.

# participate and contribute.

The implementation of completion hints, hover hints, syntax highlighting, etc., needs to be done by traversing all enumerations, which is a time-consuming task, and you can participate and contribute your knowledge to make the tool better.

In this extended github repository, I provide two CSV files "Highlight.csv" and "snippets.csv", which can add new lines to the existing content to add content.

I can add your supplementary content directly to the corresponding JOSN file by merging it into JSON format through Excel formula.