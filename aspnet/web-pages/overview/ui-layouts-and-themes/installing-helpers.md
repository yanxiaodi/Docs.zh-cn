---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: "在 ASP.NET Web 安装程序的帮助页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "本文介绍如何在 ASP.NET Web 页 (Razor) 网站上安装程序的帮助。 一个帮助程序是一个可重用组件，包括代码和标记每个..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 842c5a56d14314217c1e6ad6d48ded28d3cc5b4e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>在一个 ASP.NET Web 页 (Razor) 站点中安装程序的帮助程序
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文介绍如何在 ASP.NET Web 页 (Razor) 网站上安装程序的帮助。 A*帮助器*包括代码和标记，以执行可能需要很长时间或复杂的任务是可重用组件。
> 
> 你将学习：
> 
> - 如何使用 WebMatrix 3 创建的网站中安装程序的帮助。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>帮助器概述

用户通常想要在网页上执行某些任务需要大量的代码，或者需要额外的知识。 示例包括显示的图表数据;在页面上; 将 Twitter"跟踪"按钮从您的网站; 发送电子邮件裁剪或调整图像; 的大小为您的网站使用 PayPal。 为了更加轻松地完成这些类型的操作，ASP.NET 网页，你可以使用*帮助器*。 帮助器组件，你为站点和安装，可让你通过使用刚刚一行或两个 Razor 代码执行典型的任务。

ASP.NET Web 页具有内置的几个帮助器。 但是，使用 NuGet 包管理器提供的包 （外接程序） 中提供了许多的帮助器。 NuGet 允许你选择要安装的程序包，然后它就会完成安装的所有详细信息。

## <a name="installing-a-helper-in-webmatrix-3"></a>在 WebMatrix 3 中安装程序的帮助程序

1. 在 WebMatrix 3 中，单击**NuGet**按钮。

    ![在 WebMatrix 的 NuGet 库对话框](installing-helpers/_static/image1.png)
2. 这将启动 NuGet 包管理器，并显示可用的包。 在搜索框中，输入你想要安装的帮助程序关键字。

    ![在 WebMatrix 的 NuGet 库对话框](installing-helpers/_static/image2.png)
- 选择包，然后单击**安装**。 单击**是**当系统询问你是否想要安装包，并指示你接受条款。

    如果这是已安装程序的帮助程序第一次，NuGet 将在你的网站代码的组成帮助器中创建文件夹。
- 若要卸载程序的帮助，请单击**库**菜单上，单击**已安装**选项卡，然后选择你想要卸载的程序包。

## <a name="installing-the-twitter-helper"></a>安装的 Twitter 帮助程序

Twitter API 的最新版本不兼容与通过 NuGet 安装 Twitter 帮助器。 相反，请参阅[Twitter 使用 WebMatrix 的帮助器](twitter-helper.md)有关如何设置项目中的 Twitter 帮助程序信息的主题。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源


[简介 ASP.NET Web Pages 2-编程基础知识](../getting-started/introducing-razor-syntax-c.md)

[使用 WebMatrix 的 twitter 帮助器](twitter-helper.md)
