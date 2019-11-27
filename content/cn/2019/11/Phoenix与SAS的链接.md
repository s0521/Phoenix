---
title: "Phoenix与SAS的链接"
date: 2019-11-27T12:52:58+08:00
draft: false
typora-root-url: ../../../../static/

---

**前言：**

**定量药理学工作平台Phoenix**是**专为参与药物研发的科研工作者**所设计的一款软件，它拥有友好界面以及多种灵活的授权方式（WinNonlin、NLME、IVIVC、PK Submit、Validation Suite），为用户洞察研发过程中所产生的数据提供的强有力数据分析解决方案；SAS是一个统计学中经常被使用到的商业软件，使用它可以完成一些复杂的统计分析功能。

**有没有什么办法把两者结合起来呢？**

 ![image-20191127130204380](/images/Phoenix与SAS的链接/image-20191127130204380.png)

当然是**有的**，Phoenix作为一个完整的解决方案平台，拥有友好的兼容性，可以把R、SAS、NONMEM、SigmaPlot等第三方分析工具无缝的集成至Phoenix平台，使用户可以像使用Phoenix本身的一些分析工具一样使用它们。

**目录**

<!--toc-->

# 1   在Phoenix中进行的链接设置

“编辑（Edit）”→“首选项（Preferences…）”→“SAS”，

设定SAS执行文件的路径。

![image-20191127125502163](/images/Phoenix与SAS的链接/image-20191127125502163.png)



# 2   应用案例

## 2.1 案例基本信息：

### 2.1.1  使用的软件

![image-20191127125515467](/images/Phoenix与SAS的链接/image-20191127125515467.png)



### 2.1.2  案例使用的文件

案例使用的文件为Phoenix自带的示例文件，

如果安装Phoenix时使用默认安装路径，则文件所在路径为：

C:\Program Files (x86)\Certara\Phoenix\application\Examples\SAS\Example1

![image-20191127125525974](/images/Phoenix与SAS的链接/image-20191127125525974.png)



# 3   Phoenix中的操作

 

## 3.1 新建项目

新建Phoenix项目，并重命名为“**sas1**”

![image-20191127125535162](/images/Phoenix与SAS的链接/image-20191127125535162.png)



## 3.2 导入所需文件

右键“sas1”项目文件，选择“导入（Import）”，导入所需的文件

![image-20191127125542801](/images/Phoenix与SAS的链接/image-20191127125542801.png)



## 3.3 发送至SAS Shell

鼠标右键“代码（Code）”储存对象下的“SimpleExampleUsingColumnMappings”文件，

在右键菜单中依次选择“发送到（Send To）”→“SAS”→“SAS Shell”。

![image-20191127125551468](/images/Phoenix与SAS的链接/image-20191127125551468.png)

![image-20191127125559947](/images/Phoenix与SAS的链接/image-20191127125559947.png)

![image-20191127125606966](/images/Phoenix与SAS的链接/image-20191127125606966.png)



## 3.4 映射

![image-20191127125615591](/images/Phoenix与SAS的链接/image-20191127125615591.png)



**Subject → Subject**

**Time → Time**

 **Conc → Concen**

 

 

## 3.5 执行

![image-20191127125625427](/images/Phoenix与SAS的链接/image-20191127125625427.png)

执行过程中的截图

![image-20191127125631448](/images/Phoenix与SAS的链接/image-20191127125631448.png)



## 3.6 结果

![image-20191127125639518](/images/Phoenix与SAS的链接/image-20191127125639518.png)

![image-20191127125651305](/images/Phoenix与SAS的链接/image-20191127125651305.png)



# 4   应用案例中使用到的SAS代码及其注释

 

## 4.1 代码注释：

![image-20191127125658345](/images/Phoenix与SAS的链接/image-20191127125658345.png)

## 4.2 SAS代码：

```R
/* CLEAR LOG AND OUTPUT WINDOWS */

dm 'log; clear; output; clear;';

 

libname indata XPORT "Final Parameters.xpt"; /*WNL_IN Subject Time Concen*/

 

proc copy in=indata out=Work;

run;

 

data finpar;

set Final_Pa;

run;

 

libname outdata XPORT "aucdescrp.xpt";

 

proc copy in=work out=outdata;

select finpar;

run;
```



# 5   小结：

1)   名称为“indata”,“outdata”的永久**数据库**是Phoenix调用SAS时的保留名称，用于Phoenix与SAS之间数据的输入与输出使用。

2)   在Phoenix中调用SAS代码时，SAS代码的典型工作步骤，

1从indata中**读取**数据，

2将读取得到的数据复制到临时数据库，

3对数据进行**操作**，

4将**结果**表格复制到outdata数据库中。




# **姊妹篇**

[Phoenix 友好的兼容性：Phoenix 与 R 语言的链接](http://www.tri-ibiotech.com.cn/Appofcase/n797.html)