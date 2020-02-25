---
title: "使用PhoenixModel分析BE数据2_2"
date: 2020-02-25T13:25:52+08:00
draft: false
typora-root-url: ../../../../static/

---

<!--toc-->

# 数据：RSABE示例中的2×4，

## 模型：2×4数据所对应的默认模型

### 算法：REML

#### SAS结果：

协方差：

![img](/images/使用PhoenixModel分析BE数据2_2/clip_image001.png)

固定效应：

![img](/images/使用PhoenixModel分析BE数据2_2/clip_image003.jpg)

#### WinNonlin结果:

协方差：

![img](/images/使用PhoenixModel分析BE数据2_2/clip_image004.png)

固定效应：

![img](/images/使用PhoenixModel分析BE数据2_2/clip_image005.png)

### 算法：ML

#### SAS结果：

协方差：

![img](/images/使用PhoenixModel分析BE数据2_2/clip_image006.png)

固定效应：

![img](/images/使用PhoenixModel分析BE数据2_2/clip_image008.jpg)

#### Phoenix Model的ML结果：

协方差：

![img](/images/使用PhoenixModel分析BE数据2_2/clip_image009.png)

![img](/images/使用PhoenixModel分析BE数据2_2/clip_image011.jpg)![img](file:///C:/Users/FuYongchao/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

 

即：

| Residual         |             |             |
| ---------------- | ----------- | ----------- |
|                  | SD          | VAR         |
| ee               | 0.28959983  | 0.083868062 |
| ee1              | 0.007007845 | 4.91099E-05 |
| stdev0           | 0.33875883  | 0.114757545 |
|                  |             |             |
| Residual_Formu=0 | stdev0+ee   | 0.198625606 |
| Residual_Formu=1 | stdev0+ee1  | 0.114806655 |

 

| nAlphax0 | 0.70841119  |            |
| -------- | ----------- | ---------- |
| nAlphax1 | 0.68779692  | 0.66836388 |
|          |             |            |
|          | L1^2        |            |
|          | L1*L2       | L2^2+L3^2  |
|          |             |            |
| L1       | 0.841671664 | FA(1,1)    |
| L2       | 0.817179607 | FA(2,1)    |
| L3       | 0.024111598 | FA(2,2)    |

 

 

固定效应：

![img](/images/使用PhoenixModel分析BE数据2_2/clip_image010.png)

#  相关链接：

[使用非线性混合效应模型做BE数据分析](/cn/2020/02/使用非线性混合效应模型做be数据分析/)