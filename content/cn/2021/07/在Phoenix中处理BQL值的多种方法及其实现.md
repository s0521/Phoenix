---
title: "在Phoenix中处理BQL值的多种方法及其实现"
date: 2021-07-06T17:34:11+08:00
draft: false
typora-root-url: ../../../../static/

---

# 引言：

原始数据中“删失（Censor）”的数据点是数据分析中所面临的一项棘手的问题，“Censor（删失）”数据在药代动力学分析中，又可以被划分为“左删失（Left Censoring）”、“右删失（Right Censoring）”等，常提到的“低于定量下限（Below the Quantization Limit, BQL）”一般是一种“左删失（Left Censoring）”，“高于定量上限（After Quantification Limit, AQL）”一般是一种“右删失（Left Censoring）”，本文对“低于定量下限”数据在常见药动学分析中的处理方式的实现做了一些说明介绍。

# 目录

<!---toc--->

# 1.什么是BQL值?

BQL: Below the Quantization Limit，低于量化限

![img](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image002.png)

 

# 2.处理BQL值的多种方法

## 2.1基于文献和监管指南的多种处理方法：

- **《Ways to Fit a PK Model with Some Data Below the Quantification Limit****》所述的多种方法**
-  ——**视为缺失**-当作对待缺失的数据值
-  ————(M1)将BLQ测量的浓度视为缺失(MDV)
-  ——**强制指定一个值**
-  ————(M5, M6)将BLQ替换为任意值，如LLoQ/2 
-  ————(M7)将第一个BLQ替换为0，其余丢弃
-  ——**估计似然**
-  ————M2，估计BLQ点高于LLoQ的概率调整似然函数
-  ————M3 ，估计BLQ点的浓度小于LLoQ的似然
-  ————M4 ，估计BLQ点的浓度小于LLoQ并大于0的似然
-  ——**使用实际值**
-  ————获取LoD以上的所有值
-  ————(根据实验室的标准操作规程，这可能很难)
-  **指南建议**
-  ——**Tmax**前强制指定为0，Tmax后视为缺失

## 2.2不同方法举例：

![img](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image004.png)

**注释：**

对于Phoenix软件的NCA操作对象和ML Model操作对象，如果浓度值不是数字，则视为该数据点是缺失的。

所以对于需要被缺失的数据，可以将其从数据集中删除，或者将浓度值指定未非数值的符号即可将其视为缺失。

## 2.3处理方法类型的划分：

基于“对PK数据使用不同方法计算PK参数”视角对“处理BQL值的多种方法”的分类划分：

![image-20210706182051348](/images/在Phoenix中处理BQL值的多种方法及其实现/image-20210706182051348.png)

# 3.介绍实现

- 3.1.同时适用于“非房室模型分析”和“房室模型分析”的方法
- ——3.1.1视为缺失
- ——3.1.2强制指定为一个具体数值

- ——3.1.3区分Tmax前、Tmax后进行指定
- 3.2.仅适用于“房室模型分析”的方法
- ——3.2.1个体建模

- ——3.2.2群体建模
- Phoenix NLME8.3版本VPC绘制的新特性


## 使用的“数据”与“操作对象”

该数据集用于3.1~3.2.1章节

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image007.png)

通过在Phoenix中的**“BQL”操作对象**实现对低于定量下限数据点的处理，可通过右键数据表格，然后依次在右键菜单中选择“发送到（Send To）”→“数据管理（Data Management）” →“BQL”菜单，将数据集发送至“BQL”操作对象用于处理。

![图形用户界面, 应用程序  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image009.jpg)

在BQL操作对象中，在其“主映射表（Main）”中，按如下截图对数据集进行映射。

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image011.jpg)

## 3.1.同时适用于“非房室模型分析”和“房室模型分析”的方法

### 3.1.1视为“缺失（Missing）”

第一步，按照如下图所使的设置对“BQL”操作对象的“规则设置（Rule Set）”表格进行设置

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image013.jpg)

