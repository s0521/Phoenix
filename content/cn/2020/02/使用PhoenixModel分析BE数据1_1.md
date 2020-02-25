---
title: "使用PhoenixModel分析BE数据1_1"
date: 2020-02-25T13:23:51+08:00
draft: false
typora-root-url: ../../../../static/

---

<!--toc-->

# 数据：WinNonlin自带的2×2示例，

## 模型：2×2数据对应的默认模型

### 算法：REML

#### SAS结果：

协方差：

![img](/images/使用PhoenixModel分析BE数据1_1/clip_image001.png)

固定效应：

![img](/images/使用PhoenixModel分析BE数据1_1/clip_image003.jpg)

#### WinNonlin结果:

协方差：

![img](/images/使用PhoenixModel分析BE数据1_1/clip_image004.png)

固定效应：

![img](/images/使用PhoenixModel分析BE数据1_1/clip_image005.png)

-----分割线-----

### 算法：ML

#### SAS结果：

协方差：

![img](/images/使用PhoenixModel分析BE数据1_1/clip_image006.png)

固定效应：

![img](/images/使用PhoenixModel分析BE数据1_1/clip_image008.jpg)

#### Phoenix Model的ML结果：

协方差：

![img](/images/使用PhoenixModel分析BE数据1_1/clip_image010.jpg)

![img](/images/使用PhoenixModel分析BE数据1_1/clip_image011.png)

即：

|              | SD        | Var        |
| ------------ | --------- | ---------- |
| Residual     | 0.2189746 | 0.04794987 |
| Subject(Seq) |           | 0.08813012 |

固定效应：

![img](/images/使用PhoenixModel分析BE数据1_1/clip_image009.png)

# 相关链接：

[使用非线性混合效应模型做BE数据分析](/cn/2020/02/使用非线性混合效应模型做be数据分析/)