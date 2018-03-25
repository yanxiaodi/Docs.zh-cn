---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: "使用 Azure 服务总线的 SignalR 扩展 (SignalR 1.x) |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>使用 Azure 服务总线的 SignalR 扩展 (SignalR 1.x)
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)

在本教程中，你将部署到 Windows Azure Web 角色，使用服务总线底板来分配到每个角色实例的消息的 SignalR 应用程序。

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

先决条件：

- 一个 Windows Azure 帐户。
- [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)。
- Visual Studio 2012。

服务总线底板也是与兼容[Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)，版本 1.1。 但是，它不是与 Service Bus for Windows Server 的 1.0 版兼容的。

## <a name="pricing"></a>定价

服务总线底板使用主题发送消息。 有关最新定价的信息，请参阅[Service Bus](https://azure.microsoft.com/pricing/details/service-bus/)。 在撰写本文时，你可以向发送 1,000,000 消息每月小于 1 美元。 底板发送 SignalR hub 方法每次调用服务总线消息。 此外，还存在的连接、 断开连接、 联接或保留组等一些控制消息。 在大多数应用程序，大多数消息流量将中心方法调用。

## <a name="overview"></a>概述

我们获取详细的教程之前，以下是将要执行的操作的快速概览。

1. 使用 Windows Azure 门户创建新的 Service Bus 命名空间。
2. 将这些 NuGet 包添加到你的应用程序： 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. 创建 SignalR 应用程序。
4. 将以下代码添加到 Global.asax 配置底板： 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

对于每个应用程序，为"YourAppName"选取一个不同的值。 不要跨多个应用程序中使用相同的值。

## <a name="create-the-azure-services"></a>创建 Azure 服务

中所述创建云服务，[如何创建和部署云服务](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)。 按照部分中的步骤"如何： 创建云服务使用快速创建"。 对于本教程中，你不需要上载证书。

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

创建新的 Service Bus 命名空间中所述[如何使用 Service Bus 主题/订阅到](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)。 按照部分"创建服务 Namespace"中的步骤。

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> 请确保选择的相同区域的云服务以及服务总线命名空间。


## <a name="create-the-visual-studio-project"></a>创建 Visual Studio 项目

启动 Visual Studio。 从**文件**菜单上，单击**新项目**。

在**新项目**对话框框中，展开**Visual C#**。 下**已安装的模板**，选择**云**，然后选择**Windows Azure 云服务**。 保留默认.NET Framework 4.5。 将应用程序 ChatService，然后单击**确定**。

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

在**新建 Windows Azure 云服务**对话框中，选择 ASP.NET MVC 4 Web 角色。 单击右箭头按钮 (**&gt;**) 将该角色添加到你的解决方案。

鼠标悬停在该新角色，因此可见的铅笔图标。 单击此图标来重命名该角色。 将"SignalRChat"的角色，然后单击**确定**。

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

在**新建 ASP.NET MVC 4 项目**向导中，选择**Internet 应用程序**。 单击 **“确定”**。 项目向导将创建两个项目：

- ChatService： 此项目是 Windows Azure 应用程序。 它定义 Azure 角色和其他配置选项。
- SignalRChat： 此项目是你的 ASP.NET MVC 4 项目。

## <a name="create-the-signalr-chat-application"></a>创建 SignalR 聊天应用程序

若要创建聊天应用程序，按照本教程中的步骤[开始使用 SignalR 和 MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)。

使用 NuGet 安装所需的库。 从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在**程序包管理器控制台**窗口中，输入以下命令：

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

使用`-ProjectName`选项以将包安装到 ASP.NET MVC 项目中，而不是 Windows Azure 项目。

## <a name="configure-the-backplane"></a>配置底板

在应用程序的 Global.asax 文件中，添加以下代码：

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

现在，你需要获取服务总线连接字符串。 在 Azure 门户中，选择你创建的服务总线命名空间，然后单击访问密钥图标。

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

将连接字符串复制到剪贴板，然后将其粘贴到*connectionString*变量。

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>将部署到 Azure

在解决方案资源管理器，展开**角色**ChatService 项目中的文件夹。

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

右键单击 SignalRChat 角色并选择**属性**。 选择**配置**选项卡。下**实例**选择 2。 此外可以将 VM 大小设置为**特小**。

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

保存更改。

在解决方案资源管理器，右键单击 ChatService 项目。 选择“发布”。

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

如果这是你第一次发布到 Windows Azure，你必须下载您的凭据。 在**发布**向导中，单击"登录以下载凭据"。 这将提示你登录到 Windows Azure 门户并下载发布设置文件。

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

单击**导入**和选择你下载的发布设置文件。

单击 **“下一步”**。 在**发布设置**对话框下**云服务**，选择前面创建的云服务。

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

单击“发布” 。 它可能需要几分钟来部署应用程序和启动 Vm。

现在运行聊天应用程序时，角色实例将通过 Azure Service Bus，使用服务总线主题通信。 主题是消息队列，它允许多个订阅服务器。

底板会自动创建主题和订阅。 若要查看的订阅和消息活动，打开 Azure 门户，选择 Service Bus 命名空间，然后单击"主题"。

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

它可能需要几分钟时间才会显示在仪表板中的消息活动。

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR 管理主题生存期。 只要应用程序部署，请勿尝试手动删除主题或主题上更改设置。