第二步，执行“BQL”操作对象，执行后产生的结果如下图所示

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image015.jpg)

### 3.1.2a 强制指定为一个具体数值

第一步，按照如下图所使的设置对“BQL”操作对象的“规则设置（Rule Set）”表格进行设置

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image017.jpg)

第二步，执行“BQL”操作对象，执行后产生的结果如下图所示

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image019.jpg)

### 3.1.2b 强制指定为一个具体数值

第一步，按照如下图所使的设置对“BQL”操作对象的“规则设置（Rule Set）”表格进行设置

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image021.jpg)

第二步，执行“BQL”操作对象，执行后产生的结果如下图所示

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image023.jpg)

### 3.1.3a 根据出现位置，进行差异化的替换

第一步，按照如下图所使的设置对“BQL”操作对象的“规则设置（Rule Set）”表格进行设置

![表格  中度可信度描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image025.jpg)

第二步，执行“BQL”操作对象，执行后产生的结果如下图所示

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image027.jpg)

### 3.1.3b 根据出现位置，进行差异化的替换

第一步，按照如下图所使的设置对“BQL”操作对象的“规则设置（Rule Set）”表格进行设置

![表格  中度可信度描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image029.jpg)

第二步，执行“BQL”操作对象，执行后产生的结果如下图所示

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image031.jpg)

### 3.1.4a 将非数字字符替换为指定字符

其他原因造成的数据缺失

处理低于定量下限的数据，有时我们也面临由于其他原因造成的数据确实

![文本  低可信度描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image033.jpg)

第一步，按照如下图所使的设置对“BQL”操作对象的“规则设置（Rule Set）”表格进行设置

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image035.jpg)

第二步，执行“BQL”操作对象，执行后产生的结果如下图所示

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image037.jpg)

### 3.1.4b 将数值与非数值字符按多种规则进行替换

第一步，按照如下图所使的设置对“BQL”操作对象的“规则设置（Rule Set）”表格进行设置

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image039.jpg)

第二步，执行“BQL”操作对象，执行后产生的结果如下图所示

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image041.jpg)

## 3.2.仅适用于“房室模型分析”的方法

### 额外的要求

实现仅适用于“房室模型分析”的方法时，不仅仅需要使用“BQL”操作对象，还额外的需要在用于建模分析的操作对象“ML Model（曾用名Phoenix Model）”中进行额外的设置，才可实现。

### 3.2.1对于个体数据的实现步骤

#### 3.2.1 首先，识别出低于定量下限的浓度值

第一步，按照如下图所使的设置对“BQL”操作对象的“规则设置（Rule Set）”表格进行设置

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image043.jpg)

第二步，执行“BQL”操作对象，执行后产生的结果如下图所示

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image045.jpg)

#### 3.2.1 然后，使用房室模型估计似然

第三步，将“BQL”操作对象执行后产生的结果表格发送至“ML Model（曾用名Phoenix Model）”操作对象，然后按照下图所使完成“选项卡”区域和“映射列表”区域的设置。

小提示：与BQL有关的设置是在“选项卡”区域勾选“BQL”复选框，在映射列表中将“CObsBQL”列映射至“CObsBQL”字段。

![图形用户界面, 文本  中度可信度描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image047.jpg)

#### 3.2.1不同处理方法对房室模型估计的影响

下图分别展示了使用“完整数据集（Full Data）”进行拟合和分别“按照指南建议处理（1_3a Conditional_Tmax_Simulation_Conc）”和“估计BLQ点的浓度小于LLoQ的似然（2_1 Simulation_Conc）”处理拟合得到的药时曲线，与观测得到的血药浓度。

注释：“完整数据集（Full Data）”=“C Missing” + “C Obs”所构成的数据集，除“完整数据集（Full Data）”外其模型拟合时仅使用“C Obs”所构成的数据集，并将“C Missing” 所构成的点按对应BQL规则处理。

