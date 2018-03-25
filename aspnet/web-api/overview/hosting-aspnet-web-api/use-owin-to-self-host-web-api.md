---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: "使用 OWIN 自承载 ASP.NET Web API 2 |Microsoft 文档"
author: rick-anderson
description: "本教程演示如何在控制台应用程序，使用 OWIN 自承载 Web API 框架中托管 ASP.NET Web API。 打开.NET (OWIN) d 的 Web 界面..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>使用 OWIN 自承载 ASP.NET Web API 2
====================
通过[Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> 本教程演示如何在控制台应用程序，使用 OWIN 自承载 Web API 框架中托管 ASP.NET Web API。
> 
> [打开针对.NET 的 Web 界面](http://owin.org)(OWIN) 定义.NET web 服务器和 web 应用程序之间的抽象。 OWIN 将在服务器上，这使 OWIN 适合于你自己的过程，在 IIS 外部的 web 应用程序的自承载的 web 应用程序中脱离出来。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) （也适用于 Visual Studio 2012）
> - Web API 2


> [!NOTE]
> 你可以在本教程中找到的完整源代码[aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt)。


## <a name="create-a-console-application"></a>创建控制台应用程序

上**文件**菜单上，单击**新建**，然后单击**项目**。 从**已安装的模板**，在 Visual C# 中，单击**Windows** ，然后单击**控制台应用程序**。 将"OwinSelfhostSample"的项目，然后单击**确定**。

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>添加 Web API 和 OWIN 包

从**工具**菜单上，单击**库程序包管理器**，然后单击**程序包管理器控制台**。 在 Package Manager Console 窗口中，输入以下命令：

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

这将安装 WebAPI OWIN selfhost 包和所需的所有 OWIN 包。

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>配置 Web API 的自承载

在解决方案资源管理器，右键单击项目，然后选择**添加** / **类**添加新类。 将此类命名为 `Startup`。

![](use-owin-to-self-host-web-api/_static/image5.png)

将所有此文件中的样板文件代码替换为以下：

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>添加一个 Web API 控制器

接下来，添加 Web API 控制器类。 在解决方案资源管理器，右键单击项目，然后选择**添加** / **类**添加新类。 将此类命名为 `ValuesController`。

将所有此文件中的样板文件代码替换为以下：

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>启动 OWIN 主机并发出一个请求，使用 HttpClient

将所有的 Program.cs 文件中的样板文件代码替换为以下：

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>运行应用程序

若要运行应用程序，请在 Visual Studio 中按 F5。 输出应如下所示：

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>其他资源

[项目 Katana 事件的概述](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[承载在 Azure 辅助角色中的 ASP.NET Web API](host-aspnet-web-api-in-an-azure-worker-role.md)
