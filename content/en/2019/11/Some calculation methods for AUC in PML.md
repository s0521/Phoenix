---
title: "Some calculation methods for AUC in PML"
date: 2019-11-19T17:44:04+08:00
draft: false
typora-root-url: ../../../../static/

---

# Scenarios:

One of the operations that is often required to create a model using the Phoenix Modeling Language (PML) is to calculate the AUC. There are two main methods for calculating the AUC in PML, formulas-based and based on integrals-based.

Key words: Phoenix WinNonlin, Phoenix NLME, PML, Phoenix Modeling Language, AUC, AUClast, AUCinf, pAUC, Secondary parameter, Pharmacokinetic, PK/PD, Pharmacometrics

# Formula-based AUC calculation

The AUC calculated based on the formula is equivalent to AUCinf because it is an analytical solution. It can be subdivided into two categories when calculating the AUC based on the formula.

## Implementation 1: "Methods for defining secondary parameters"

Use the "Secondary(二级参数)" statement to define the AUC, such as secondary (AUC=A1Dose/Cl), and then view the value of this parameter in the "Secondary" worksheet under the "Results(结果)" subtab.

```
test(){
deriv(A1 = - (Cl * C)- (Cl2 * (C - C2)))
urinecpt(A0 = (Cl * C))
deriv(A2 = (Cl2 * (C - C2)))
C = A1 / V
dosepoint(A1, idosevar = A1Dose, infdosevar = A1InfDose, infratevar = A1InfRate)
C2 = A2 / V2
error(CEps = 0.136218)
observe(CObs = C + CEps)
stparm(V = tvV)
stparm(Cl = tvCl)
stparm(V2 = tvV2)
stparm(Cl2 = tvCl2)
fixef(tvV = c(, 55, ))
fixef(tvCl = c(, 0.37, ))
fixef(tvV2 = c(, 55, ))
fixef(tvCl2 = c(, 1, ))
secondary(AUC = A1Dose/Cl)
}
```

![1574156746005](/images/PML中AUC的计算方法/1574156746005.png)

## Implementation 2: "Methods for directly defining formulas"

Define the calculation formula of the parameter directly in the model, such as AUC=A1Dose/Cl, and then add the table under the “Run Options(运行选项)” tab (if it is the simulation mode, you can specify the parameter directly in the model table). Specify "AUC" as the output "Varirables(变量)" in the table, and finally you can see the value of this parameter in the "Table01" worksheet under the "Results(结果)" subtab.

```
test(){
deriv(A1 = - (Cl * C)- (Cl2 * (C - C2)))
urinecpt(A0 = (Cl * C))
deriv(A2 = (Cl2 * (C - C2)))
C = A1 / V
dosepoint(A1, idosevar = A1Dose, infdosevar = A1InfDose, infratevar = A1InfRate)
C2 = A2 / V2
AUC = A1Dose/Cl
error(CEps = 0.136218)
observe(CObs = C + CEps)
stparm(V = tvV)
stparm(Cl = tvCl)
stparm(V2 = tvV2)
stparm(Cl2 = tvCl2)
fixef(tvV = c(, 55, ))
fixef(tvCl = c(, 0.37, ))
fixef(tvV2 = c(, 55, ))
fixef(tvCl2 = c(, 1, ))
 
}
```

![1574156816934](/images/PML中AUC的计算方法/1574156816934.png)

 

As you can see, both methods can output the value of AUC, and it looks no different.

## Differences in results between the two formulas-based methods

What is the difference between the two methods?

The main difference between the two methods is that:

The former method, which can be used to calculate the AUC of an individual model, and it can also calculate the AUC of a typical population value in a population model after replacing "Cl" with "tvCl", but it cannot calculate the AUC of each individual in the population model.

The latter method, which can be used to calculate the AUC of an individual model，it can calculate the AUC of each individual in the population model.

Obviously, what we need more when calculating the population model is the value of the individual's AUC in the population model.

# Integrals-based AUC calculation

In addition to the above formula-based method for calculating AUC, Phoenix also allows users to  integrals-based method for calculating AUC. This method is a way of definite integration. So the calculated AUC is equivalent to AUClast or part of the area under the specified time period "Partial Areas AUC(部分曲线下面积)" or AUCall.

## Implementation: “Methods for Integrals-based“

Write a differential equation for calculating AUC in the model, such as deriv (AUC = C), and then add the table under the “Run Options(运行选项)” tab (if it is the simulation mode, you can specify the parameter directly in the model table). Specify "AUC" as the output "Varirables(变量)" in the table, and the time required to report the AUC is specified in "Times", and finally you can see the value of this parameter in the "Table01" worksheet under the "Results(结果)" subtab.

```
test(){
deriv(A1 = - (Cl * C)- (Cl2 * (C - C2)))
urinecpt(A0 = (Cl * C))
deriv(A2 = (Cl2 * (C - C2)))
C = A1 / V
dosepoint(A1, idosevar = A1Dose, infdosevar = A1InfDose, infratevar = A1InfRate)
C2 = A2 / V2
deriv(AUC = C)
error(CEps = 0.136218)
observe(CObs = C + CEps)
stparm(V = tvV)
stparm(Cl = tvCl)
stparm(V2 = tvV2)
stparm(Cl2 = tvCl2)
fixef(tvV = c(, 55, ))
fixef(tvCl = c(, 0.37, ))
fixef(tvV2 = c(, 55, ))
fixef(tvCl2 = c(, 1, ))
}
```

![计算](/images/PML中AUC的计算方法/clip_image003.png)

You can see that there are multiple different AUC values in the result. The value of these AUCs is the value of AUC from 0 to the corresponding time point, so this method is more similar to AUClast.

The “Methods for Integrals-based“ can be combined with the "Methods for defining secondary parameters" or "Methods for directly defining formulas" above to satisfy the calculation of some complex AUCs, such as the AUC value of each individual's 0-6 hour is calculated in the population model, which can be based on the "integration-based method" + "method of directly defining the formula", deriv (AUC = C), pAUC = AUC.

# summary:：

##  Individual model:

Calculate AUCinf, using "Method for defining secondary parameters", secondary(AUC=A1Dose/Cl)

Calculate AUClast, using “Methods for Integrals-based“, deriv(AUC = C)

## Population model:

When calculating the AUCinf of each individual, using "method of directly defining the formula"，AUC=A1Dose/Cl

When calculating the AUClast of each individual, using “Methods for Integrals-based“, deriv(AUC = C)

Other complex situations may require flexible application.
