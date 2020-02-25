---
title: "瑞德西韦Remdesivir_在狨猴体内的药动学试验与结果"
date: 2020-02-25T13:10:59+08:00
draft: false
typora-root-url: ../../../../static/

---

# 前言

瑞德西韦(Remdesivir, GS-5734)是近期热点，在临床试验结束并获得预期结果前，它对目前的疫情没有任何帮助，但这不妨碍我们来从多个角度了解它，本文是从其临床前非人灵长类药动学试验这一视角对该化合物进行一次观察。

<!--toc-->

# 本文实现目标：

通过文献报道信息，获取文中瑞德西韦(Remdesivir, GS-5734)的药代动力学特性。

# 文献来源：

文献在线阅读网址：

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5567817/

标题：Broad-spectrum antiviral GS-5734 inhibits both epidemic and zoonotic coronaviruses

中文标题：广谱抗病毒GS-5734抑制流行和人畜共患的冠状病毒

# 试验材料：

## 试验化合物：

**名字：**瑞德西韦(Remdesivir, GS-5734)

**分子量：**602.576

## 试验药物制剂：

GS-5734与12％磺基丁基醚-β-环糊精（SBE-β-CD）在水(含HCl / NaOH)中配制，pH调节至5.0。

(in vehicle containing 12% sulfobutylether-β-cyclodextin in water (with HCl/NaOH) at pH 5.0 for in vivo studies.)

注射液的体积：未提及

## 试验动物与设施：

试验设施：未明确说明，可能是在animal biosafety level 3 (ABSL3) facilities at UNC Chapel Hill

试验动物：狨猴（marmosets）

只数：3

性别：雄性

## 给药与采样方案：

**给药方案：**血管内静脉注射，注射部位未提及

**采样方案：**0.083、0.25、0.5、1、2、4、8和24h

## 试验样品分析：

**分析方法：**略，见原文。

**样品基质：**血浆

**检测靶标：**瑞德西韦和其代谢产物

# 试验结果：

## 药时曲线（平均）：

![img](/images/瑞德西韦Remdesivir_在狨猴体内的药动学试验与结果/clip_image002.png)

## 血药浓度表：

通过抠图，获得浓度数据

|       | Intermediate Metabolite   中间代谢产物 | 瑞德西韦 | nucleoside   一磷酸核苷 |
| ----- | -------------------------------------- | -------- | ----------------------- |
| Time  | 浅蓝色                                 | 红色     | 深蓝色                  |
| 0.083 | #N/A                                   | #N/A     | #N/A                    |
| 0.25  | 2.3233                                 | 5.79     | 0.5329                  |
| 0.5   | 1.5331                                 | 1.3086   | 1.2302                  |
| 1     | 0.9966                                 | 1.1859   | 1.5968                  |
| 2     | 0.36619                                | 0.5899   | 1.5776                  |
| 4     | 0.0487                                 | 0.07628  | 0.8921                  |
| 8     | #N/A                                   | 0.008643 | 0.33847                 |
| 24    | #N/A                                   | #N/A     | 0.09997                 |

![img](/images/瑞德西韦Remdesivir_在狨猴体内的药动学试验与结果/clip_image004.png)

## 药动学参数：

**文献报道参数:**

无

**采用抠图数据计算所得参数：**

**数据分析使用软件：**Phoenix WinNonlin

瑞德西韦

| Compound   | Parameter   | Units    | Estimate   | 中文参数名                      |
| ---------- | ----------- | -------- | ---------- | ------------------------------- |
| Remdesivir | Dose        | umol/kg  | 16.595417  | 剂量                            |
| Remdesivir | HL_Lambda_z | h        | 0.98896838 | 末端消除半衰期                  |
| Remdesivir | AUClast     | h*umol/L | 7.1609128  | 曲线下面积(0到最后一个可定量点) |
| Remdesivir | AUCINF_obs  | h*umol/L | 7.1732445  | 曲线下面积(0到无穷远)           |
| Remdesivir | Cl_obs      | L/h/kg   | 2.3135162  | 清除率                          |
| Remdesivir | MRTlast     | h        | 0.60327651 | 平均滞留时间                    |
| Remdesivir | Vss_obs     | L/kg     | 1.4307829  | 分布容积                        |

 

其他：

| Compound                | HL_Lambda_z   (h) | Tmax   (h) | Cmax   (umol/L) | AUClast   (h*umol/L) |
| ----------------------- | ----------------- | ---------- | --------------- | -------------------- |
| Intermediate Metabolite | 0.68861437        | 0.25       | 2.3233          | 2.5011725            |
| Nuc                     | 5.9971645         | 1          | 1.5968          | 11.01931             |

 

**数据分析软件截图：**

 

瑞德西韦药动学参数：

![img](/images/瑞德西韦Remdesivir_在狨猴体内的药动学试验与结果/clip_image006.jpg)

数据分析工作流程图：

![img](/images/瑞德西韦Remdesivir_在狨猴体内的药动学试验与结果/clip_image008.jpg)