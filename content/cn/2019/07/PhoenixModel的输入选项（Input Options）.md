---
title: "PhoenixModel的输入选项（Input Options）"
date: 2019-07-26T10:35:11+08:00
typora-root-url: ..\..\..\..\static
draft: false
---

# 引言

学习群体药代动力学时，刚开始看到数据集中的“MDV”、“SS”、“ADDL”、“Date”、“II”

和“REST”时感觉莫名其妙，脑海里蹦出他是谁？他来自哪？他要干什么！这样的三连问，今天我们就来探索下！

 

# 一、他是谁：

在Phoenix的“输入选项（Input Options）”选项卡我们可以看到一下这几个复选框：

- “重置？(Reset?)”
- “错过此因变量的观测(MDV？，Missing Dependent Variable)”
- “稳态(SS，Steady State)”
- “额外附加剂量(ADDL，AdditionaDose labels)”
- “日期?(Date?)”

![1564108790876](/images/PhoenixModel的输入选项（Input Options）/1564108790876.png)

 

## 作用：

这几个复选框的主要作用是，允许用户额外的为原始数据表格增加些逻辑条件列，以便于用户更灵活的构建数据集，缩减数据集的体积；所以这些需选框不是必须勾选的，不使用这些复选框同样可以构建出等价的满足用户自身需要的数据集，但额外的掌握这几个复选框的使用将使用户可以更灵活便利的构建出需要的数据集。

 

# 二、他来自哪：

这几个复选框的功能来源于对NONME子程序PREDPP的“数据预处理器”对数据进行准备所需的几个数据列的名称，Phoenix兼容了这种以往的数据排布格式。

 

# 三、他要干什么！

Phoenix中每一个复选框的功能概览：

-  “重置？(Reset?)”：当“重置(Reset)”列中的数值为1时，将重置当前个体，将该个体的微分方程的状态变量设置为零，并重新启动模型中的任何“序列(Sequence)”语句。
-  “错过此观测(MDV？)”：当“错过此观测(MDV？)”列中数值为1时，将忽略当前这一行中因变量的观测值。
-  “稳态(Steady State)”：新增“稳态(Steady State)”子选项卡，允许用户在此假定当前个体，已经经过无数给个药周期的循环，到达了稳态。
-  “额外附加剂量(ADDL)”：新增“额外附加剂量(ADDL)”子选项卡，允许用户在此指定额外的给药循环周期内的给药方案，和循环次数。
-  “日期?(Date?)”：新增“日期(Date)”数列，允许用户输入现实世界的日期。

 

# 四、小结：

将在后续的案例中逐个的说明每个复选框的具体功能与示例。

后续介绍的顺序：

５.“重置？(Reset?)”

２.“错过此观测(MDV？)”

４.“稳态(Steady State)”

３.“额外附加剂量(ADDL)”

１.“日期?(Date?)”

 

# 参考：

NONMEM用户手册第6本，第7章PREDPP

NONMEM Users Guide Part VI - PREDPP - Chapter VII

 

Phoenix NLME用户指南，第2章Phoenix Model对象，第一节用户界面

Phoenix NLME User’s Guide - Chapter 2 The Phoenix ModeObject - Object user interface