![图表, 直方图  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image049.jpg)

### 3.2.2对于群体数据

#### 3.2.2.1群体建模所使用的数据

![图形用户界面, 应用程序, 表格, Excel  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image051.jpg)

![图表  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image053.jpg)

#### 3.2.2.2群体建模中BQL规则的实现步骤

在Phoenix软件中对数据进行“个体药动学建模”与“群体药动学建模”都是通过同一个操作对象“ML Model（曾用名Phoenix Model）”实现，所以设置上也完全一样

步骤一，使用“BQL”操作对象识别出低于定量下限的浓度值

步骤二，在“ML Model”中使用房室模型估计似然

#### 3.2.2.3第一步，识别出低于定量下限的浓度值

第一步，按照如下图所使的设置对“BQL”操作对象的“规则设置（Rule Set）”表格进行设置

![表格  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image055.jpg)

第二步，执行“BQL”操作对象，执行后产生的结果如下图所示

![图形用户界面, 应用程序, 表格, Excel  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image057.jpg)

#### 3.2.2.4第三步，使用房室模型估计似然

![图形用户界面, 文本, 应用程序  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image059.jpg)

#### 3.2.2.5不同处理方法对结果影响

![图形用户界面  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image061.jpg)

### 3.2.3 Phoenix8.3版本VPC绘制的新特性

Phoenix8.3版本针对群体模型的VPC图绘制增加了新的特性，在用户表示出BQL的情况下，可以在“Predictive Check”运行模式下额外绘制在X轴变量不同“Bin（分箱，箱体）”情形下，分别绘制每个“Bin（分箱，箱体）”中BQL数据点占所有数据点的BQL比例图。

#### 3.2.3.1 BQL比例图图

所需的额外设置：

在“ML Model（曾用名Phoenix Model）”操作对象中，勾选“Population（群体）”复选框， 在“Structure（结构）”选项卡页面中勾选“BQL”复选框

在“Structure（结构）”选项卡页面中选择“Predictive Check”运行模式，并勾选“BQL”复选框，然后在对应的下拉列表中选择“BQL Fraction(BQL比例图)”

执行，即可在“Results（结果）”标签页中查看到“Pop PredCheck BQLFraction”图

![图形用户界面  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image063.jpg)

#### 3.2.3.2对应的“VPC图”与“BQL缺失比例图”

![图表, 折线图  描述已自动生成](/images/在Phoenix中处理BQL值的多种方法及其实现/clip_image065.jpg)

# 4.小结：

在Phoenix软件中通过使用“BQL”操作对象并辅以“ML Model”操作对象可以轻松实现多种处理BQL数值的方法，这些方法有的仅适用于“房室模型拟合”，有的同时适用于“非房室模型分析”和“房室模型拟合”。

# 参考资料：

-  《WinNonlin User's Guide》
-  《Maximum Likelihood Models User's Guide》
-  使用低于量化限制的一些数据拟合 PK 模型的方法， [1]"Errata: Ways to Fit a PK Model with Some Data Below the Quantification Limit, Stuart L. Beal, J. Pharmacokin. Pharmacodyn. 28 :481–504 (2001)." Journal of Pharmacokinetics & Pharmacodynamics 29.3(2002):309-309.
-  群体分析中的QL（量化极限）处理：右删失(AQL)的特殊情况，https://support.certara.com/forums/topic/808-samples-under-loq-handling-in-phoenix-cf-nonmem-ahn-method/?hl=censored#entry4195
-  什么是量化？， https://www.certara.com/knowledge-base/what-is-quantification-anyway/
-  如何处理那些blq， https://www.certara.com/knowledge-base/what-to-do-with-those-blqs/
-  群体分析中的 QL（Quantification limits，量化限制）的处理：右侧删失的特殊情况 （AQL）， https://slidetodoc.com/ql-quantification-limits-handling-in-population-analysis-special/
