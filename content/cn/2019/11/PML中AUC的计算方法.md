---
title: "PML中AUC的计算方法"
date: 2019-11-19T17:44:04+08:00
draft: false
typora-root-url: ../../../../static/

---

# 场景：

使用Phoenix建模语言(Phoenix Modeling Language,PML)制作模型的时候经常需要的一个操作是计算AUC，在PML中计算AUC的方法主要有两种，基于公式与基于积分。

 

# 基于公式的AUC计算

基于公式计算得到的AUC因为是解析解，所以其实相当于AUCinf，具体在基于公式计算AUC时又可以细分为两类，

## 实现方式1：“定义二级参数的方法”

使用“二级参数（Secondary）”语句定义AUC，例如secondary(AUC=A1Dose/Cl)，然后在“结果（Results）”子标签页下的“Secondary”工作表中查看该参数的值。

```r
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
#int
fixef(tvV = c(, 55, ))
fixef(tvCl = c(, 0.37, ))
fixef(tvV2 = c(, 55, ))
fixef(tvCl2 = c(, 1, ))
secondary(AUC = A1Dose/Cl)
}
```

![1574156746005](/images/PML中AUC的计算方法/1574156746005.png)

## 实现方式2：“直接定义公式的方法”

直接在模型中定义参数的计算公式，例如AUC=A1Dose/Cl，然后再“运行选项（Run Options）”选项卡下增加自定义报表（如果是模拟模式，则可以直接在模型报表中指定该参数），在其中定义输出AUC这个“变量（Varirables）”，最后就可以在“结果（Results）”子标签页下的“Table01”工作表中查看到该参数的值。

```R
test(){
#PK
    deriv(A1 = - (Cl * C)- (Cl2 * (C - C2)))
    urinecpt(A0 = (Cl * C))
    deriv(A2 = (Cl2 * (C - C2)))
    C = A1 / V
    dosepoint(A1, idosevar = A1Dose, infdosevar = A1InfDose, infratevar = A1InfRate)
    C2 = A2 / V2
    AUC = A1Dose/Cl
#Observe
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

 

可以看到，这两种方式都可以输出AUC的值，看起来没什么差别。

## 两种公式定义方法得到结果的差异

这两种方法的差别是什么呢？

这两种方法的主要差异在于：

前者可用于计算个体模型的AUC，已及在将“Cl”替换为“tvCl”后计算群体模型中群体典型值的AUC，但没有办法计算群体模型中每个个体的AUC。

后者既可以用于计算个体模型的AUC，也可以用于计算群体模型中个体的AUC值．

显然在计算群体模型的时候我们更多的需要的是群体模型中个体的AUC的值。

 

# 基于积分的AUC计算

除了上述基于公式的计算AUC的方法，Phoenix也允许用户基于积分的方法计算AUC，这种方法是定积分的方式，所以求算出算的AUC相当于AUClast或者指定时段的部分曲线下面积(Partial Areas AUC)或者AUCall。

## 实现方式：“基于积分的方法“

在模型中写入计算AUC的微分方程，例如deriv(AUC = C)，然后再“运行选项（Run Options）”选项卡下增加自定义报表（如果是模拟模型，则可以直接在模型报表中指定该参数），在其中定义输出AUC这个“变量（Varirables）”并在“Times”中指定需要报告AUC的时间，最后就可以在“结果（Results）”子标签页下的“Table01”工作表中查看到该参数的值。

```R
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

可以看到结果中有多个不同的AUC的值，这些AUC的值就是从0时刻其到到对应时间点的AUC的值，所以说这种方式更类似与AUClast。

“基于积分的方法“可以与上述的“定义二级参数的方法”或者“直接定义公式的方法”进行组合，以满足一些复杂AUC的计算，比如在群体模型中计算每个个体0-6小时的AUC的值，这可以采用“基于积分的方法“+“直接定义公式的方法”，deriv(AUC = C) ，pAUC=AUC。

 

# 小结：

## 个体模型：

计算AUCinf，用“定义二级参数的方法”，secondary(AUC=A1Dose/Cl)

计算AUClast，用“基于积分的方法“，deriv(AUC = C)

## 群体模型：

计算每个个体的AUCinf时，用“直接定义公式的方法”，AUC=A1Dose/Cl

计算每个个体的AUClast时，用“基于积分的方法“，deriv(AUC = C)

其他的一些复杂情况，可能需要灵活应用。

 

 

 

 

 

 