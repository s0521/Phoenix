---
title: "Phoenix的异步执行JMS"
date: 2020-12-29T10:59:43+08:00
draft: false
typora-root-url: ../../../../static/

---

Phoenix软件具有灵活的部署方式，除了最引人瞩目的NLME的远程集群执行之外，外允许用于异步的方式执行Phoenix的操作对象。

# 异步执行的方式的应用场景是：

我正在筛选基础模型，接下来打算使用同一组参数的初始值，同时考察下加和型、比例型、混合型误差模型，

如果一个个的执行的化相当于串联方式，非常耗人，也无法充分发挥多核的好处。

异步执行允许我们分别提交加和型、比例型、混合型误差模型，让他们同时进行计算，极大的节约时间。

异步执行的关键：JMS，Job Managenment System，作业管理系统

使用完全模式安装Phoenix后，即安装完成了完全版的JMS，但除了本都部署外，最好分开部署。

# JMS的组成部分：

1. Phoenix提交端

2. JQS，作业队列管理节点

3. JPS，作业执行节点

这三部分可以在一台电脑上，也可以分开部署在3个不同的电脑上。

# 部署场景1：全本地部署

1.使用“完全”的方式安装Phoenix

2.启动Phoenix，“文件(file)”→“首选项(preferences)”→“远程执行(remote execution)”→“作业管理系统(JMS)”

3.勾选“允许远程提交(Remote Submit Enabled)”，然后重启Phoenix

![img](/images/Phoenix的异步执行JMS/clip_image001.jpg)

4.导航至Phoenix安装目录下

5.以管理员身份打开2个“命令行(cmd)”对话框，分别输入

jqs.exe / console

jps.exe / console

启动对应的服务。

6.启动Phoenix

至此，就完成了系统的部署，与服务的启动，可以使用远程提交的方式提交Phoenix的执行任务了。 

## 额外的点评：

当前JMS系统使用似乎存在不少bug，所以你高概率可能上述步骤跑不通，但没关系了解以下即可。

# 部署场景2：分开部署

1.首先准备3台拟使用的电脑，必须都是windows系统。

2.确保三台电脑的联通性，打开对对应的防火墙端口。

3.3台电脑上分别以完全版的方式安装Phoenix（也可以用自定义方式，但不在本文说明范围）。

4.Phoenix端，

- 启动Phoenix，导航至“文件(file)”→“首选项(preferences)”→“远程执行(remote     execution)”→“作业管理系统(JMS)”。
- 允许远程提交，
- 将JQS服务的IP地址填写入“JQS Queue Server”文本框。

5.在JQS端，在安装目录下修改“jqs.exe.config”文件，填写License服务器的地址与类型。

6.在JPS端，在安装目录下修改“jps.exe.config”文件，填写License服务器的地址与类型。在安装目录下修改“jobManagement.xml”中，修改“localhost”，替换为JQS服务对应的IP地址。

至此，就完成了系统的部署，与服务的启动，可以使用远程提交的方式提交Phoenix的执行任务了。

## 额外的点评：

同样，当前JMS系统使用似乎存在不少bug，所以你高概率可能上述步骤跑不通，但没关系了解以下即可。

# 此篇文章的目的

目的是小结下最近对JMS的探索，我探索了好久，跑通的次数嘛。。。。。

我只跑通了**1**次。 



Phoenix是一个非常优秀的软件，虽然你读我的文章可能会发现很多它的bug，但这并不影响它的优秀。