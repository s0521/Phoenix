---
title: "FurtPlot首次人体的剂量与浓度可视化图使用说明"
date: 2025-01-13T21:42:21+08:00
draft: false
typora-root-url: ../../../../static/

---

# 工具名称：Furt Plot

First in hUman dose and concentRaTion plot，Furt Plot，首次人体的剂量与浓度可视化图。

[TOC]

# 工具用途：

**基于**已有的临床前的获得的**不同来源**的**浓度数据**（特别是亲和力常数Kd、产生生物学的效应的最低浓度，最低有效剂量的Cmax，毒理研究的的Cmax），将这些浓度数据的单位**换算为同一种单**位比如ng/mL，之**后**在一张图表中绘制出来**进行可视化**，最后在尝试**基于**用户组定义的药物**分布容积**衍生**计算**出这些浓度所对应的**剂量**，**并**对这些剂量数据也进**行可视化**，以便更直观的了解数据间的关系，以及与实际的或者可能的人体的浓度数据进行对比。

# 使用前需要准备的数据与信息：

分子的分子量、药理试验测得亲和力常数Kd、毒理研究测得Cmax，其他有用的药理学或药效学对应的药物的浓度数据。

# 使用步骤：

## 1.在浏览器中输入以下网址打开工具所在的页面：

https://furt.netlify.app/

![图片包含 表格  描述已自动生成](/images/FurtPlot首次人体的剂量与浓度可视化图使用说明/clip_image002.jpg)

## 2.阅读使用说明

## 3.在“1、输入分子量”中输入分子量。

![文本  描述已自动生成](/images/FurtPlot首次人体的剂量与浓度可视化图使用说明/clip_image003.png)

## 4.在“3、录入数据”中调整计划录入数据的行数

![图片包含 文本  描述已自动生成](/images/FurtPlot首次人体的剂量与浓度可视化图使用说明/clip_image004.png)

## 5.在“3、录入数据”中根据列名信息录入数据

其中“纵坐标”、“浓度数据1”、“单位”为必填信息，如果缺少这些信息本行数据可能无法在图中正确显示。

![表格  描述已自动生成](/images/FurtPlot首次人体的剂量与浓度可视化图使用说明/clip_image006.jpg)

### 各个列的含义与用于：

- “纵坐标”：一个纯粹用于辅助绘图的信息；指定该行数据纵坐标数值，用于排版，建议不同行间为连续的整数，不同行的本列数据可以重复。

- “浓度数据1”：来自与不同研究的关键的浓度数据；指定该行数据横坐标的数值。

- “浓度数据2”：来自与不同研究的关键的浓度数据；如果想要绘制箭头，则需指定该列的数据，该列与“浓度数据1”组成了箭头的左右两端的坐标，如果留空则不会绘制箭头。

- “单位”：来自与不同研究的关键的浓度数据所对应的单位；由于不同研究间习惯使用的单位不同，因此要想把他们汇总在一起绘制图形必须进行单位换算，而单位换算的前提是知道数据原始对应的单位是什么，所以提供本例供填写原始单位，后续会将数据统一分别换算为以“ng/mL”和“nmol/L”为单位的数据供绘图使用。

- “数据标签”：一些重点想要额外传达给读者的关于本数据的信息；该列信息会作为图中数据标识符的数据数据标签显示在图上。

- “分子量”：分子本身的一个关键信息；用于实现摩尔单位和质量单位间的换算使用，如果上传数据，则会读取上传数据中的该列的信息作为本行的分子量信息；如果选择编辑本表格提供该信息，则需修改“1、输入分子量”中的分子量信息统一修改本表的分子量，不支持直接在表格中此列数据。（如果上传数据后期望该分子量，也许在“1、输入分子量”中进行修改）

- “换算后浓度1（nmol/L）” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是将“浓度数据1”换算为以nmol/L为单位后的数值。

- “换算后浓度2（nmol/L）” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是将“浓度数据2”换算为以nmol/L为单位后的数值。

- “换算后浓度3（ng/mL）” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是将“浓度数据1”换算为以ng/mL为单位后的数值。

- “换算后浓度4（ng/mL）” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是将“浓度数据2”换算为以ng/mL为单位后的数值。


## 6.在“4、绘制的图形”中查看绘制好的图形

## 7. 可选步骤，绘制“剂量”图

如果需要基于浓度数据衍生出的“剂量”图，需在填写“假设1:分布容积相对体重的占比为”中的数据，用于大分子静脉注射给药剂量预测时可基于单抗典型的分布容积3~6L酌情填写为5~10中的一个数值（3L/60kg=0.05=5%, 6L/60kg=0.1=10%），用于其他给药图形或者小分子药物时，读者需自行酌情填写合适的数值。（有时大分子其他给途径基于继续使用5-10也是合适的，小分子由于分布容积与血液体积相关性不大，可自行酌情填写，比如动物的没千克体重对应的分布容积作为一个粗略的估计值）

![文本, 信件  描述已自动生成](/images/FurtPlot首次人体的剂量与浓度可视化图使用说明/clip_image008.jpg)

## 8.在“5、下载全部数据”、“6、下载全部图形”、“7、下载word文档”中下载

根据需要可在在“5、下载全部数据”、“6、下载全部图形”、“7、下载word文档”中下载全部数据、全部图形、或者是包含有全部数据与图形以及简单解说的word文档。

![文本, 信件  描述已自动生成](/images/FurtPlot首次人体的剂量与浓度可视化图使用说明/clip_image009.png)

## 9.关闭本页面。

（本网页的所有数据对仅在浏览器本地进行处理，刷新页面会丢失已经设置的信息）