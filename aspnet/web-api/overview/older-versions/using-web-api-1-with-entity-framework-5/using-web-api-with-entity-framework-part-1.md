---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: "第 1 部分： 概述和创建项目 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 47af34c72f1959756f5d68e0e80052e700c7b19c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="part-1-overview-and-creating-the-project"></a>第 1 部分： 概述和创建项目
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

实体框架是一种对象/关系映射框架。 它将在代码中的域对象映射到关系数据库中的实体。 大多数情况下，你无需担心数据库层中，因为实体框架将为你对其负责。 你的代码操作对象，并更改保存到数据库。

## <a name="about-the-tutorial"></a>有关本教程

在本教程中，你将创建一个简单应用商店应用程序。 有两个主要部分应用程序。 正常的用户可以查看产品和创建订单：

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

管理员可以创建、 删除或编辑产品：

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>你将学习的技能

下面是你将要掌握的内容：

- 如何使用 ASP.NET Web API 的实体框架。
- 如何使用 knockout.js 创建动态的客户端 UI。
- 如何与 Web API 一起使用 forms 身份验证，用户进行身份验证。

虽然本教程是自包含，但你可能想要首先阅读以下教程：

- [你的第一个 ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [创建一个 Web API 支持 CRUD 操作](../creating-a-web-api-that-supports-crud-operations.md)

一些了解[ASP.NET MVC](../../../../mvc/index.md)也是有帮助。

## <a name="overview"></a>概述

在高级别中，下面是应用程序的体系结构：

- ASP.NET MVC 为客户端生成的 HTML 页。
- ASP.NET Web API 公开的数据 （产品和 orders） 上的 CRUD 操作。
- 实体框架将转换到数据库实体使用 Web API 的 C# 模型。

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

下图显示了如何在应用程序的各层表示的域对象： 数据库层，对象模型中，依次连网格式，它用于传输到客户端通过 HTTP 的数据。

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>创建 Visual Studio 项目

你可以创建教程使用 Visual Web Developer Express 或 Visual Studio 的完整版本的项目。

从**启动**页上，单击**新项目**。

在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。 下**Visual C#**，选择**Web**。 在项目模板列表中，选择**ASP.NET MVC 4 Web 应用程序**。 将"ProductStore"的项目，然后单击**确定**。

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

在**新建 ASP.NET MVC 4 项目**对话框中，选择**Internet 应用程序**单击**确定**。

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

"Internet 应用程序"模板创建的 ASP.NET MVC 应用程序支持窗体身份验证。 如果你运行应用程序现在，它已有的某些功能：

- 新的用户可以注册通过单击右上角的"注册"链接。
- 已注册的用户可以通过单击"登录"链接进行登录。

将会自动创建的数据库中保留成员身份信息。 有关 ASP.NET mvc 的窗体身份验证的详细信息，请参阅[演练： 在 ASP.NET MVC 中使用 Forms 身份验证](https://msdn.microsoft.com/library/ff398049(VS.98).aspx)。

## <a name="update-the-css-file"></a>更新 CSS 文件

此步骤是外观上的但它将使呈现如前面的屏幕截图的页。

在解决方案资源管理器，展开内容文件夹并打开名为 Site.css 文件。 添加以下 CSS 样式：

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

>[!div class="step-by-step"]
[下一篇](using-web-api-with-entity-framework-part-2.md)
