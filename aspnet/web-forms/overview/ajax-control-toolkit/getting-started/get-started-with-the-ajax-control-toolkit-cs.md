---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: "要开始使用 AJAX 控件工具包 (C#) |Microsoft 文档"
author: microsoft
description: "了解你需要知道若要开始使用 AJAX 控件工具包。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d3f4dd26a9f82dce78b1c3665f9da6b54bdacba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>要开始使用 AJAX 控件工具包 (C#)
====================
通过[Microsoft](https://github.com/microsoft)

> 了解你需要知道若要开始使用 AJAX 控件工具包。


AJAX 控件工具包包含 30 多个可用的控件，可以使用 ASP.NET 应用程序中。 在本教程中，您将学习如何下载 AJAX 控件工具包和将工具包控件添加到你 Visual Studio/Visual Web Developer Express 工具箱。

## <a name="downloading-the-ajax-control-toolkit"></a>下载 AJAX 控件工具包

[AJAX 控件工具包](http://devexpress.com/act)开放源代码项目开发的 ASP.NET 社区和 ASP.NET 团队的成员。 


[![下载 AJAX 控件工具包](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**图 01**： 下载 AJAX 控件工具包 ([单击以查看实际尺寸的图像](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


下载文件后，你需要取消阻止该文件。 右键单击该文件，选择属性，然后单击**解除阻止**按钮 （请参见图 2）。


[![取消阻止 AJAX 控件工具包 ZIP 文件](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**图 02**： 取消阻止 AJAX 控件工具包 ZIP 文件 ([单击以查看实际尺寸的图像](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


取消阻止该文件后，你可以将文件解压缩： 右键单击该文件并选择**提取所有**菜单选项。 现在，我们已准备好将该工具包添加到 Visual Studio/Visual Web Developer 工具箱。

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>添加到工具箱的 AJAX 控件工具包

使用 AJAX 控件工具包的最简单方法是将该工具包添加到 Visual Studio/Visual Web Developer 工具箱 （请参见图 3）。 这样一来，你可以只需将工具包控件拖到页在你想要使用它。


[![AJAX 控件工具包将显示在工具箱](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**图 03**: AJAX 控件工具包将显示在工具箱 ([单击以查看实际尺寸的图像](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


首先，你需要向工具箱添加一个 AJAX 控件工具包选项卡。 请按照下列步骤。

1. 通过选择菜单选项文件中，新的网站中创建新的 ASP.NET 网站。 双击解决方案资源管理器窗口中的 Default.aspx，以在编辑器中打开该文件。
2. 右键单击常规选项卡下的工具箱，然后选择菜单选项**添加选项卡**（请参见图 4）。
3. 输入一个称为 AJAX 控件工具包的新选项卡。


[![添加新选项卡](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**图 04**： 添加新选项卡 ([单击以查看实际尺寸的图像](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


接下来，你需要将 AJAX 控件工具包控件添加到新选项卡。请执行这些步骤：

- AJAX 控件工具包选项卡下右键单击并选择菜单选项**选择项 （请参见图 5）**。
- 浏览到您解压缩 AJAX 控件工具包并选择 AjaxControlToolkit.dll 程序集的位置。


[![选择要添加到工具箱项](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**图 05**： 选择要添加到工具箱项 ([单击以查看实际尺寸的图像](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


完成这些步骤后，所有工具包控件都将出现在工具箱中。

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>升级到新版本的工具包

如果你使用较旧版本的该工具包的并现在需要将移到更高版本是建议的步骤：

- 二进制文件-从你的网站 Bin 文件夹中删除 AjaxControlToolkit.dll 程序集的旧版本。
- 工具箱项-删除 AJAX 控件工具包选项卡，然后按照上述步骤以重新创建具有 AjaxControlToolkit.dll 程序集的新版本的选项卡。

>[!div class="step-by-step"]
[下一篇](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
