---
uid: web-api/overview/older-versions/self-host-a-web-api
title: "自承载 ASP.NET Web API 1 (C#) |Microsoft 文档"
author: MikeWasson
description: "ASP.NET Web API 不需要 IIS。 在主机过程中，你可以自承载 web API。 本教程演示如何承载 web API applic 控制台中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a>自承载 ASP.NET Web API 1 (C#)
====================
通过[Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web API 不需要 IIS。 在主机过程中，你可以自承载 web API。 本教程演示如何承载于控制台应用程序内的 web API。
> 
> **新的应用程序应使用 OWIN 自承载 Web API。** 请参阅[使用 OWIN 自承载 ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Web API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>创建控制台应用程序项目

启动 Visual Studio 并选择**新项目**从**启动**页。 或从**文件**菜单上，选择**新建**然后**项目**。

在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。 下**Visual C#**，选择**Windows**。 在项目模板列表中，选择**控制台应用程序**。 将项目&quot;SelfHost&quot;单击**确定**。

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>设置目标框架 (Visual Studio 2010)

如果你使用的 Visual Studio 2010，更改目标框架为.NET Framework 4.0。 (默认情况下，项目模板面向[.Net Framework 客户端配置文件](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)。)

在解决方案资源管理器，右键单击该项目并选择**属性**。 在**目标框架**下拉列表中，将目标框架更改为.NET Framework 4.0。 当系统提示以应用更改，单击**是**。

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>安装 NuGet 包管理器

NuGet 包管理器是将 Web API 程序集添加到非 ASP.NET 项目的最简单方法。

若要检查是否安装了 NuGet 包管理器，请单击**工具**Visual Studio 中的菜单。 如果你看到一个菜单项调用**库程序包管理器**，您就可以得到 NuGet 包管理器。

若要安装 NuGet 包管理器：

1. 启动 Visual Studio。
2. 从**工具**菜单上，选择**扩展和更新**。
3. 在**扩展和更新**对话框中，选择**联机**。
4. 如果看不到"NuGet Package Manager"，请在搜索框中键入"nuget package manager"。
5. 选择 NuGet 包管理器，然后单击**下载**。
6. 下载完成后，系统将提示安装。
7. 安装完成后，你可能会提示您重新启动 Visual Studio。

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>添加 Web API NuGet 包

安装 NuGet 包管理器后，将添加到你的项目的 Web API 自承载包。

1. 从**工具**菜单上，选择**库程序包管理器**。 *请注意*： 如果你不会看到此菜单项，请确保正确安装该 NuGet 包管理器。
2. 选择**管理解决方案的 NuGet 包...**
3. 在**Manage NugGet Packages**对话框中，选择**联机**。
4. 在搜索框中，键入&quot;Microsoft.AspNet.WebApi.SelfHost&quot;。
5. 选择 ASP.NET Web API Self Host 包，然后单击**安装**。
6. 该包将安装后，单击**关闭**关闭对话框。

> [!NOTE]
> 请确保安装名为 Microsoft.AspNet.WebApi.SelfHost，不 AspNetWebApi.SelfHost 的程序包。


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>创建模型和控制器

本教程使用相同的模型和控制器类[入门](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)教程。

添加一个名为的公共类`Product`。

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

添加一个名为的公共类`ProductsController`。 派生此类从**System.Web.Http.ApiController**。

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

有关此控制器中的代码的详细信息，请参阅[入门](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)教程。 此控制器定义三个 GET 操作：

| URI | 描述 |
| --- | --- |
| / api/产品 | 获取所有产品的列表。 |
| /api/products/*id* | 获取产品的 id。 |
| /api/products/?category=*category* | 按类别获取产品的列表。 |

## <a name="host-the-web-api"></a>承载 Web API

打开文件 Program.cs 并添加以下 using 语句：

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

以下代码添加到**程序**类。

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>（可选）添加一个 HTTP URL Namespace 保留项

此应用程序侦听`http://localhost:8080/`。 默认情况下，侦听特定的 HTTP 地址需要管理员特权。 当运行本教程时，因此，可能会收到此错误:"HTTP 无法注册 URL http://+:8080/"有两种方法，若要避免此错误：

- 使用提升的管理员权限运行 Visual Studio 或
- 使用 Netsh.exe 你向帐户授予权限以保留该 URL。

若要使用 Netsh.exe，使用管理员特权打开命令提示符并输入以下命令： 以下命令：

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

其中*计算机 \ 用户名*是你的用户帐户。

完成之后自承载，请务必删除保留：

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>从客户端应用程序 (C#) 调用 Web API

让我们来编写的简单控制台应用程序调用 web API。

向解决方案中添加新的控制台应用程序项目：

- 在解决方案资源管理器，右键单击该解决方案并选择**添加新项目**。
- 创建名为的新控制台应用&quot;ClientApp&quot;。

![](self-host-a-web-api/_static/image5.png)

使用 NuGet 包管理器添加 ASP.NET Web API Core Libraries 包：

- 从工具菜单中，选择**库程序包管理器**。
- 选择**管理解决方案的 NuGet 包...**
- 在**管理 NuGet 包**对话框中，选择**联机**。
- 在搜索框中，键入&quot;Microsoft.AspNet.WebApi.Client&quot;。
- 选择 Microsoft ASP.NET Web API 客户端库包，然后单击**安装**。

添加在 ClientApp 对 SelfHost 项目的引用：

- 在解决方案资源管理器，右键单击 ClientApp 项目。
- 选择**添加引用**。
- 在**引用管理器**对话框下**解决方案**，选择**项目**。
- 选择 SelfHost 项目。
- 单击 **“确定”**。

![](self-host-a-web-api/_static/image6.png)

打开 Client/Program.cs 文件。 添加以下**使用**语句：

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

添加静态**HttpClient**实例：

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

添加以下方法以按类别列出所有产品，列表按 ID、 产品和产品列表。

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

这些方法的一种都遵循相同的模式：

1. 调用**HttpClient.GetAsync**将 GET 请求发送到的相应 URI。
2. 调用**HttpResponseMessage.EnsureSuccessStatusCode**。 如果 HTTP 响应状态错误代码，此方法将引发异常。
3. 调用**ReadAsAsync&lt;T&gt;** 进行反序列化的 HTTP 响应中的 CLR 类型。 此方法是扩展方法，在中定义**System.Net.Http.HttpContentExtensions**。

**GetAsync**和**ReadAsAsync**方法都异步。 它们返回**任务**表示异步操作的对象。 获取**结果**属性阻止线程，直到操作完成。

有关使用 HttpClient，包括如何进行非阻止调用，请参阅[调用 Web API 从.NET 客户端](../advanced/calling-a-web-api-from-a-net-client.md)。

在调用这些方法之前, 设置 BaseAddress 属性对所 HttpClient 实例"`http://localhost:8080`"。 例如:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

这应输出以下。 （请记住要首先运行 SelfHost 应用程序。）

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
