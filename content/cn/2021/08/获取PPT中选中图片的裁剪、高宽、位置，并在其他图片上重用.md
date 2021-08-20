---
title: "获取PPT中选中图片的裁剪、高宽、位置，并在其他图片上重用"
date: 2021-08-21T00:04:23+08:00
draft: false
typora-root-url: ../../../../static/

---

# 编写该代码的目的：

## **初衷**

我最近在撰写一些教程类的PPT，撰写过程繁琐、重复，其中有很多操作步骤是一样的，PPT的部分页面时可以重用的，但是PPT页面中对应的截图不能重用，因为有变化；截图、粘贴、裁剪、缩放、移动位置、删除旧图，一顿操作枯燥而乏味。。。。。

我尝试寻找现有工具简化这一过程，努力的搜索与尝试了一番，发现唯一可堪一用的是“Pocket（口袋动画）”中的“设计”→“定位排版”→“对象尺子”、“应用尺子”；此功能可以实现图片的“宽高”、“位置”的获取与重用，这虽然可以减轻一些工作，但“裁剪”这一步仍然费劲，继续搜索而无所获。

于是，该代码代码诞生了！

# 正文

## **代码**

```vbscript
Dim modeler01_CropTop As Single
Dim modeler01_CropRight As Single
Dim modeler01_CropBottom As Single
Dim modeler01_CropLeft As Single

Dim modeler01_ScaleHeight As Single
Dim modeler01_ScaleWidth As Single

Dim modeler01_Height As Single
Dim modeler01_Width As Single

Dim modeler01_Top   As Single
Dim modeler01_Left  As Single
Sub 获取_图片_裁切_高宽_位置信息()
    Call test_获取_图片_裁切信息
    Call test_获取_图片_高宽信息
    Call test_获取_图片_位置信息
End Sub
Sub 设置_图片_裁切_高宽_位置信息()
    Call test_设置_图片_裁切信息
    Call test_设置_图片_高宽信息
    Call test_设置_图片_位置信息
End Sub
Sub 获取_图片_裁切_高宽信息()
    Call test_获取_图片_裁切信息
    Call test_获取_图片_高宽信息
End Sub
Sub 设置_图片_裁切_高宽信息()
    Call test_设置_图片_裁切信息
    Call test_设置_图片_高宽信息
End Sub

Sub test_获取_图片_裁切信息()
    Set shape_1 = ActiveWindow.Selection.ShapeRange(1)
    Set b = shape_1.PictureFormat

    With shape_1.Duplicate
        .ScaleHeight 1, True
        origHeight = .Height
        origWidth = .Width
        .Delete
    End With

    With shape_1
        modeler01_CropTop = .PictureFormat.CropTop / origHeight
        modeler01_CropRight = .PictureFormat.CropRight / origWidth
        modeler01_CropBottom = .PictureFormat.CropBottom / origHeight
        modeler01_CropLeft = .PictureFormat.CropLeft / origWidth
    End With
End Sub

Sub test_设置_图片_裁切信息()
    Set shape_1 = ActiveWindow.Selection.ShapeRange(1)
    With shape_1.Duplicate
        .ScaleHeight 1, True
        origHeight = .Height
        origWidth = .Width
        .Delete
    End With

    With shape_1
        .PictureFormat.CropTop = modeler01_CropTop * origHeight
        .PictureFormat.CropRight = modeler01_CropRight * origWidth
        .PictureFormat.CropBottom = modeler01_CropBottom * origHeight
        .PictureFormat.CropLeft = modeler01_CropLeft * origWidth
    End With
End Sub


Sub test_获取_图片_高宽信息()
    Set shape_1 = ActiveWindow.Selection.ShapeRange(1)
    Set b = shape_1.PictureFormat

    With shape_1
        modeler01_Height = .Height
        modeler01_Width = .Width
    End With

    Debug.Print "   modeler01_Height   "; modeler01_Height
    Debug.Print "   modeler01_Width    "; modeler01_Width
End Sub

Sub test_设置_图片_高宽信息()
    Set shape_1 = ActiveWindow.Selection.ShapeRange(1)

    With shape_1
        .Height = modeler01_Height
        .Width = modeler01_Width
    End With
End Sub

Sub test_获取_图片_位置信息()
    Set shape_1 = ActiveWindow.Selection.ShapeRange(1)
    Set b = shape_1.PictureFormat

    With shape_1
        modeler01_Top = .Top
        modeler01_Left = .Left
    End With
End Sub

Sub test_设置_图片_位置信息()
    Set shape_1 = ActiveWindow.Selection.ShapeRange(1)
    With shape_1
        .Top = modeler01_Top
        .Left = modeler01_Left
    End With
End Sub
```

