---
title: "文献中公式错误的一些吐槽_CQT建模白皮书中的公式"
date: 2025-08-27T23:34:30+08:00
draft: false
typora-root-url: ../../../../static/

---

[TOC]

# 1.[白皮书](https://doi.org/10.1007/s10928-017-9558-5)中的模型

## **2017年原始论文中的公式**

$$
\Delta QTc_{ijk} = \left( {\theta_{0} + \eta_{0,i} } \right) + \theta_{1} TRT_{j}  \left( {\theta_{2} + \eta_{2,i} } \right)C_{ijk} + \theta_{3} TIME_{j} + \theta_{4} \left( {QTc_{i,j = 0} - \overline{{QTc_{0} }} } \right)
$$

## **发表后[2018年杂志](https://link.springer.com/article/10.1007/s10928-017-9565-6)刊登的错误订正后的公式**

$$
\Delta QTc_{ijk} = \left( {\theta_{0} + \eta_{0,i} } \right) + \theta_{1} TRT_{j} + \left( {\theta_{2} + \eta_{2,i} } \right)C_{ijk} + \theta_{3} TIME_{j} + \theta_{4} \left( {QTc_{i,j = 0} - \overline{{QTc_{0} }} } \right)
$$

## **差异：**

原来TRT项和浓度项间缺少加号“+”，订正后加上了加号“+”。

## **公式中各项的释义：**

•ΔQTc<sub>ijk</sub>表示**受试者i**在**干预措施j**下的**时间k**时相对基线变化的QTc。

•θ<sub>0</sub>表示截距项无干预效应时的总体平均截距。

•η<sub>0,i</sub>表示截距项θ<sub>0</sub>相关的随机效应。

•θ<sub>1</sub>表示干预措施TRT<sub>j</sub>(j=0:安慰剂， j=0:活性药物)相关的固定效应。

•θ<sub>2</sub>表示浓度与ΔQTc<sub>ijk</sub>间假设存在的线性关系时的总体平均斜率。

•η<sub>2,i</sub>表示斜率项θ<sub>2</sub>相关的随机效应。

•C<sub>ijk</sub>表示受试者i在干预措施j下的时间点k时浓度。

•θ<sub>3</sub>表示时间相关的固定效应。

•θ<sub>4</sub>表示基线QTc<sub>i,j=0</sub>相关的固定效应。

•$\overline{{QTc_{0} }}$表示QTc<sub>ij0</sub>的总体平均值，即基线时(=time 0)时QTc值的平均值。

# 2.我对白皮书中公式的评论

虽然2018年更新后，公式的错误得到了一些改善，但仍然存在一些错误，我将我的的评论直接在公式各个项的含义中进行评论，评论放置在大括号{}中呈现。

## **公式中各个项的含义：**

•ΔQTc<sub>ijk</sub>表示**受试者i**在**干预措施j**下的**时间k**时相对基线变化的QTc。

•θ<sub>0</sub>表示截距项无干预效应时的总体平均截距。

•η<sub>0,i</sub>表示截距项θ<sub>0</sub>相关的随机效应。

•θ<sub>1</sub>表示干预措施TRT<sub>j</sub>(j=0:安慰剂， j=0:活性药物)相关的固定效应。

•θ<sub>2</sub>表示浓度与ΔQTc<sub>ijk</sub>间假设存在的线性关系时的总体平均斜率。

•η<sub>2,i</sub>表示斜率项θ<sub>2</sub>相关的随机效应。

•C<sub>ijk</sub>表示受试者i在干预措施j下的时间点k时浓度。

•θ<sub>3</sub>表示时间相关的固定效应。*{公式中时间被描述为“TIME<sub>j</sub>” ，但结合上下文k表示具体时间点，以及干预措施被描述为“TRT<sub>j</sub>”，所以此处“TIME**<sub>j</sub>**”是错误的，正确的表述应该为“TIME**<sub>k</sub>**”}*

