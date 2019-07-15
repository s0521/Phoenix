---
title: "Phoenix_NLME的远程Linux计算节点搭建"
date: 2019-06-19T15:00:06+08:00
draft: false
---

Phoenix NLME可以将计算任务发送至远程的计算平台进行计算，来加速计算工作。
发送至远程平台的方式又可以分为“NLME作业控制系统（NLME Job Control System）”和“Phoenix作业管理系统（Phoenix Job Management System，Phoenix JMS）”，本文仅针对前者“NLME作业控制系统（NLME Job Control System）”进行介绍。

### 1. 搭建“NLME作业控制系统（NLME Job Control System）”所需环境：

##### 1.1 安装有Phoenix软件的Windows端：  
操作系统：Window7，Window8.1，Window10,64位操作系统  
所需软件：Phoenix8.1  
所需许可证：拥有Phoenix NLME许可证（License）  
系统语言：中文，英文等（参见Phoenix安装需求）
IP地址：192.168.31.91  
用途概述：作为NLME任务的构建，提交段，是计算任务的发起端。  

##### 1.2 接受任务的Linux主节点：
操作系统：CentOS Linux 7(我是用的是它，其他未测试过)  
所需软件： 
 
* 必须：GCC，R，ksh；  
* 可选（我安装过程中遇到坑，认为一下也是需要的软件，但官方为列出的）：epel-release，libxml2-devel
* R包： batchtools，XML，reshape，Certara.NLME8（从Phoenix的安装目录中获取）

所需许可证：Windows端有即可，Linux端无需额外许可证  
系统语言：英文（安装过程可以使用中文的界面，但安装完成后用于接受”作业（Job）“的系统必须为英文，否则会因为Windows段的Phoenix无法识别返回的中文日期而发生错误）  
IP地址：192.168.31.130  
用途概述：在”MultiCore“模式下，该节点可用与执行NLME的任务，如果不是仅仅这台计算linux计算机作为计算节点，而是组成集群，则需要额外的软件，如：作业管理系统软件（SGE，TORQUE，LSF），如果组成组成并行集群，则需要安装并行化软件，如OpenMPI。
本次搭建的系统仅用于在”MultiCore“模式下执行”作业“，在后续文章中将介绍”TORQUE“的搭建。

##### 1.3 计算节点：
操作系统：CentOS Linux 7  
所需软件：  
 
* 必须：GCC，R，ksh；  
* 可选（我安装过程中遇到坑，认为一下也是需要的软件，但官方为列出的）：epel-release，libxml2-devel
* R包： batchtools，XML，reshape，Certara.NLME8（从Phoenix的安装目录中获取）
系统语言：英文（安装过程可以使用中文的界面，但安装完成后用于接受”作业（Job）“的系统必须为英文，否则会因为Windows段的Phoenix无法识别返回的中文日期而发生错误）

所需许可证：Windows端有即可，Linux端无需额外许可证  
系统语言：英文  
IP地址：192.168.31.130  
用途概述：”计算节点“与”接受任务的Linux主节点“可以是同一台计算机，也可以是不同的计算机，在”MultiCore“模式下必须是同一台，所以本例中”计算节点“与”接受任务的Linux主节点“是同一台计算机。

### 2. 安装过程

##### 2.1安装有Phoenix软件的Windows端：
略，详情参见Phoenix的用户手册。

##### 2.2接受任务的Linux主节点：
#在Linux中安装基础环境软件
```linux
yum install epel-release
yum install GCC
yum install R
yum install ksh
yum install libxml2-devel
```

#安装R的包
```r
R
install.packages("batchtools")
install.packages("XML")
install.packages("reshape")
install.packages("Rmpi")
#install.packages("Certara.NLME8") #此包需要手动安装
q()
```

#导航至包含有Certara.NLME8_0.0.1.0000.tar.gz包的目录使用如下命令安装
```linux
R CMD INSTALL Certara.NLME8_0.0.1.0000.tar.gz
```

#启动远程访问服务
```linux
service sshd start
```

##### 2.3计算节点：
本案例无需此节点。

### 3.组态（配置）
##### 3.1接受任务的Linux主节点：
#不建议使用root账户执行作业（”None“和”MultiCore“可以使用root用户，作业管理模式不可以使用root用户，这是来自作业管理软件的限制），使用一下步骤新建Linux用户的账户
```linux
adduser devin
passwd devin #设置密码，此处我设置为a
```

#查找R软件的安装目录
```linux
[root@master ~]# which R
/usr/bin/R
```

#创建用于储存NLME发送过来的任务的目录，路径可更具需要更改，但要确保接受任务的账户（本案例中为devin账户）有权读写该目录
```linux
mkdir -m 777 /var/tmp/nlme
```

##### 3.2安装有Phoenix软件的Windows端：
启动Phoenix，

以此在菜单栏点击
”编辑（Edit）“→”首选项（Perferences）“→”远程执行（Remote Execution）“→”计算节点（Compute Grid）“，
导航至”计算节点（Compute Grid）“配置页面，之后点击页面上的”增加（Add）“按钮新增一个配置

#各个文本框的填写说明

* 用户计算机名称（User machine name）：远程计算的名称，该名称将显示在Phoenix Model的“运行选项（Run Option）”选项卡上的选择框中。
* 启动脚本（Startup script）：在远程主机上执行的脚本，用于设置运行环境。
* 机器名称/IP地址（Machine name/IP address）：实际机器名称或其IP地址。
* 共享文件夹（Shared folder）：应用程序可以在远程计算机上写入结果/临时文件的位置。
* 机器类型（Machine type）：从Windows或Linux中选择。
* R文件夹（R folder）：远程计算机上R的路径。
* 并行模式（Parallel mode）：如果机器类型为Windows，请选择None，MultiCore，MPI。如果机器类型是Linux，请选择None，MultiCore，MPI，TORQUE，SGE，SGE_MPI，TORQUE_MPI。
* 核心数（Number of cores）：此网格上可用的计算核心数。
* 用户（User）：登录主机的用户名。除非使用私钥文件，否则这是使用网格所必需的。
* 密码（Password）：登录主机的密码。除非使用私钥文件，否则这是使用网格所必需的。

#填写示例：
```zhushi
User machine name：test
Machine name/IP address: 192.168.31.130
Shared folder: /var/tmp/nlme
Machine type: Linux
R folder:/bin/R
Parallel mode:MultiCore
Number of cores: 2
User:devin
Password:a
```

### 4. 测试
启动Phoenix，

加载测试用的群体文件，如
C:\Program Files (x86)\Certara\Phoenix\application\Examples\NLME\Pheno.phxproj

选择其中的“Pheno Model”操作对象，然后导航至“运行选项（Run Option）”选项卡上的选择框中。

在“执行在（Execute on）”下拉框中选择“test”选项，点击Phoenix工具栏上的“执行（Execute）”按钮

略等20s左右，即可查看到返回的计算结果。

### 5. 错误排查
如果超过1分钟都没反应，那说明你的操作步骤中可能存在问题，可按如下步骤排查：
1.导航至Linux计算机上的“共享文件夹（Shared folder）” /var/tmp/nlme，查看其中是否有任何文件，如果没有请检查共享目录的设置与Linux账户的配置。

2.查看“共享文件夹（Shared folder）” 下以“DME_”开头的文件夹，在其中找到“NlmeRemote.LOG”文件，打开它了解报错的信息。

3.有时也可查看Windows端上Phoenix软件的Log日志尝试获取报错信息（在Phoenix菜单栏依次点击“帮助（Help）”→“查看日志（View Logs）”）。