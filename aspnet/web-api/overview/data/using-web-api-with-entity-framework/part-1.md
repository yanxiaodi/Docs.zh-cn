---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: "与实体框架 6 使用 Web API 2 |Microsoft 文档"
author: MikeWasson
description: "本教程将教您创建的 ASP.NET Web API 的 web 应用程序的基础知识后端。 本教程使用 Entity Framework 6 的数据布局..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: cceefa128f90b4c3e23dd31119f44e6ffc55f46f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-web-api-2-with-entity-framework-6"></a>与实体框架 6 使用 Web API 2
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://github.com/MikeWasson/BookService)

> 本教程将教您创建的 ASP.NET Web API 的 web 应用程序的基础知识后端。 本教程使用 Entity Framework 6 进行的数据层和 Knockout.js 进行客户端 JavaScript 应用程序。 本教程还演示如何将应用部署到 Azure App Service Web Apps。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Web API 2.1
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


本教程使用 Entity Framework 6 和 ASP.NET Web API 2 创建的 web 应用程序操作后端数据库。 下面是应用的将创建程序的屏幕截图。

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

该应用使用单页面应用程序 (SPA) 设计。 "单页面应用程序"是 web 应用程序加载单个 HTML 页，然后页动态更新，而不是加载新页面常用术语。 初始页加载后, 应用程序介绍与通过 AJAX 请求服务器。 AJAX 请求返回的 JSON 数据，应用程序用它来更新 UI。

AJAX 不是新的但目前有更加轻松地生成和维护一个大型的复杂的 SPA 应用程序的 JavaScript 框架。 本教程使用[Knockout.js](http://knockoutjs.com/)，但你可以使用任一 JavaScript 客户端框架。

下面是此应用程序主要构建基块：

- ASP.NET MVC 创建 HTML 页。
- ASP.NET Web API 处理 AJAX 请求，并返回 JSON 数据。
- Knockout.js 数据绑定的 HTML 元素 JSON 数据。
- 实体框架与数据库通信。

## <a name="see-this-app-running-on-azure"></a>请参阅在 Azure 上运行此应用程序

若要查看与实时 web 应用运行的已完成的站点吗？ 只需单击下面的按钮，可以将应用程序的完整版本部署到你的 Azure 帐户。

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

你需要将此解决方案部署到 Azure 的 Azure 帐户。 如果你还没有帐户，可以使用以下选项：

- [免费建立一个 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-获取信用额度来试用付费版 Azure 服务，你可以使用和甚至在用完后，最多可以保留帐户和使用免费的 Azure 服务。
- [激活 MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN 订阅为你的信用额度可以用于付费版 Azure 服务的每个月。

## <a name="create-the-project"></a>创建项目

打开 Visual Studio。 从**文件**菜单上，选择**新建**，然后选择**项目**。 (或单击**新项目**起始页上。)

在**新项目**对话框中，单击**Web**的左窗格中和**ASP.NET Web 应用程序**在中间窗格中。 将项目 BookService，然后单击**确定**。

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

在**新建 ASP.NET 项目**对话框中，选择**Web API**模板。

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

如果你想要托管在 Azure App Service 中的项目，请将**在云中托管**选中框。

单击**确定**以创建该项目。

## <a name="configure-azure-settings-optional"></a>配置 Azure 设置 （可选）

如果保留**云中的主机**选中选项，Visual Studio 将提示你登录到 Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

登录到 Azure 后，Visual Studio 会提示你配置 web 应用。 输入站点的名称，选择你的 Azure 订阅，然后选择地理区域。 下**数据库服务器**，选择**创建新的服务器**。 输入管理员用户名和密码。

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

>[!div class="step-by-step"]
[下一篇](part-2.md)
