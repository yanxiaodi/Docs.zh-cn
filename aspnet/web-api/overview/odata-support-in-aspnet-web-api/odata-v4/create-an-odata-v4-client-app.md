---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: "创建 OData v4 客户端应用程序 (C#) |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="create-an-odata-v4-client-app-c"></a>创建 OData v4 客户端应用程序 (C#)
====================
通过[Mike Wasson](https://github.com/MikeWasson)

在前面的教程，你将创建可支持的 CRUD 操作的基本 OData 服务。 现在让我们来创建服务的客户端。

启动 Visual Studio 的新实例并创建新的控制台应用程序项目。 在**新项目**对话框中，选择**已安装** &gt; **模板** &gt; **Visual C#** &gt; **Windows 桌面**，然后选择**控制台应用程序**模板。 将项目&quot;ProductsApp&quot;。

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> 你还可以将控制台应用程序添加到包含 OData 服务的同一 Visual Studio 解决方案。


## <a name="install-the-odata-client-code-generator"></a>安装的 OData 客户端代码生成器

从**工具**菜单上，选择**扩展和更新**。 选择**联机** &gt; **Visual Studio 库**。 在搜索框中，搜索&quot;OData 客户端代码生成器&quot;。 单击**下载**安装 VSIX。 系统可能会提示你重新启动 Visual Studio。

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>本地运行 OData 服务

从 Visual Studio 运行 ProductService 项目。 默认情况下，Visual Studio 将启动浏览器访问的应用程序根目录。 请注意 URI;您将需要它在下一步。 使应用程序保持运行。

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> 如果将这两个项目置于同一解决方案中，请确保运行 ProductService 项目而不调试。 在下一步的步骤中，你将需要使本服务运行时修改控制台应用程序项目。


## <a name="generate-the-service-proxy"></a>生成的服务代理

服务代理是一个.NET 类，定义用于访问 OData 服务的方法。 代理将转换为 HTTP 请求的方法调用。 将通过运行创建代理类[T4 模板](https://msdn.microsoft.com/library/bb126445.aspx)。

右键单击该项目。 选择**添加** &gt; **新项**。

![](create-an-odata-v4-client-app/_static/image5.png)

在**添加新项**对话框中，选择**Visual C# 项** &gt; **代码** &gt; **OData 客户端**。 该模板命名&quot;ProductClient.tt&quot;。 单击**添加**并依次单击安全警告。

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

此时，你将获得一个错误，则可以忽略。 Visual Studio 会自动运行该模板后，但需要某些配置设置的模板第一个。

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

打开文件 ProductClient.odata.config。在`Parameter`元素中，粘贴 ProductService 项目 （上一步） 从 URI 中。 例如:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

再次运行的模板。 在解决方案资源管理器，右键单击 ProductClient.tt 文件，然后选择**运行自定义工具**。

此模板创建一个名为 ProductClient.cs 定义代理的代码文件。 如果您更改 OData 终结点，你的应用，开发时，运行再次要更新代理的模板。

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>使用服务代理调用 OData 服务

打开文件 Program.cs 并将替换为以下的样板文件代码。

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

值替换*serviceUri*用于服务 URI 的更早版本。

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

运行应用程序时，它应输出以下各项：

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
