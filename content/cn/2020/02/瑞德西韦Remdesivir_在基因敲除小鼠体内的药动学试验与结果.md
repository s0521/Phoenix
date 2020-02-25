---
title: "瑞德西韦Remdesivir_在基因敲除小鼠体内的药动学试验与结果"
date: 2020-02-25T13:10:59+08:00
draft: false
typora-root-url: ../../../../static/

---

# 前言

瑞德西韦(Remdesivir, GS-5734)是近期热点，在临床试验结束并获得预期结果前，它对目前的疫情没有任何帮助，但这不妨碍我们来从多个角度了解它，本文是从其临床前啮齿类药动学试验这一视角对该化合物进行一次观察。

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

试验设施：animal biosafety level 3 (ABSL3) facilities at UNC Chapel Hill

试验动物：缺失Ces1c（Ces1c -/-）基因的小鼠(mice genetically deleted for Ces1c (Ces1c−/−))

只数：未明确提及

性别：未明确提及

## 给药与采样方案：

**给药方案：**皮下注射，25mg/kg

**采样方案：**0.25、0.5、1、2、4、6、8和12h。

## 试验样品分析：

**分析方法：**略，见原文。

**样品基质：**血浆

**检测靶标：**瑞德西韦和其代谢产物

# 试验结果：

## 药时曲线（平均，每个时间点3个样品）：

![image-20200225131744815](/images/瑞德西韦Remdesivir_在基因敲除小鼠体内的药动学试验与结果/image-20200225131744815.png)

## 血药浓度表：

通过抠图，获得浓度数据

|      | Ala-Met   丙氨酸代谢物 | 瑞德西韦 | nucleoside   一磷酸核苷 |
| ---- | ---------------------- | -------- | ----------------------- |
| Time | 浅蓝色                 | 红色     | 深蓝色                  |
| 0.25 | 7.959                  | 9.122    | 0.708                   |
| 0.5  | 10.724                 | 9.231    | 2.4587                  |
| 1    | 6.817                  | 5.406    | 4.2419                  |
| 2    | 2.4294                 | 0.7937   | 6.395                   |
| 4    | 0.10935                | 0.038547 | 1.9965                  |
| 6    | 0.033109               | 0.02273  | 1.5729                  |
| 8    | 0.018715               | #N/A     | 0.7786                  |
| 12   | 0.017725               | #N/A     | 0.6842                  |

 ![image-20200225131753585](/images/瑞德西韦Remdesivir_在基因敲除小鼠体内的药动学试验与结果/image-20200225131753585.png)

## 药动学参数：

**文献报道参数:**

t 1/2 = 0.42 h(~25min)

**采用抠图数据计算所得参数：**

**数据分析使用软件：Phoenix WinNonlin**

瑞德西韦

| Compound   | Parameter   | Units    | Estimate   | 中文参数名                      |
| ---------- | ----------- | -------- | ---------- | ------------------------------- |
| Remdesivir | Dose        | umol/kg  | 41.488543  | 剂量                            |
| Remdesivir | HL_Lambda_z | h        | 0.62507987 | 末端消除半衰期*                 |
| Remdesivir | AUClast     | h*umol/L | 11.086999  | 曲线下面积(0到最后一个可定量点) |
| Remdesivir | AUCINF_obs  | h*umol/L | 11.107497  | 曲线下面积(0到无穷远)           |
| Remdesivir | Vz_F_obs    | L/kg     | 3.3683873  | 分布容积                        |
| Remdesivir | Cl_F_obs    | L/h/kg   | 3.7351838  | 清除率                          |
| Remdesivir | MRTlast     | h        | 0.8281135  | 平均滞留时间                    |

*基于药时曲线数字话所得到的平均血药浓度计算的末端消除半衰期与文献报道值差异较大，所以AUCINF_obs，Vz_F_obs，Cl_F_obs也会误差较大；当不使用最后一个观测浓度(6h时的浓度)拟合半衰期时，可得与文献相似的半衰期0.42h。

 

其他：

| Compound   | HL_Lambda_z  (h) | Tmax  (h) | Cmax  (umol/L) | AUClast  (h*umol/L) |
| ---------- | ---------------- | --------- | -------------- | ------------------- |
| Ala-Met    | 1.250399         | 0.5       | 10.724         | 15.144613           |
| Nucleoside | 4.9187603        | 2         | 6.395          | 24.715938           |

 

# **数据分析软件截图：**

瑞德西韦NCA拟合的末端消除：

![image-20200225131808648](/images/瑞德西韦Remdesivir_在基因敲除小鼠体内的药动学试验与结果/image-20200225131808648.png)

数据分析工作流程图：

 ![image-20200225131818849](/images/瑞德西韦Remdesivir_在基因敲除小鼠体内的药动学试验与结果/image-20200225131818849.png)