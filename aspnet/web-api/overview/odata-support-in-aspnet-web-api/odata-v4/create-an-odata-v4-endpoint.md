---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: "创建 OData v4 终结点使用 ASP.NET Web API 2.2 |Microsoft 文档"
author: MikeWasson
description: "开放式数据协议 (OData) 是 web 数据访问协议。 OData 提供统一的方式来查询和操作通过 CRUD 操作的数据集..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>创建 OData v4 终结点使用 ASP.NET Web API 2.2
====================
通过[Mike Wasson](https://github.com/MikeWasson)

> 开放式数据协议 (OData) 是 web 数据访问协议。 OData 提供统一的方式来查询和操作数据集通过 CRUD 操作 （创建、 读取、 更新和删除）。
> 
> ASP.NET Web API 支持 v3 和 v4 协议。 你甚至可以有运行的并行 v4 终结点与 v3 终结点。
> 
> 本教程演示如何创建支持的 CRUD 操作的 OData v4 终结点。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Web API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>教程版本
> 
> 有关 OData 版本 3，请参阅[创建 OData v3 终结点](../odata-v3/creating-an-odata-endpoint.md)。


## <a name="create-the-visual-studio-project"></a>创建 Visual Studio 项目

在 Visual Studio 中，从**文件**菜单上，选择**新建** &gt; **项目**。

展开**已安装** &gt; **模板** &gt; **Visual C#** &gt; **Web**，然后选择**ASP.NET Web 应用程序**模板。 将项目&quot;ProductService&quot;。

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

在**新项目**对话框中，选择**空**模板。 下&quot;将文件夹添加和核心引用...&quot;，单击**Web API**。 单击“确定”。

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>安装 OData 包

从**工具**菜单上，选择**NuGet 包管理器** &gt; **程序包管理器控制台**。 在 Package Manager Console 窗口中，键入：

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

此命令将安装最新的 OData NuGet 包。

## <a name="add-a-model-class"></a>将一个模型类添加

A*模型*是一个对象，表示你的应用程序中的数据实体。

在解决方案资源管理器，右键单击模型文件夹。 从上下文菜单中，选择**添加** &gt; **类**。

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> 按照约定，模型类都放在 Models 文件夹中，但你不需要遵循此约定中您自己的项目。


将此类命名为 `Product`。 在 Product.cs 文件中，将替换为以下内容的样板文件代码：

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id`属性是实体键。 客户端可以查询按的键的实体。 例如，若要获取的产品与 ID 为 5，URI 是`/Products(5)`。 `Id`属性也将后端数据库中的主键。

## <a name="enable-entity-framework"></a>启用实体框架

对于本教程，我们将使用 Entity Framework (EF) Code First 创建后端数据库。

> [!NOTE]
> Web API OData 不需要 EF。 使用任何数据访问层上可以转换模型数据库实体。


首先，为 EF 安装 NuGet 包。 从**工具**菜单上，选择**NuGet 包管理器** &gt; **程序包管理器控制台**。 在 Package Manager Console 窗口中，键入：

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

打开 Web.config 文件中，并添加以下节内的**配置**元素后面**configSections**元素。

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

此设置将添加 LocalDB 数据库的连接字符串。 本地运行应用时，将使用此数据库。

接下来，添加一个名为类`ProductsContext`到 Models 文件夹：

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

在构造函数中，`"name=ProductsContext"`提供的连接字符串名称。

## <a name="configure-the-odata-endpoint"></a>配置 OData 终结点

打开文件应用\_Start/WebApiConfig.cs。 添加以下**使用**语句：

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

然后添加以下代码到**注册**方法：

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

此代码执行两项操作：

- 创建实体数据模型 (EDM)。
- 添加一个路由。

EDM 是一个抽象模型的数据。 EDM 用于创建服务元数据文档。 **ODataConventionModelBuilder**类创建 EDM 通过使用默认的命名约定。 此方法要求最少的代码。 如果你想 EDM 的更好地控制，则可以使用**ODataModelBuilder**类以创建 EDM 显式添加属性、 密钥和导航属性。

A*路由*告知如何将 HTTP 请求路由到终结点的 Web API。 若要创建 OData v4 路由的步骤，请调用**MapODataServiceRoute**扩展方法。

如果你的应用程序具有多个 OData 终结点，每个创建单独的路由。 为每个路由，指定唯一的路由名称和前缀。

## <a name="add-the-odata-controller"></a>添加 OData 控制器

A*控制器*是处理 HTTP 请求的类。 你创建的每个实体在你的 OData 服务中设置单独的控制器。 在本教程中，将为创建一个控制器，`Product`实体。

在解决方案资源管理器，右键单击 Controllers 文件夹，然后选择**添加** &gt; **类**。 将此类命名为 `ProductsController`。

> [!NOTE]
> 本教程针对 OData v3 使用的版本**添加控制器**基架。 目前，OData v4 没有基架。


将替换为以下 ProductsController.cs 中的样板文件代码。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

控制器使用`ProductsContext`类来访问使用 EF 的数据库。 请注意，控制器将重写**释放**方法来释放**ProductsContext**。

这是控制器的起始点。 接下来，我们将添加所有的 CRUD 操作的方法。

## <a name="querying-the-entity-set"></a>查询实体集

添加以下方法`ProductsController`。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

无参数版本`Get`方法将返回整个产品集合。 `Get`方法替换*密钥*参数按照键查找产品 (在这种情况下，`Id`属性)。

**[EnableQuery]**属性使客户端能够修改查询，通过使用如 $filter、 $sort 和 $page 的查询选项。 有关详细信息，请参阅[支持 OData 查询选项](../supporting-odata-query-options.md)。

## <a name="adding-an-entity-to-the-entity-set"></a>将实体添加到实体集

若要启用客户端向数据库添加新的产品，添加以下方法`ProductsController`。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>更新实体

OData 支持用于更新实体，修补程序和 PUT 的两个不同的语义。

- 修补程序执行部分更新。 客户端指定只是要更新的属性。
- PUT 会替换整个实体。

PUT 的缺点是客户端必须发送实体，包括未更改的值中的所有属性的值。 [OData 规范](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719)规定修补程序是首选。

在任何情况下，以下是针对修补程序和 PUT 方法的代码：

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

对于 PATCH 的控制器使用**增量&lt;T&gt;** 类型以跟踪所做的更改。

## <a name="deleting-an-entity"></a>删除实体

若要启用客户端以从数据库中删除产品，添加以下方法`ProductsController`。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
