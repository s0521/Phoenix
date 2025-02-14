---
title: "hedVplot人体等效剂量可视化图使用说明"
date: 2025-01-19T22:47:16+08:00
draft: false
typora-root-url: ../../../../static/

---

# 工具名称：hedV plot

**h**uman **e**quivalent **d**ose **V**isualization **plot**，hedV plot，人体等效剂量可视化图。

[TOC]

# 工具用途：

**基于**已有的临床前的获得的来自**不同种属**的各种**剂量数据**（特别是"未见不良反应剂量NOAEL"、"最低起效剂量"），分别**采用**2005年**美国**食品药品监督管理局（**FDA**）发布的《Estimating the Maximum Safe Starting Dose in Initial Clinical Trials for Therapeutics in Adult Healthy Volunteers》指南**和**2012年**中国**药品审评中心（**CDE**）发布《健康成年志愿者首次临床试验药物最大推荐起始剂量的估算指**导原则**》**中的方法**，**计算**出这些剂量所对应的**人体等效剂量**（HED），**并**在一张图表中绘制出来进行**可视化**，以便更直观的了解数据间的关系。

# 使用前需要准备的数据与信息：

分子的分子量、毒理研究使用的剂量（特别是"未见不良反应剂量NOAEL"）、药效学研究使用的剂量（特别是最低起效剂量）其他有用的剂量信息。

# 使用步骤：

## 1.在浏览器中输入以下网址打开工具所在的页面：

https://hedv.netlify.app/

![image-20250119230630156](/images/hedVplot人体等效剂量可视化图使用说明/image-20250119230630156.png)

## 2.阅读使用说明

## 3.在“1、输入分子量”中输入分子量。

![image-20250119230709868](/images/hedVplot人体等效剂量可视化图使用说明/image-20250119230709868.png)

## 4.在“3、录入数据”中调整计划录入数据的行数

![image-20250119230725462](/images/hedVplot人体等效剂量可视化图使用说明/image-20250119230725462.png)

## 5.在“3、录入数据”中根据列名信息录入数据

其中“纵坐标”、“剂量”、“物种”为必填信息，如果缺少这些信息本行数据可能无法在图中正确显示。

![image-20250119230757695](/images/hedVplot人体等效剂量可视化图使用说明/image-20250119230757695.png)

### 各个列的含义与用途：

- “纵坐标”：一个纯粹用于辅助绘图的信息，需用户提供，必须；指定该行数据纵坐标数值，用于排版，建议不同行间为连续的整数，不同行的本列数据可以重复。
- “剂量(mg/kg)”：来自与不同研究的关键的剂量数据（必须是以mg/kg为单位的剂量），需用户提供，必须；指定该行数据横坐标的数值。
- “物种”：来自与不同研究的关键的物种数据，需用户提供，必须；该行数据在图中的颜色。
- “研究类型”：说明数据的来源，是来自与毒理、药效还是其他，需用户提供，可选；指定数据的标志符。
- “数据标签”：一些重点想要额外传达给读者的关于本数据的信息，需用户提供，可选；该列信息会作为图中数据标识符的数据数据标签显示在图上。
- “分子量”：分子本身的一个关键信息；用于实现摩尔单位和质量单位间的换算使用，如果上传数据，则会读取上传数据中的该列的信息作为本行的分子量信息；如果选择编辑本表格提供该信息，则需修改“1、输入分子量”中的分子量信息统一修改本表的分子量，不支持直接在表格中此列数据。（如果上传数据后期望该分子量，也许在“1、输入分子量”中进行修改）
- “典型体重(kg)” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是使用的数据来自于美国FDA指南中的数字，如果没有则来自与中国CDE指南中的数字。
- “美国FDA表格 适用体重范围 (kg)” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是使用的数据来自于美国FDA指南中的数字，如果没有则空缺。
- “美国FDA表格 算人体等效剂量 (mg/kg)” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是将“剂量”列中的数据按照美国FDA指南中的表格换算为人体等效剂量后的的数值。
- “中国CDE表格 算人体等效剂量 (mg/kg)” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是将“剂量”列中的数据按照中国CDE指南中的表格换算为人体等效剂量后的的数值。
- “美国FDA系数0.67 算人体等效剂量 (mg/kg)” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是将“剂量”列中的数据按照美国FDA指南中提供的基于换算系数0.67换算为人体等效剂量后的的数值。
- “中国CDE系数0.698 算人体等效剂量 (mg/kg)” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是将“剂量”列中的数据按照中国CDE指南中提供的基于换算系数0.698换算为人体等效剂量后的的数值。
- “中国CDE系数0.698 算人体等效剂量 (mg/kg)” ：不可直接在表格中编辑，会自动基于前面各列的信息衍生出数值；本列是将“剂量”列中的数据按照自定定义的换算系数0.75换算为人体等效剂量后的的数值。


## 6.在“4、绘制的图形”中查看绘制好的图形

## 7. 可选步骤

无

## 8.在“5、下载全部数据”、“6、下载全部图形”、“7、下载word文档”中下载

根据需要可在在“5、下载全部数据”、“6、下载全部图形”、“7、下载word文档”中下载全部数据、全部图形、或者是包含有全部数据与图形以及简单解说的word文档。

![image-20250119231018992](/images/hedVplot人体等效剂量可视化图使用说明/image-20250119231018992.png)

## 9.关闭本页面。

（本网页的所有数据对仅在浏览器本地进行处理，刷新页面会丢失已经设置的信息）
