---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: "创建控制器 (C#) |Microsoft 文档"
author: StephenWalther
description: "在本教程中，Stephen Walther 演示如何将控制器添加到 ASP.NET MVC 应用程序。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 9faaff1e00998ef9a77c4928a9eb36fc93ab97f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-controller-c"></a>创建控制器 (C#)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> 在本教程中，Stephen Walther 演示如何将控制器添加到 ASP.NET MVC 应用程序。


本教程旨在说明如何创建新的 ASP.NET MVC 控制器。 了解如何创建控制器通过使用 Visual Studio 添加控制器菜单选项，以及通过手动创建的类文件。

### <a name="using-the-add-controller-menu-option"></a>使用添加控制器菜单选项

创建新的控制器的最简单方法是右键单击 Visual Studio 解决方案资源管理器窗口中的 Controllers 文件夹并选择**添加、 控制器**菜单选项 （请参见图 1）。 选择此菜单选项将打开**添加控制器**对话框 （请参见图 2）。


[![新项目对话框](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**图 01**： 添加新的控制器 ([单击以查看实际尺寸的图像](creating-a-controller-cs/_static/image2.png))


[![新项目对话框](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**图 02**: 添加控制器对话框 ([单击以查看实际尺寸的图像](creating-a-controller-cs/_static/image4.png))


请注意，以突出显示的控制器名称的第一部分**添加控制器**对话框。 每个控制器名称必须以后缀结尾*控制器*。 例如，你可以创建名为的控制器*ProductController*但不是名为的控制器*产品*。


如果你创建缺少的控制器*控制器*后缀然后你将无法调用控制器。 不执行该操作--我已浪费大量我生命周期后再犯此错误的时间。


**列表 1-Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

你应始终创建 Controllers 文件夹中的控制器。 否则为你将违反 ASP.NET MVC 的约定，而其他开发人员将有难度时间来了解你的应用程序。

### <a name="scaffolding-action-methods"></a>基架操作方法

当您创建一个控制器时，你可以选择自动生成创建、 更新和详细信息的操作方法 （请参见图 3）。 如果选择此选项，则会生成列出 2 中的控制器类。


[![自动创建操作方法](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**图 03**： 自动创建操作方法 ([单击以查看实际尺寸的图像](creating-a-controller-cs/_static/image6.png))


**列出 2-Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

这些生成的方法是存根 （stub） 方法。 你必须添加用于创建、 更新和自己客户显示详细信息的实际逻辑。 但是，存根 （stub） 方法向您提供很好的起点。

### <a name="creating-a-controller-class"></a>创建了控制器类

ASP.NET MVC 控制器是只是一个类。 如果你愿意，你可以忽略方便 Visual Studio 控制器基架和手动创建了控制器类。 请执行这些步骤：

1. 右键单击 Controllers 文件夹，然后选择菜单选项**添加、 新项**和选择**类**模板 （请参见图 4）。
2. 将新类 PersonController.cs，然后单击**添加**按钮。
3. 修改生成的类文件，以便类继承自基 System.Web.Mvc.Controller 类 （请参阅列出 3）。


[![创建一个新类](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**图 04**： 创建新类 ([单击以查看实际尺寸的图像](creating-a-controller-cs/_static/image8.png))


**列出 3-Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

列出 3 中的控制器公开一个名为 index （） 返回该字符串"Hello World ！"的操作。 你可以通过运行你的应用程序并请求如下所示的 URL 来调用此控制器操作：

`http://localhost:40071/Person`

> [!NOTE] 
> 
> ASP.NET 开发服务器使用一个随机端口号 (例如，40071)。 在输入的 URL 来调用控制器时，你将需要提供正确的端口号。 可以通过将鼠标悬停在图标，为 ASP.NET 开发服务器在 Windows 通知区域中 （屏幕的右下角） 来确定的端口号。

>[!div class="step-by-step"]
[上一页](adding-dynamic-content-to-a-cached-page-cs.md)
[下一页](creating-an-action-cs.md)
