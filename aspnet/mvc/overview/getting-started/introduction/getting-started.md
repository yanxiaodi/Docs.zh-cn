---
uid: mvc/overview/getting-started/introduction/getting-started
title: "ASP.NET MVC 5 入门 |Microsoft 文档"
author: Rick-Anderson
description: "注意： 本教程的更新的版本是可以在此处使用 Visual Studio 2015。 新的教程使用 ASP.NET 核心 MVC 6，它提供许多 improvem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 1616b238612fa9f519418f583c40a46b9d81d8ce
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a>ASP.NET MVC 5 入门
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 本教程将教您生成 ASP.NET MVC 5 web 应用程序使用的基础知识[Visual Studio 2017](https://www.visualstudio.com/)。 对于教程的最后一个源位于[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)
 
 
 本教程编写的[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )， [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )和[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )
 
 你需要将此应用程序部署到 Azure 的 Azure 帐户：
 
 - 你可以[免费建立一个 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-获取信用额度来试用付费版 Azure 服务，你可以使用和甚至在用完后，最多可以保留帐户和使用免费的 Azure 服务。
 - 你可以[激活 MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN 订阅为你的信用额度可以用于付费版 Azure 服务的每个月。


## <a name="getting-started"></a>入门

首先，安装并运行[Visual Studio 2017](https://www.visualstudio.com/)。

Visual Studio 是一个 IDE 或集成的开发环境。 就像使用 Microsoft Word 编写文档，你将使用 IDE 来创建应用程序。 Visual Studio 中没有向你显示各个可用的选项的底部的列表。 此外还有一个菜单，提供另一种方法在 IDE 中执行任务。 (例如，而不是选择**新项目**从**启动**页上，你可以使用菜单并选择**文件** &gt; **新项目**.)

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a>创建第一个应用程序

单击**新项目**，然后选择 Visual C# 在左侧，然后**Web** ，然后选择**ASP.NET Web 应用程序 (.NET Framework)**。 将你的项目"MvcMovie"，然后单击**确定**。

![](getting-started/_static/image2.png)

在**新建 ASP.NET 项目**对话框中，单击**MVC** ，然后单击**确定**。

![](getting-started/_static/image3.png)

因此，必须运行的应用程序现在不执行任何操作的情况下，visual Studio 刚创建的 ASP.NET MVC 项目使用默认模板 ！ 这是简单"Hello World ！" 项目和它的启动你的应用程序的好时机。

![](getting-started/_static/image4.png)

单击 F5 启动调试。 F5 导致 Visual Studio 启动[IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)并运行你的 web 应用。 然后，visual Studio 启动浏览器，并打开应用程序的主页。 请注意，浏览器的地址栏显示`localhost:port#`和不类似于`example.com`。 这是因为`localhost`始终指向你自己本地的计算机，在这种情况下运行你刚才生成应用程序。 当 Visual Studio 运行 web 项目时，随机端口适用于 web 服务器。 在下图中，端口号是 1234年。 运行应用程序时，你将看到不同的端口号。

![](getting-started/_static/image5.png)

现成此默认模板也提供家庭、 联系人和关于页。 图中不显示**主页**，**有关**和**联系人**链接。 根据你的浏览器窗口的大小，你可能需要单击导航图标以查看这些链接。

![](getting-started/_static/image6.png)  

应用程序还提供支持注册和登录。 下一步是更改此应用程序的工作原理，并得有点了解 ASP.NET MVC。 关闭 ASP.NET MVC 应用程序，让我们更改一些代码。

当前的教程的列表，请参阅[MVC 建议文章](../mvc-learning-sequence.md)。

## <a name="see-this-app-running-on-azure"></a>请参阅在 Azure 上运行此应用程序

若要查看与实时 web 应用运行的已完成的站点吗？ 只需单击下面的按钮，可以将应用程序的完整版本部署到你的 Azure 帐户。

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

你需要将此解决方案部署到 Azure 的 Azure 帐户。 如果你还没有帐户，可以使用以下选项：

- [免费建立一个 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-获取信用额度来试用付费版 Azure 服务，你可以使用和甚至在用完后，最多可以保留帐户和使用免费的 Azure 服务。
- [激活 MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN 订阅为你的信用额度可以用于付费版 Azure 服务的每个月。

>[!div class="step-by-step"]
[下一篇](adding-a-controller.md)
