---
title: "瑞德西韦Remdesivir_在恒河猴体内的药动学试验与结果"
date: 2020-02-19T09:33:31+08:00
draft: false
typora-root-url: ../../../../static/

---

<!--toc-->

# 前言

瑞德西韦(Remdesivir, GS-5734)是近期热点，在临床试验结束并获得预期结果前，它对目前的疫情没有任何帮助，但这不妨碍我们来从多个角度了解它，本文是从其临床前非人灵长类药动学试验这一视角对该化合物进行一次观察。

# 本文实现目标：

通过文献报道信息，获取文中瑞德西韦(Remdesivir, GS-5734)的药代动力学特性。

# 文献来源：

文献在线阅读网址：

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5551389/

标题：Therapeutic Efficacy of the Small Molecule GS-5734 against Ebola Virus in Rhesus Monkeys

中文标题：小分子GS-5734对恒河猴埃博拉病毒的治疗作用

# 试验材料：

## 试验化合物：

**名字：**瑞德西韦(Remdesivir, GS-5734)

**分子量：**602.576

## 试验药物制剂：

GS-5734与12％磺基丁基醚-β-环糊精（SBE-β-CD）在水中配制，使用HCl将pH调节至3.0。(GS-5734 was formulated in water with 12% sulfobutylether-β-cyclodextrin (SBE-β-CD), pH adjusted to 3.0 using HCl.)

注射液的体积为2.0 mL / kg

## 试验动物与设施：

试验设施：科文斯，Covance, Inc. (Madison, WI)

试验动物：恒河猴(Rhesus monkeys (Macaca maniculata))，3只，雄性

## 给药与采样方案：

**给药方案：**10 mg /kg，静脉内推注(推注时长约1分钟)，大腿内测隐静脉（左/右）（right or left saphenous vein），动物麻醉状态下给予药物。

**血液样品采集：**从股静/动脉采集样品，

**采样方案：**给药前，给药后0.083, 0.25, 0.5, 1, 2, 4, 8, 24h。

**额外在下列时间采集PBMC样品**：2, 4, 8, 24h

 

## 试验样品分析：

**分析方法：**略，见原文。

**样品来源：**血液中的血浆和血中的单核细胞(peripheral blood mononuclear cells, PBMCs)

**检测靶标：**瑞德西韦和/或其代谢产物

# 试验结果：

## 药时曲线（平均）：

![img](/images/瑞德西韦Remdesivir_在恒河猴体内的药动学试验与结果/clip_image002.jpg)

健康恒河猴中，静脉内注射GS-5734（瑞德西韦）10 mg / kg ，给药后的药时曲线（平均值±SD，n = 3），其中：血浆GS-5734（黑色），Ala-Met（红色），Nuc（蓝色）; PBMC中的NTP（绿色）

## 血药浓度表：

通过抠图获得浓度数据

|       | 瑞德西韦 | Nuc    | Ala-Met | NTP    |
| ----- | -------- | ------ | ------- | ------ |
| Time  | 黑色     | 蓝色   | 红色    | 绿色   |
| 0.083 | 5.111    | 0.2707 | 1.361   | #N/A   |
| 0.25  | 1.8361   | 0.6858 | 1.5201  | #N/A   |
| 0.5   | 0.9507   | 1.0053 | 1.6452  | #N/A   |
| 1     | 0.3704   | 1.0515 | 0.9661  | #N/A   |
| 2     | 0.06402  | 0.912  | 0.2447  | 33.23  |
| 4     | #N/A     | 0.5562 | 0.0255  | 32.48  |
| 8     | #N/A     | 0.3379 | 0.0109  | 25.401 |
| 24    | #N/A     | 0.101  | #N/A    | 11.378 |

 

![img](/images/瑞德西韦Remdesivir_在恒河猴体内的药动学试验与结果/clip_image004.jpg)

## 药动学参数：

**文献报道参数:**

t 1/2 = 0.39 h

**采用抠图数据计算所得参数：**

**使用软件：Phoenix WinNonlin**

瑞德西韦

| Compound   | Parameter   | Units    | Estimate   | 中文参数名                      |
| ---------- | ----------- | -------- | ---------- | ------------------------------- |
| Remdesivir | Dose        | umol/kg  | 16.595417  | 剂量                            |
| Remdesivir | HL_Lambda_z | h        | 0.38669607 | 末端消除半衰期                  |
| Remdesivir | AUClast     | h*umol/L | 2.0408235  | 曲线下面积(0到最后一个可定量点) |
| Remdesivir | AUCINF_obs  | h*umol/L | 2.0765393  | 曲线下面积(0到无穷远)           |
| Remdesivir | Cl_obs      | L/h/kg   | 7.9918627  | 清除率                          |
| Remdesivir | MRTlast     | h        | 0.32771554 | 平均滞留时间                    |
| Remdesivir | Vss_obs     | L/kg     | 2.9256106  | 分布容积                        |

 

其他：

| Compound | HL_Lambda_z  (h) | Tmax  (h) | Cmax  (umol/L) | AUClast  (h*umol/L) |
| -------- | ---------------- | --------- | -------------- | ------------------- |
| Ala-Met  | 1.1351782        | 0.5       | 1.6452         | 2.2940649           |
| NTP      | 13.380331        | 2         | 33.23          | 508.934             |
| Nuc      | 8.4013504        | 1         | 1.0515         | 8.5658731           |

 

**数据分析软件截图：**

 

瑞德西韦药动学参数：

![img](/images/瑞德西韦Remdesivir_在恒河猴体内的药动学试验与结果/clip_image006.jpg)

数据分析工作流程图：

![img](/images/瑞德西韦Remdesivir_在恒河猴体内的药动学试验与结果/clip_image008.jpg)

 