全部的代码就是上述了，主要是实现了对所选中对所选中图片的“裁切”、“宽高”、“位置”信息的获取与设置，并额外编写了用于串联这几个独立过程的代码，便于一键完成“裁切”、“宽高”、“位置”的获取，然后选一个新的对象，再一键完成“裁切”、“宽高”、“位置”的设置~！

## **使用方法**：

1.我将上述代码处在一个PPT的宏模块中，之后将PPT另存为后缀名为“.ppam”格式，然后设置为PPT的加载项。

2.下载、安装“RunMacro”程序，修改“”安装目录（C:\Program Files\RunMacro）下的“PowerPoint.xml”文件，修改后内容如下：

```xml
<customUI xmlns="http://schemas.microsoft.com/office/2009/07/customui">
	<ribbon startFromScratch="false">
		<tabs>
			<tab id="Tab1" label="RunMacro">
				<group id="Group2" label="Cut">
					<button id="Button3" label="Get_Cut" imageMso="G" size="large" onAction="Shape_Cut_size_position.ppam!模块1.test_获取_图片_裁切信息"/>
					<button id="Button4" label="Set_Cut" imageMso="S" size="large" onAction="Shape_Cut_size_position.ppam!模块1.test_设置_图片_裁切信息"/>
				</group>
				<group id="Group3" label="Size">
					<button id="Button5" label="Get_Size" imageMso="G" size="large" onAction="Shape_Cut_size_position.ppam!模块1.test_获取_图片_高宽信息"/>
					<button id="Button6" label="Set_Size" imageMso="S" size="large" onAction="Shape_Cut_size_position.ppam!模块1.test_设置_图片_高宽信息"/>
				</group>
				<group id="Group4" label="Position">
					<button id="Button7" label="Get_position" imageMso="G" size="large" onAction="Shape_Cut_size_position.ppam!模块1.test_获取_图片_位置信息"/>
					<button id="Button8" label="Set_position" imageMso="S" size="large" onAction="Shape_Cut_size_position.ppam!模块1.test_设置_图片_位置信息"/>
				</group>
				<group id="Group5" label="Cut_Size">
					<button id="Button9" label="Get_Cut_Size" imageMso="G" size="large" onAction="Shape_Cut_size_position.ppam!模块1.获取_图片_裁切_高宽信息"/>
					<button id="Button10" label="Set_Cut_Size" imageMso="S" size="large" onAction="Shape_Cut_size_position.ppam!模块1.设置_图片_裁切_高宽信息"/>
				</group>
				<group id="Group6" label="All">
					<button id="Button1" label="Get_All" imageMso="G" size="large" onAction="Shape_Cut_size_position.ppam!模块1.获取_图片_裁切_高宽_位置信息"/>
					<button id="Button2" label="Set_All" imageMso="S" size="large" onAction="Shape_Cut_size_position.ppam!模块1.设置_图片_裁切_高宽_位置信息"/>
				</group>
			</tab>
		</tabs>
	</ribbon>
</customUI>
```

注：由于我不知到该如何解决与实现“在PPT中通过菜单按钮调用宏的方法”，所以使用搜索的一个替代方案，安装“RunMacro”程序，然后借助“RunMacro”程序实现“在PPT中通过菜单按钮调用宏”，上述XML文件就是用于配置按钮、以及按钮对应的宏；关于“RunMacro”的见我参考的此文“[https://www.cnblogs.com/ryueifu-VBA/p/11108695.html](https://link.zhihu.com/?target=https%3A//www.cnblogs.com/ryueifu-VBA/p/11108695.html)”，“RunMacro”下载连接“[https://files.cnblogs.com/files/ryueifu-VBA/RunMacro-Setup.zip](https://link.zhihu.com/?target=https%3A//files.cnblogs.com/files/ryueifu-VBA/RunMacro-Setup.zip)”。

3.然后就可在PPT中通过菜单按钮调用自己所编辑的上述的宏用于图片信息的获取与修改了~！

![image-20210821000812304](/images/获取PPT中选中图片的裁剪、高宽、位置，并在其他图片上重用/image-20210821000812304.png)

## 小结

最开始的问题至此得到了解决，如果看官你觉的有需求可以拿去使用，也可以在其中再增加些其他的步骤，不如直接一键 获取信息、从剪贴板粘贴图片、设置粘贴的图片，真正的一键解决，或者将这些宏绑定至快捷键，摆脱繁琐的鼠标操作，等等。

# 参考资料：

- [https://www.cnblogs.com/ryueifu-VBA/p/11108695.html](https://link.zhihu.com/?target=https%3A//www.cnblogs.com/ryueifu-VBA/p/11108695.html)
- [https://docs.microsoft.com/zh-cn/office/vba/api/powerpoint.pictureformat](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/office/vba/api/powerpoint.pictureformat)

- 