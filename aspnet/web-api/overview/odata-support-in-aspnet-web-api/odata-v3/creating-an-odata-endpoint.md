---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: "创建与 Web API 2 OData v3 终结点 |Microsoft 文档"
author: MikeWasson
description: "开放式数据协议 (OData) 是 web 数据访问协议。 OData 提供统一的方式来构造数据、 查询的数据，以及处理数据..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 33fe4d764bf9bf64c852f1269255925b5cc42536
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>创建与 Web API 2 OData v3 终结点
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [开放数据协议](http://www.odata.org/)(OData) 是用于 web 的数据访问协议。 OData 提供统一的方式来结构数据、 查询的数据和操作数据集通过 CRUD 操作 （创建、 读取、 更新和删除）。 OData 支持 AtomPub (XML) 和 JSON 格式。 OData 还定义了如何公开数据的元数据。 客户端可以使用元数据发现的类型信息和数据集的关系。
> 
> ASP.NET Web API 可以轻松创建数据集的 OData 终结点。 你可以控制完全的 OData 操作终结点支持。 你可以托管多个 OData 终结点，以及非 OData 终结点。 必须对数据模型、 后端业务逻辑和数据层的完全控制。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - OData 版本 3
> - Entity Framework 6
> - [Fiddler Web 调试代理 （可选）](http://www.fiddler2.com)
> 
> 在添加了 web API OData 支持[ASP.NET 和 Web Tools 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。 但是，本教程使用已添加到 Visual Studio 2013 中的基架。


在本教程中，你将创建一个简单的 OData 终结点的客户端可以查询。 你还将创建的 C# 客户端终结点。 完成本教程后，接下来的教程演示如何添加更多的功能，包括实体关系，操作，然后展开的 $/ $选择。

- [创建 Visual Studio 项目](#create-project)
- [添加实体模型](#add-model)
- [添加一个 OData 控制器](#add-controller)
- [添加的 EDM 和路由](#edm)
- [种子数据库 （可选）](#seed-db)
- [浏览 OData 终结点](#explore)
- [OData 序列化格式](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>创建 Visual Studio 项目

在本教程中，你将创建一个支持基本的 CRUD 操作的 OData 终结点。 终结点将公开单个资源，产品的列表。 更高版本的教程将添加更多的功能。

启动 Visual Studio 并选择**新项目**从开始页。 或从**文件**菜单上，选择**新建**然后**项目**。

在**模板**窗格中，选择**已安装的模板**和展开 Visual C# 节点。 下**Visual C#**，选择**Web**。 选择**ASP.NET Web 应用程序**模板。

![](creating-an-odata-endpoint/_static/image1.png)

在**新建 ASP.NET 项目**对话框中，选择**空**模板。 下&quot;将文件夹添加和核心引用...&quot;，检查**Web API**。 单击 **“确定”**。

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>添加实体模型

模型是表示应用程序中的数据的对象。 对于本教程，我们需要表示产品的模型。 模型对应于我们 OData 实体类型。

在解决方案资源管理器，右键单击模型文件夹。 从上下文菜单中，选择**添加**然后选择**类**。

![](creating-an-odata-endpoint/_static/image3.png)

在**添加新**项对话框，将类&quot;产品&quot;。

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> 按照约定，模型类放置在模型文件夹中。 无需遵循此约定在你自己的项目中，但我们将在本教程中使用它。


在 Product.cs 文件中，添加以下类定义：

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

ID 属性将为实体键。 客户端可以查询 id 的产品 此字段还将后端数据库中的主键。

现在生成项目。 在下一步的步骤中，我们将使用某些 Visual Studio 基架使用反射来找到的产品类型。

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>添加一个 OData 控制器

A*控制器*是处理 HTTP 请求的类。 定义每个实体集在你的 OData 服务的单独控制器。 在本教程中，我们将创建一个控制器。

在解决方案资源管理器，右键单击 Controllers 文件夹。 选择**添加**，然后选择**控制器**。

![](creating-an-odata-endpoint/_static/image5.png)

在**添加基架**对话框中，选择&quot;Web API 2 OData 控制器其操作使用 Entity Framework&quot;。

![](creating-an-odata-endpoint/_static/image6.png)

在**添加控制器**对话框中，控制器"ProductsController"。 选择&quot;使用异步控制器操作&quot;复选框。 在**模型**下拉列表中，选择产品类别。

![](creating-an-odata-endpoint/_static/image7.png)

单击**新建数据上下文...**按钮。 保留的数据上下文类型的默认名称，然后单击**添加**。

![](creating-an-odata-endpoint/_static/image8.png)

在添加控制器对话框添加控制器中，单击添加。

![](creating-an-odata-endpoint/_static/image9.png)

注意： 是否你收到一条错误消息，指出&quot;时出错，获取类型...&quot;，请确保添加产品类后生成 Visual Studio 项目。 基架使用反射来查找类。

![](creating-an-odata-endpoint/_static/image10.png)

基架添加到项目的两个代码文件：

- Products.cs 定义实现 OData 终结点的 Web API 控制器。
- ProductServiceContext.cs 提供方法来查询使用实体框架的基础数据库。

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>添加的 EDM 和路由

在解决方案资源管理器，展开应用程序\_启动文件夹并打开名为 WebApiConfig.cs 文件。 此类包含 Web API 的配置代码。 将替换为以下这段代码：

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

此代码执行两项操作：

- 创建实体数据模型 (EDM) 的 OData 终结点。
- 添加终结点的路由。

EDM 是一个抽象模型的数据。 EDM 用于创建元数据文档并定义服务的 Uri。 **ODataConventionModelBuilder**使用一组默认的命名约定 EDM 创建 EDM 的实体。 此方法要求最少的代码。 如果你想 EDM 的更好地控制，则可以使用**ODataModelBuilder**类以创建 EDM 显式添加属性、 密钥和导航属性。

**EntitySet**方法将添加的实体集 EDM 到：

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

字符串"Products"定义的实体集的名称。 控制器的名称必须匹配的实体集的名称。 在本教程中，实体集的名称为"Products"和控制器命名为`ProductsController`。 如果你名为的实体集"ProductSet"，你需要命名控制器`ProductSetController`。 请注意，终结点可以有多个实体集。 调用**EntitySet&lt;T&gt;** 有关每个实体，以及然后定义一个相应的控制器。

**MapODataRoute**方法将添加 OData 终结点的路由。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

第一个参数是路由的友好名称。 你的服务的客户端不会看到此名称。 第二个参数是终结点的 URI 前缀。 给定此代码，Products 实体集的 URI 是 http://*主机名*  /odata/产品。 你的应用程序可以具有多个 OData 终结点。 对于每个终结点，调用**MapODataRoute**提供唯一的路由名称和唯一的 URI 前缀。

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>种子数据库 （可选）

在此步骤中，你将使用实体框架植入到数据库的某些测试数据。 此步骤是可选的但它可让你立即测试 OData 终结点。

从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在 Package Manager Console 窗口中，输入以下命令：

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

这将添加名为迁移和名为 Configuration.cs 代码文件的文件夹。

![](creating-an-odata-endpoint/_static/image12.png)

打开此文件并添加以下代码`Configuration.Seed`方法。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

在程序包管理器控制台窗口中，输入以下命令：

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

这些命令生成创建数据库，并将执行该代码的代码。

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>浏览 OData 终结点

在本部分中，我们将使用[Fiddler Web 调试代理](http://www.fiddler2.com)将请求发送到终结点并检查响应消息。 这将帮助你了解 OData 终结点的功能。

在 Visual Studio 中，按 F5 启动调试。 默认情况下，Visual Studio 将打开浏览器指向`http://localhost:*port*`，其中*端口*是在项目设置中配置的端口号。

你可以更改项目设置中的端口号。 在解决方案资源管理器，右键单击该项目并选择**属性**。 在属性窗口中，选择**Web**。 输入下的端口号**项目 Url**。

### <a name="service-document"></a>服务文档

*服务文档*包含 OData 终结点的实体集的列表。 若要获取服务文档，请对根 URI 的服务发送 GET 请求。

使用 Fiddler，输入以下 URI 中的**Composer**选项卡： `http://localhost:port/odata/`，其中*端口*是端口号。

![](creating-an-odata-endpoint/_static/image13.png)

单击**执行**按钮。 Fiddler 将 HTTP GET 请求发送到你的应用程序。 你应看到 Web 会话列表中的响应。 如果一切正常，则状态代码将为 200。

![](creating-an-odata-endpoint/_static/image14.png)

双击 Web 会话列表以查看检查器选项卡中的响应消息的详细信息中的响应。

![](creating-an-odata-endpoint/_static/image15.png)

原始 HTTP 响应消息应类似于以下形式：

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

默认情况下，Web API AtomPub 格式返回服务文档。 若要请求 JSON，请对 HTTP 请求中添加以下标头：

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

现在 HTTP 响应包含 JSON 负载：

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>服务元数据文档

*服务元数据文档*描述服务中，使用一种称为概念架构定义语言 (CSDL) 的 XML 语言的数据模型。 元数据文档在服务中，显示的数据的结构，可以用于生成客户端代码。

若要获取的元数据文档，发送 GET 请求到`http://localhost:port/odata/$metadata`。 下面是本教程中所示的终结点的元数据。

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>实体集

若要获取的产品实体集，发送 GET 请求到`http://localhost:port/odata/Products`。

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>实体

若要获取各个产品，发送 GET 请求到`http://localhost:port/odata/Products(1)`，其中"1"是产品 id。

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>OData 序列化格式

OData 支持几种序列化格式：

- Atom 发布 (XML)
- JSON"light"（在 OData v3 中引入）
- JSON"详细"(OData v2)

默认情况下，Web API 使用 AtomPubJSON"light"格式。 

若要获取 AtomPub 格式，请设置为"application/atom + xml"Accept 标头。 下面是一个示例响应正文：

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

你可以看到的 Atom 格式的一个明显缺点： 它是大量比详细 JSON light。 但是，如果必须理解 AtomPub 的客户端，则客户端可能通过 JSON 希望该格式。

下面是实体的 JSON 轻量版本的相同:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

OData 协议版本 3 中引入了 JSON 浅色格式。 为了向后兼容性，客户端可以请求的较旧的"详细"JSON 格式。 若要请求详细 JSON，请将 Accept 标头设置为`application/json;odata=verbose`。 以下是详细的版本：

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

此格式可传达在响应正文中，可以将其添加增加大量开销 over 整个会话的多个元数据。 此外，它将添加一定程度的间接性包装在名为"d"的属性的对象。

## <a name="next-steps"></a>后续步骤

- [添加实体关系](working-with-entity-relations.md)
- [添加 OData 操作](odata-actions.md)
- [从.NET 客户端调用 OData 服务](calling-an-odata-service-from-a-net-client.md)
