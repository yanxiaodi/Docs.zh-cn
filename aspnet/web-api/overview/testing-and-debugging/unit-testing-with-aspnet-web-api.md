---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: "单元测试 ASP.NET Web API 2 |Microsoft 文档"
author: tfitzmac
description: "此指南和应用程序演示如何为你的 Web API 2 应用程序创建简单的单元测试。 本教程演示如何包含单元测试项目..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-aspnet-web-api-2"></a>单元测试 ASP.NET Web API 2
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

[下载已完成的项目](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> 此指南和应用程序演示如何为你的 Web API 2 应用程序创建简单的单元测试。 本教程演示如何在解决方案中，包括单元测试项目和写入检查从控制器方法的返回的值的测试方法。
> 
> 本教程假定你熟悉的 ASP.NET Web API 的基本概念。 有关介绍性教程，请参阅[Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。
> 
> 本主题中的单元测试仅有意限于简单数据方案。 单元测试更高级的数据方案，请参阅[模拟实体框架时单元测试 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2


## <a name="in-this-topic"></a>在本主题中

本主题包含以下各节：

- [先决条件](#prereqs)
- [下载代码](#download)
- [使用单元测试项目创建应用程序](#appwithunittest)

    - [创建应用程序时添加单元测试项目](#whencreate)
    - [将单元测试项目添加到现有应用程序](#addtoexisting)
- [设置 Web API 2 应用程序](#setupproject)
- [在测试项目中安装 NuGet 包](#testpackages)
- [创建测试](#tests)
- [运行测试](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>系统必备

Visual Studio 2017 Community、 Professional 或 Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>下载代码

下载[已完成的项目](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)。 可下载的项目包含本主题和单元测试代码[模拟实体框架时单元测试 ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)主题。

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>使用单元测试项目创建应用程序

可以创建单元测试项目，创建你的应用程序时，也可以将单元测试项目添加到现有应用程序。 本教程演示了这两种方法用于创建单元测试项目。 若要遵循本教程，你可以使用这两种方法。

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>创建应用程序时添加单元测试项目

创建一个新的 ASP.NET Web 应用程序名为**StoreApp**。

![创建项目](unit-testing-with-aspnet-web-api/_static/image1.png)

在新建 ASP.NET 项目窗口中，选择**空**模板并添加文件夹和核心 Web api 的引用。 选择**添加单元测试**选项。 单元测试项目将自动命名**StoreApp.Tests**。 你可以保留此名称。

![创建单元测试项目](unit-testing-with-aspnet-web-api/_static/image2.png)

创建应用程序之后, 你将看到它包含两个项目。

![两个项目](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>将单元测试项目添加到现有应用程序

如果创建了应用程序时，你没有创建单元测试项目，你可以添加一个在任何时间。 例如，假设你已有应用程序名为 StoreApp，并且你想要添加单元测试。 若要添加单元测试项目，右键单击你的解决方案，然后选择**添加**和**新项目**。

![向解决方案添加新项目](unit-testing-with-aspnet-web-api/_static/image4.png)

选择**测试**左窗格中，然后选择**单元测试项目**对于项目类型。 将项目**StoreApp.Tests**。

![添加单元测试项目](unit-testing-with-aspnet-web-api/_static/image5.png)

你将看到你的解决方案中的单元测试项目。

在单元测试项目中，添加对原始项目的项目引用。

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>设置 Web API 2 应用程序

在 StoreApp 项目中，添加的类文件，以便**模型**文件夹名为**Product.cs**。 文件的内容替换为以下代码。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

生成解决方案。

右键单击 Controllers 文件夹，然后选择**添加**和**新建基架项**。 选择**Web API 2 控制器-空**。

![添加新控制器](unit-testing-with-aspnet-web-api/_static/image6.png)

将控制器名称设置为**SimpleProductController**，然后单击**添加**。

![指定控制器](unit-testing-with-aspnet-web-api/_static/image7.png)

用下面的代码替换现有代码。 若要简化此示例中，数据存储在一个列表，而不是数据库。 此类中定义的列表表示生产数据。 请注意控制器包括的产品对象的列表将作为参数的构造函数。 此构造函数，可将测试数据传递时单元测试。 控制器还包含两个**异步**方法来演示单元测试异步方法。 这些异步方法在实现通过调用**Task.FromResult**若要最大程度减少冗余代码，而通常的方法应包括资源密集型操作。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

GetProduct 方法返回的实例**IHttpActionResult**接口。 IHttpActionResult 是一个 Web API 2 中的新功能，它简化了单元测试开发。 实现 IHttpActionResult 接口的类位于[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)命名空间。 这些类表示通过操作请求可能作出响应并它们对应于 HTTP 状态代码。

生成解决方案。

你现已准备好设置测试项目。

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>在测试项目中安装 NuGet 包

当你使用空模板创建应用程序时，单元测试项目 (StoreApp.Tests) 不包括任何已安装的 NuGet 包。 其他模板，例如 Web API 模板中，将一些 NuGet 程序包包括在单元测试项目。 对于本教程中，你必须包含测试项目的 Microsoft ASP.NET Web API 2 核心程序包。

右键单击 StoreApp.Tests 项目并选择**管理 NuGet 包**。 必须选择要将包添加到该项目 StoreApp.Tests 项目。

![管理包](unit-testing-with-aspnet-web-api/_static/image8.png)

查找和安装 Microsoft ASP.NET Web API 2 核心包。

![安装 web api 核心程序包](unit-testing-with-aspnet-web-api/_static/image9.png)

关闭管理 NuGet 包窗口。

<a id="tests"></a>
## <a name="create-tests"></a>创建测试

默认情况下，你的测试项目包括一个名为 UnitTest1.cs 的空测试文件。 此文件显示了用于创建测试方法的特性。 为单元测试，可以使用此文件，或创建你自己的文件。

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

对于本教程中，你将创建您自己的测试类。 你可以删除 UnitTest1.cs 文件。 添加一个名为类**TestSimpleProductController.cs**，并将代码替换下面的代码。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>运行测试

现在你就可以运行测试。 所有使用标记的方法**TestMethod**将测试属性。 从**测试**菜单项，运行测试。

![运行测试](unit-testing-with-aspnet-web-api/_static/image11.png)

打开**测试资源管理器**窗口中，并注意测试的结果。

![测试结果](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>摘要

已完成本教程。 在本教程中的数据已特意简化能够专注于单元测试条件。 单元测试更高级的数据方案，请参阅[模拟实体框架时单元测试 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)。
