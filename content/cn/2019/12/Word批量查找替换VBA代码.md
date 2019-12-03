---
title: "Word批量查找替换VBA代码"
date: 2019-12-03T20:35:18+08:00
draft: false
typora-root-url: ../../../../static/
---



最近没什么可写的，把之前的代码翻出来晒晒。

# 编写该代码的目的：

## 初衷

该代码最初用于我自己弄得一个用于批量化写word报告的东西，因为我原来需要写一堆格式化的报告，不同报告间除了数据发生了变化，其他几乎一模一样，我想偷懒（~~主要是我容易写错~~）,就使用VBA写了一段代码用于实现自动化，

将已有的word文档报告改造成模板，

在一个Excel表格中定义一些变量，包括变量名与变量的值（也有一些数组变量）

使用VBA代码批量的将word中的变量名替换为变量的值

然后就可以开心的仅更新Excel表格，批量的生成报告了~

这个理想很美丽，但实际执行过程中，只有简单点的报告可以这样弄，复杂点的报告因为需要可知化的地方很多，实现起来很痛苦，（特别是一些word中语句需要注意单复数的变换，有些参数缺失时需要完全移除语句），所以该工具旨在我这里用于部分报告的生成。

## 现在

现在我已经不用写报告了 :) 

所以再次想到该代码是最近在翻译一些资料时，机翻总是将一些词汇翻译为一些特定的不合时宜的词，然后需要手动改，改多了我有懒了，再次想到了我之前的代码，修修改改一下又能接着使用了~

# 正文

## 代码

```vbnet
'替换word
Public wdApp, WrdDoc '定义公共变量类，wdapp, WrdDoc代表文档

Sub 批量查找替换()
Application.ScreenUpdating = False

'获取目标word文档
Call getwd
'wdapp.Visible = False

'开始替换
 Call StReplace

'清空变量
Set WrdDoc = Nothing
Set wdApp = Nothing
Application.ScreenUpdating = True
MsgBox "转换完成", vbOKOnly, ""
'Application.ScreenUpdating = False
End Sub


Sub getwd()
'获取当前打开的word文档
    Set wdApp = GetObject(, "Word.Application")
    Set WrdDoc = wdApp.Documents(1)
End Sub


Sub StReplace()
Application.ScreenUpdating = False
Dim wt, urow1 As Long, lrow1 As Long
Dim i1 As Long
Dim chaz1 As String, tih1 As String, lsa

Set wt = ThisWorkbook.ActiveSheet

lrow1 = 2
urow1 = wt.Cells(2000, 1).End(xlUp).Row
For i1 = lrow1 To urow1
    chaz1 = wt.Cells(i1, 1).Value
    tih1 = wt.Cells(i1, 2).Value
    If chaz1 = "" Then
    Else
        lsa = czth1(chaz1, tih1)
    End If
Next
Application.ScreenUpdating = False

'MsgBox "转换完成", vbOKOnly, ""
End Sub
```

## 使用方法

把上述代码放在Excel的宏模块中，然后在当前活动的工作表中，A列填须在word中查找的目标（从第二行开始），B列中填对应的替换的值，之后打开需要查找替换的word，然后运行“批量查找替换()”过程即可完成自动的查找替换。

![1575377494218](/images/Word批量查找替换VBA代码/1575377494218.png)

# 小结

希望这段代码也能帮到需要的人。