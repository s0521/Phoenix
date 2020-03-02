---
title: "Phoenix与NONMEM的链接"
date: 2020-02-26T11:50:20+08:00
draft: false
typora-root-url: ../../../../static/

---

# 背景：

Phoenix WinNonlin / Phoenix NLME / Phoenix IVIVC等Phoenix系列软件是专为参与药物研究与开发的科学家提供了强有力的分析工具，NONMEM是一个通过代码行使用的计算引擎，没有图形界面，那**有没有什么办法把二者结合起来**，使得NONMEM可以通过Phoenix友好的图形界面进行控制与操作呢？

 

**当然是有的**，Phoenix作为一个完整的解决方案平台，拥有友好的兼容性，可以把R、SAS、NONMEM、SigmaPlot等第三方分析工具无缝的集成至Phoenix平台，使我们可以像使用Phoenix本身的一些分析工具一样使用它们。

 

**使用的软件**：Phoenix8.2，NONMEM7.3

**所需授权**：Phoenix WinNonlin / Phoenix NLME / Phoenix IVIVC等任意一种授权皆可。

 

# 操作流程说明：

<!--toc-->

# 1.链接设置：

## 1.1.查找当前电脑中“NONMEM”安装的位置，

比如：“C:\nm73g\run\nmfe73.bat”

![image-20200226115213992](/images/Phoenix与NONMEM的链接/image-20200226115213992.png)             

## 1.2.启动Phoenix软件

## 1.3.打开首选项对话框

点击“编辑（Edit）”菜单→“首选项（Preferences…）”，打开首选项对话框

## 1.4.指定NONMEM7批处理文件位置

选择菜单中的“NONMEM”节点，指定“NONMEM7批处理文件位置(NONMEM7 Batch File Location)”。

例如“C:\nm73g\run\nmfe73.bat”

  ![image-20200226115226633](/images/Phoenix与NONMEM的链接/image-20200226115226633.png)

 

## 1.5.测试

点击“测试(Test)”按钮，测试与NONMEM的链接，测试成功会出现如下图所示提示。

![image-20200226115323391](/images/Phoenix与NONMEM的链接/image-20200226115323391.png)

# 2.案例演示：

## 2.1.新建Phoenix项目，

新建Phoenix项目，命名为“NONMEM01”

## 2.2.导入文件

鼠标右击“数据（Data）”文件夹，选择“导入（Import）”。

![image-20200226115330236](/images/Phoenix与NONMEM的链接/image-20200226115330236.png)

## 2.3.导入

通过导航窗导航至Phoenix安装目录中的“示例（Examples）”文件夹，例如“C:\Program Files (x86)\Certara\Phoenix\application\Examples\NONMEM\LocalRun”，选中所有文件，点击“打开”。

![image-20200226115337980](/images/Phoenix与NONMEM的链接/image-20200226115337980.png)

## 2.4.导入后的截图

导入后可得到下图所示的结果。

![image-20200226115344218](/images/Phoenix与NONMEM的链接/image-20200226115344218.png)

## 2.5.发送至NONMEM Shell

鼠标右击“代码（Code）”文件夹中的“CONTROL5”文件，选择“发送至（Send To）”→“NONMEM”→“NONMEM Shell”。

![image-20200226115349436](/images/Phoenix与NONMEM的链接/image-20200226115349436.png)

## 2.6.选择数据源

为“NONMEM”操作对象中的“Dataset”报表选择外部数据源，将原始数据中的“Residuals”表格作为他们的数据源，然后完成映射列表。

![image-20200226115354457](/images/Phoenix与NONMEM的链接/image-20200226115354457.png)

## 2.7.执行

点击执行按钮，执行“NONMEM”操作对象，执行过程中可以看到一闪而过的调用“NONMEM”产生的日志窗口。

![image-20200226115358993](/images/Phoenix与NONMEM的链接/image-20200226115358993.png)

## 2.8.结果，

在结果子标签中，我们可以看到一系列的结果：附加结果、数据结果、图表结果、文本结果。

![image-20200226115405012](/images/Phoenix与NONMEM的链接/image-20200226115405012.png)

![image-20200226115420789](/images/Phoenix与NONMEM的链接/image-20200226115420789.png)

# 3.案例中“NONMEM”代码的注释

## 代码及注释（下图中绿色的部分为注释）：

![image-20200226115436718](/images/Phoenix与NONMEM的链接/image-20200226115436718.png)

## NONMEM代码文本：

```fortran
$PROB THEOPHYLLINE  POPULATION DATA

$INPUT   ID DOSE=AMT TIME CP=DV WT

$DATA    THEOPP2

  

$SUBROUTINES ADVAN2

 

$PK

;THETA(1)=MEAN ABSORPTION RATE CONSTANT (1/HR)

;THETA(2)=MEAN ELIMINATION RATE CONSTANT (1/HR)

;THETA(3)=SLOPE OF CLEARANCE VS WEIGHT RELATIONSHIP (LITERS/HR/KG)

;SCALING PARAMETER=VOLUME/WT SINCE DOSE IS WEIGHT-ADJUSTED

  CALLFL=1

  KA=THETA(1)+ETA(1)

  K=THETA(2)+ETA(2)

  CL=THETA(3)*WT+ETA(3)

  SC=CL/K/WT

 

$THETA 

(.1,3,5) ;KA

(.008,.08,.5) ;K

(.004,.04,.9) ;CL

$OMEGA BLOCK(3) 6 .005 .0002 .3 .006 .4

 

$ERROR

  Y=F+EPS(1)

 

$SIGMA .4

 

$EST   MAXEVAL=450 PRINT=5

$COV

$TABLE     ID DOSE WT TIME PRED CP

$SCAT      (RES WRES) VS TIME BY ID

$SCAT      PRED VS CP
```

 

# 4.小结

4.1.在Phoenix中“NONMEM Shell”通过读取代码中的“$DATA”语句指定Phoenix需要提供的数据的报表，通过“$INPUT”语句来读取Phoenix中的表格中的列。

4.2.代码执行完毕后，Phoenix会读取工作空间中的文件（图），把它们展示到结果列表中去，并额外的自动生成一些常用的常用的数据结果和图表结果，极大的降低了NONMEM的使用难度，提高了代码工具的易用性。

 

到此本案例就结束了，通过该案例我们可以发现，在Phoenix中调用NONMEM非常的方便，可以提高NONMEM代码的可重用性，完善NONMEM的稽查轨迹，这是Phoenix强大且易用的一种体现。

 

# 相关链接：

[Phoenix友好的兼容性：Phoenix与R语言的链接](http://www.tri-ibiotech.com.cn/Appofcase/n797.html)

[Phoenix友好的兼容性：Phoenix与SAS的链接](/cn/2019/11/phoenix%E4%B8%8Esas%E7%9A%84%E9%93%BE%E6%8E%A5/)

