---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: "模拟实体框架时单元测试 ASP.NET Web API 2 |Microsoft 文档"
author: tfitzmac
description: "此指南和应用程序演示如何为 Web API 2 的应用程序使用实体框架创建单元测试。 它演示如何修改..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>模拟实体框架时单元测试 ASP.NET Web API 2
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

[下载已完成的项目](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> 此指南和应用程序演示如何为 Web API 2 的应用程序使用实体框架创建单元测试。 它显示如何修改基架的控制器，以便传递的上下文对象，用于测试，以及如何创建测试对象，用于处理与实体框架。
> 
> 有关使用 ASP.NET Web API 进行单元测试的简介，请参阅[使用 ASP.NET Web API 2 进行单元测试](unit-testing-with-aspnet-web-api.md)。
> 
> 本教程假定你熟悉的 ASP.NET Web API 的基本概念。 有关介绍性教程，请参阅[Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。
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
- [创建模型类](#modelclass)
- [添加控制器](#controller)
- [添加依赖关系注入](#dependency)
- [在测试项目中安装 NuGet 包](#testpackages)
- [创建测试上下文](#testcontext)
- [创建测试](#tests)
- [运行测试](#runtests)

如果你已经完成中的步骤[使用 ASP.NET Web API 2 进行单元测试](unit-testing-with-aspnet-web-api.md)，你可以跳到部分[添加控制器](#controller)。

<a id="prereqs"></a>
## <a name="prerequisites"></a>系统必备

Visual Studio 2017 Community、 Professional 或 Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>下载代码

下载[已完成的项目](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)。 可下载的项目包含本主题和单元测试代码[单元测试 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)主题。

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>使用单元测试项目创建应用程序

可以创建单元测试项目，创建你的应用程序时，也可以将单元测试项目添加到现有应用程序。 本教程演示创建应用程序时创建单元测试项目。

创建一个新的 ASP.NET Web 应用程序名为**StoreApp**。

在新建 ASP.NET 项目窗口中，选择**空**模板并添加文件夹和核心 Web api 的引用。 选择**添加单元测试**选项。 单元测试项目将自动命名**StoreApp.Tests**。 你可以保留此名称。

![创建单元测试项目](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

在创建应用程序，你将看到它包含两个项目- **StoreApp**和**StoreApp.Tests**。

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>创建模型类

在 StoreApp 项目中，添加的类文件，以便**模型**文件夹名为**Product.cs**。 文件的内容替换为以下代码。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

生成解决方案。

<a id="controller"></a>
## <a name="add-the-controller"></a>添加控制器

右键单击 Controllers 文件夹，然后选择**添加**和**新建基架项**。 选择其操作使用 Entity Framework 的 Web API 2 控制器。

![添加新控制器](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

设置下列值：

- 控制器名称： **ProductController**
- 模型类：**产品**
- 数据上下文类: [选择**新建数据上下文**按钮，这在如下所示的值填充]

![指定控制器](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

单击**添加**以自动生成的代码创建控制器。 代码包括用于创建、 检索、 更新和删除产品类的实例方法。 下面的代码演示了方法对于添加产品。 请注意该方法返回的实例**IHttpActionResult**。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult 是一个 Web API 2 中的新功能，它简化了单元测试开发。

在下一步的部分中，你将自定义生成的代码以简化将测试对象传递给控制器。

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>添加依赖关系注入

目前，ProductController 类是硬编码为使用 StoreAppContext 类的实例。 调用依赖关系注入的模式将用于修改你的应用程序并删除该硬编码的依赖关系。 通过打破这种依赖性，您可以传入的模拟对象测试时。

右键单击**模型**文件夹，并添加名为的新接口**IStoreAppContext**。

将代码替换为以下代码。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

打开 StoreAppContext.cs 文件并进行以下突出显示的更改。 要注意的重要的更改是：

- StoreAppContext 类可实现 IStoreAppContext 接口
- 实现 MarkAsModified 方法


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

打开 ProductController.cs 文件。 更改现有代码以匹配突出显示的代码。 这些更改在 StoreAppContext 中断依赖项，并启用其他类来在不同对象中传递上下文类。 此更改将使你在单元测试期间在测试上下文中传递。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

没有一次的多个更改，则必须在 ProductController 进行。 在**PutProduct**方法中，替换的实体状态设置为行修改通过 MarkAsModified 方法调用。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

生成解决方案。

你现已准备好设置测试项目。

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>在测试项目中安装 NuGet 包

当你使用空模板创建应用程序时，单元测试项目 (StoreApp.Tests) 不包括任何已安装的 NuGet 包。 其他模板，例如 Web API 模板中，将一些 NuGet 程序包包括在单元测试项目。 对于本教程，你必须包括实体框架包时和测试项目的 Microsoft ASP.NET Web API 2 核心程序包。

右键单击 StoreApp.Tests 项目并选择**管理 NuGet 包**。 必须选择要将包添加到该项目 StoreApp.Tests 项目。

![管理包](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

从联机程序包中，查找和安装 EntityFramework 包 （版本 6.0 或更高版本）。 如果显示 EntityFramework 包已安装，您可能选择 StoreApp 项目而不是 StoreApp.Tests 项目。

![添加实体框架](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

查找和安装 Microsoft ASP.NET Web API 2 核心包。

![安装 web api 核心程序包](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

关闭管理 NuGet 包窗口。

<a id="testcontext"></a>
## <a name="create-test-context"></a>创建测试上下文

添加一个名为类**TestDbSet**向测试项目。 此类用作测试数据集的基类。 将代码替换为以下代码。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

添加一个名为类**TestProductDbSet**向该测试项目，其中包含下面的代码。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

添加一个名为类**TestStoreAppContext**和替换为以下代码替换现有代码。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>创建测试

默认情况下，你的测试项目包括名为一个空的测试文件**UnitTest1.cs**。 此文件显示了用于创建测试方法的特性。 对于本教程中，您可以删除此文件，因为你将添加一个新测试类。

添加一个名为类**TestProductController**向测试项目。 将代码替换为以下代码。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>运行测试

现在你就可以运行测试。 所有使用标记的方法**TestMethod**将测试属性。 从**测试**菜单项，运行测试。

![运行测试](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

打开**测试资源管理器**窗口中，并注意测试的结果。

![测试结果](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