•θ<sub>4</sub>表示基线QTc<sub>i,j=0</sub>相关的固定效应。*{公式中各个受试者的基线QTc被描述为“QTc<sub>i,j=0</sub>” ，但结合上下文k表示具体时间点，以及以及下文中该概念被表述为QTc<sub>ij0</sub> ，所以此处“**QTc<sub>i,j=0</sub>**”是错误的，正确的表述应该为“**QTc<sub>i,k=0</sub>**”或者是“QTc<sub>ij0</sub>”}*

•$\overline{{QTc_{0} }}$表示QTc<sub>ij0</sub>的总体平均值，即基线时(=time 0)时QTc值的平均值。



公式改了一遍还是错误只能感慨做些写作时匆忙与粗糙。

# 3.正确的白皮书中公式应该为

$$
\Delta QTc_{ijk} = \left( {\theta_{0} + \eta_{0,i} } \right) + \theta_{1} TRT_{j} + \left( {\theta_{2} + \eta_{2,i} } \right)C_{ijk} + \theta_{3} TIME_{k} + \theta_{4} \left( {QTc_{i,k = 0} - \overline{{QTc_{0} }} } \right)
$$

## **公式中各项的释义：**

•ΔQTc<sub>ijk</sub>表示**受试者i**在**干预措施j**下的**时间k**时相对基线变化的QTc。

•θ<sub>0</sub>表示截距项无干预效应时的总体平均截距。

•η<sub>0,i</sub>表示截距项θ<sub>0</sub>相关的随机效应。

•θ<sub>1</sub>表示干预措施TRT<sub>j</sub>(j=0:安慰剂， j=0:活性药物)相关的固定效应。

•θ<sub>2</sub>表示浓度与ΔQTc<sub>ijk</sub>间假设存在的线性关系时的总体平均斜率。

•η<sub>2,i</sub>表示斜率项θ<sub>2</sub>相关的随机效应。

•C<sub>ijk</sub>表示受试者i在干预措施j下的时间点k时浓度。

•θ<sub>3</sub>表示时间相关的固定效应。

•θ<sub>4</sub>表示基线QTc<sub>i,k=0</sub>相关的固定效应。

•$\overline{{QTc_{0} }}$表示QTc<sub>ij0</sub>的总体平均值，即基线时(=time 0)时QTc值的平均值。

**关于随机效应的一些假设：**

•假设随机效应符合均值为[0,0]的正态分布。

•随机效应矩阵G为无结构的协方差矩阵。

•残差矩阵R符合均值为0的正态分布。

## **英文版各项含义修订后：**

- ΔQTc<sub>ijk</sub> is the change from baseline in QTc for subject i in treatment j at time k; 
- θ<sub>0</sub> is the population mean intercept in the absence of a treatment effect;
- η<sub>0,i</sub>  is the random effect associated with the intercept term θ<sub>0</sub>; 
- θ<sub>1</sub> is the fixed effect associated with treatment TRT<sub>j</sub> (0 = placebo, 1 = active drug); 
- θ<sub>2</sub> is the population mean slope of the assumed linear association between concentration and ΔQTc<sub>ijk</sub>;
- η<sub>2,i</sub> is the random effect associated with the slope θ<sub>2</sub>; 
- C<sub>ijk</sub> is the concentration for subject i in treatment j and time k;
- θ<sub>3</sub> is the fixed effect associated with time; 
- θ<sub>4</sub> is the fixed effect associated with baseline QTc<sub>i,k=0</sub>;
- $\overline{{QTc_{0} }}$ is overall mean of QTc<sub>ij0</sub>, i.e., the mean of all the baseline (= time 0) QTc values. 

It is assumed 

- the random effects are normally distributed with mean [0,0]
- and an unstructured covariance matrix G, 
- whereas the residuals are normally distributed with mean 0 and variance R.

# 其他：

本篇先吐槽至此。

# 参考文献：

[2017:Scientific white paper on concentration-QTc modeling](https://link.springer.com/article/10.1007/s10928-017-9558-5)

[2018:Correction to: Scientific white paper on concentration-QTc modeling](https://link.springer.com/article/10.1007/s10928-017-9565-6)
