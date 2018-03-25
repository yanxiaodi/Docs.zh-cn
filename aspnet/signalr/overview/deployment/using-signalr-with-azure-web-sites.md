---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: "与在 Azure App Service Web Apps 使用 SignalR |Microsoft 文档"
author: pfletcher
description: "本文档介绍如何配置的 SignalR 应用程序在 Microsoft Azure 上运行。 在本教程中的软件版本使用，Visual Studio 2013 或 Vis...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>与在 Azure App Service Web Apps 使用 SignalR
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

> 本文档介绍如何配置的 SignalR 应用程序在 Microsoft Azure 上运行。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)或 Visual Studio 2012
> - .NET 4.5
> - SignalR 版本 2
> - Azure SDK 2.3 for Visual Studio 2013 或 2012
>   
> 
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)， [StackOverflow.com](http://stackoverflow.com/)，或[Microsoft Azure 论坛](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>目录

- [介绍](#introduction)
- [将 SignalR Web 应用部署到 Azure App Service](#deploying)
- [在 Azure App Service 上的启用 Websocket](#websocket)
- [使用 Azure Redis 缓存底板](#backplane)
- [后续步骤](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>介绍

ASP.NET SignalR 可用来将一个新的服务器和 web 或.NET 客户端之间的交互功能级别。 时在 Azure 中托管，SignalR 应用程序可以充分利用高度可用的可伸缩性，并运行在云中提供高性能环境。

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>将 SignalR Web 应用部署到 Azure App Service

SignalR 不会添加到应用程序部署到 Azure 与部署到本地服务器的任何特定的复杂性。 使用 SignalR 的应用程序可以承载于 Azure 而无需配置或其他设置中的任何更改 (尽管 Websocket 支持，请参阅[启用 Websocket 在 Azure App Service 上](#websocket)下面。)对于本教程中，你需要将中创建的应用程序部署[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)到 Azure。

**系统必备**

- Visual Studio 2013. 如果你没有 Visual Studio，Visual Studio 2013 Express for Web 是在安装中包括的 Azure sdk。
- [Visual Studio 2013 的 azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)或[for Visual Studio 2012 的 Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511)。
- 若要完成本教程，你将需要 Azure 订阅。 你可以[激活 MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)，或[注册试用订阅](https://azure.microsoft.com/pricing/free-trial/)。

### <a name="deploying-a-signalr-web-app-to-azure"></a>将 SignalR web 应用部署到 Azure

1. 完成[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)，或下载中完成的项目[代码库](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)。
2. 在 Visual Studio 中，选择**生成**，**发布 SignalR 聊天**。
3. 在"发布 Web"对话框中，选择"Windows Azure 网站"。

    ![选择 Azure 网站](using-signalr-with-azure-web-sites/_static/image1.png)
4. 如果您没有登录到你的 Microsoft 帐户，请单击**登录...**在"选择现有网站"对话框中，并登录。

    ![选择现有网站](using-signalr-with-azure-web-sites/_static/image2.png)    ![登录到 Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. 在"选择现有网站"对话框中，单击**新建**。

    ![新建网站](using-signalr-with-azure-web-sites/_static/image4.png)
6. 在"Windows Azure 上的创建网站"对话框中，输入唯一的应用程序名称。 在区域下拉列表中选择离您最近的区域。 单击 **“创建”**。

    ![在 Azure 上创建站点](using-signalr-with-azure-web-sites/_static/image5.png)
7. 在"发布 Web"对话框中，单击**发布**。

    ![发布站点](using-signalr-with-azure-web-sites/_static/image6.png)
8. 应用程序完成后发布，将在浏览器中打开托管在 Azure App Service Web Apps 中的 SignalR 聊天应用程序。

    ![在浏览器中打开站点](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>在 Azure App Service Web Apps 上的启用 Websocket

Websocket 需要显式启用 web 应用中使用 SignalR 应用程序; 中否则，将使用其他协议 (请参阅[传输和回退](../getting-started/introduction-to-signalr.md#transports)有关详细信息)。

为了在 Azure App Service Web Apps 上使用 Websocket，在在 web 应用的配置节中启用它。 若要执行此操作，打开你的 web 应用中[Azure 管理门户](https://manage.windowsazure.com/)，并选择配置。

![配置选项卡](using-signalr-with-azure-web-sites/_static/image8.png)

在配置页的顶部，确保.NET 4.5 用于你的 web 应用。

![.NET framework 版本 4.5 设置](using-signalr-with-azure-web-sites/_static/image9.png)

在配置页上，在**Websocket**设置中选择**上**。

![Websocket 设置： 上](using-signalr-with-azure-web-sites/_static/image10.png)

在配置页的底部，选择**保存**以保存所做的更改。

![保存设置](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>使用 Azure Redis 缓存底板

如果 web 应用，使用多个实例，并且这些实例的用户需要进行交互与另一个 （，以便，例如，创建一个实例中的聊天消息可以访问连接到其他实例的用户），则[Azure Redis 缓存底板](../performance/scaleout-with-redis.md)必须在你的应用程序中实现。

<a id="nextsteps"></a>
## <a name="next-steps"></a>后续步骤

有关在 Azure App Service Web Apps 的详细信息，请参阅[Web Apps 概述](https://azure.microsoft.com/documentation/articles/app-service-web-overview/)。
