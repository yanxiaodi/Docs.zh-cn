---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "启用 Windows 身份验证中 Katana |Microsoft 文档"
author: MikeWasson
description: "这篇文章演示如何启用 Katana 中 Windows 身份验证。 它涵盖了两种方案： 到主机 Katana，使用 IIS 和使用 HttpListener 自承载 Kat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="enabling-windows-authentication-in-katana"></a>启用 Windows 身份验证中 Katana
====================
通过[Mike Wasson](https://github.com/MikeWasson)

> 这篇文章演示如何启用 Katana 中 Windows 身份验证。 它涵盖了两种方案： 到主机 Katana，使用 IIS 和使用 HttpListener 自承载自定义过程中的 Katana。 感谢您对 Barry Dorrans、 David Matson 和 Chris 跨查看这篇文章。


Katana 是 Microsoft 的实现[OWIN](http://owin.org/)，用于.NET 的打开 Web 界面。 你可以阅读 OWIN 和 Katana 的简介[此处](an-overview-of-project-katana.md)。 OWIN 体系结构具有多个层：

- 主机： 管理 OWIN 管道在其中运行的进程。
- 服务器： 打开网络套接字并侦听请求。
- 中间件： 处理的 HTTP 请求和响应。

Katana 目前提供两个服务器，这两种支持 Windows 集成身份验证：

- **Microsoft.Owin.Host.SystemWeb**. 与 ASP.NET 管道中使用 IIS。
- **Microsoft.Owin.Host.HttpListener**. 使用[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)。 此服务器当前是默认选项，当自承载 Katana。

> [!NOTE]
> Katana 当前不提供 OWIN 中间件对于 Windows 身份验证，因为此功能已在服务器中可用。


## <a name="windows-authentication-in-iis"></a>在 IIS 中的 Windows 身份验证

使用 Microsoft.Owin.Host.SystemWeb，你可以只需启用 IIS 中的 Windows 身份验证。

让我们首先创建一个新的 ASP.NET 应用程序，使用"ASP.NET 空 Web 应用程序"项目模板。

![](enabling-windows-authentication-in-katana/_static/image1.png)

接下来，添加 NuGet 包。 从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在 Package Manager Console 窗口中，输入以下命令：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

现在，添加一个名为类`Startup`替换为以下代码：

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

这是你需要创建"Hello world"应用程序以便 OWIN，在 IIS 上运行。 按 F5 调试该应用程序。 你应该会看到"Hello World ！" 在浏览器窗口中。

![](enabling-windows-authentication-in-katana/_static/image2.png)

接下来，我们将启用 IIS Express 中的 Windows 身份验证。 从**视图**菜单上，选择**属性**。 单击解决方案资源管理器来查看项目属性中的项目名称。

在**属性**窗口中，设置**匿名身份验证**到**禁用**并设置**Windows 身份验证**到**启用**。

![](enabling-windows-authentication-in-katana/_static/image3.png)

当从 Visual Studio 运行应用程序时，IIS Express 将要求用户的 Windows 凭据。 你可以通过使用看到此[Fiddler](http://fiddler2.com/home)或另一个 HTTP 调试工具。 下面是示例 HTTP 响应：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

此响应中的 Www-authenticate 标头指示服务器支持[Negotiate](http://www.ietf.org/rfc/rfc4559.txt)协议，从而使用 Kerberos 或 NTLM。

更高版本，在部署到的服务器应用程序时，请按照[这些步骤](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)在该服务器上启用 IIS 中的 Windows 身份验证。

## <a name="windows-authentication-in-httplistener"></a>在 HttpListener 的 Windows 身份验证

如果你使用 Microsoft.Owin.Host.HttpListener 自承载 Katana，则可以直接在上启用 Windows 身份验证**HttpListener**实例。

首先，创建一个新的控制台应用程序。 接下来，添加 NuGet 包。 从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在 Package Manager Console 窗口中，输入以下命令：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

现在，添加一个名为类`Startup`替换为以下代码：

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

此类实现同一"Hello world"示例从之前，但它还将 Windows 身份验证设置为身份验证方案。

内部`Main`函数中，启动 OWIN 管道：

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

你可以在 Fiddler 来确认应用程序正在使用 Windows 身份验证发送请求：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>相关主题

[项目 Katana 概述](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[了解 OWIN MVC 5 中的窗体身份验证](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
