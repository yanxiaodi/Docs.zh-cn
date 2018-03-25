---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: "使用 Windows 身份验证 (VB) 的用户进行身份验证 |Microsoft 文档"
author: microsoft
description: "了解如何在 MVC 应用程序的上下文中使用 Windows 身份验证。 了解如何启用 Windows 身份验证在你的应用程序 web co..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 63b1266e03041c4261e71fd25e988c63932b503e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="authenticating-users-with-windows-authentication-vb"></a>使用 Windows 身份验证 (VB) 的用户进行身份验证
====================
by [Microsoft](https://github.com/microsoft)

> 了解如何在 MVC 应用程序的上下文中使用 Windows 身份验证。 你了解如何启用应用程序的 web 配置文件中的 Windows 身份验证以及如何使用 IIS 中配置身份验证。 最后，你将了解如何使用 [Authorize] 属性来限制对特定 Windows 用户或组的控制器操作的访问。


本教程旨在说明如何可以充分利用的安全功能内置于 Internet Information Services 到密码保护 MVC 应用程序中的视图。 了解如何让控制器操作只能由特定 Windows 用户或用户特定的 Windows 组成员的调用。

使用 Windows 身份验证时有意义，您构建了一个内部公司网站 （intranet 站点） 和您希望用户能够访问网站时使用其标准的 Windows 用户名和密码。 如果你在构建面向网站 （Internet 网站） 外，请考虑改为使用 Forms 身份验证。

#### <a name="enabling-windows-authentication"></a>启用 Windows 身份验证

在创建新的 ASP.NET MVC 应用程序时，默认情况下未启用 Windows 身份验证。 窗体身份验证是启用的 MVC 应用程序的默认身份验证类型。 通过修改 MVC 应用程序的 web 配置 (web.config) 文件，必须启用 Windows 身份验证。 查找&lt;身份验证&gt;部分，然后修改它以使用 Windows，而不是这样的窗体身份验证：

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

当你启用 Windows 身份验证时，你的 web 服务器将成为负责对用户进行身份验证。 通常，有两种不同类型的创建和部署 ASP.NET MVC 应用程序时，你使用的 web 服务器。

首先，在开发的 MVC 应用程序时，你使用 Visual Studio 所含的 ASP.NET 开发 Web 服务器。 默认情况下，ASP.NET 开发 Web 服务器的当前 Windows 帐户 （无论你用于登录到 Windows 帐户） 的上下文中执行所有页面。

ASP.NET 开发 Web 服务器还支持 NTLM 身份验证。 你可以通过右键单击解决方案资源管理器窗口中的项目的名称并选择属性来启用 NTLM 身份验证。 接下来，选择 Web 选项卡并选中 NTLM 复选框 （请参见图 1）。

**图 1 – 启用 ASP.NET 开发 Web 服务器的 NTLM 身份验证**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

生产 web 应用程序，在手的形状，你使用 IIS 作为 web 服务器。 IIS 支持多种类型的身份验证包括：

- 基本身份验证 – 定义为 HTTP 1.0 协议的一部分。 通过 Internet 以明文形式 (Base64 编码) 发送用户名和密码。 -摘要式身份验证-发送密码，而不是本身，在 internet 上的密码哈希的值。 -集成 Windows (NTLM) 身份验证 – 要在使用 windows 的 intranet 环境中使用的身份验证的最佳类型。 -证书身份验证 – 启用身份验证使用客户端证书。 证书映射到 Windows 用户帐户。

> [!NOTE] 
> 
> 有关这些不同类型的身份验证的详细概述，请参阅[https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Internet Information Services 管理器可用于启用特定类型的身份验证。 请注意，所有类型的身份验证都不可用对于每个操作系统。 此外，如果您正在使用 Windows Vista IIS 7.0，你将需要启用 Windows 身份验证的不同类型，它们显示在 Internet Information Services 管理器中之前。 打开**控制面板、 程序、 程序和功能，打开或关闭 Windows 功能**，然后展开 Internet 信息服务节点 （请参见图 2）。

**图 2 – 启用 Windows IIS 功能**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

使用 Internet Information Services，可以启用或禁用不同类型的身份验证。 例如，图 3 说明禁用匿名身份验证和启用集成 Windows (NTLM) 身份验证时使用的 IIS 7.0。

**图 3 – 启用集成的 Windows 身份验证**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>授权 Windows 用户和组

启用 Windows 身份验证后，你可以使用&lt;Authorize&gt;属性来控制对控制器或控制器操作的访问。 此特性可以应用于整个的 MVC 控制器或特定控制器操作。

例如，列表 1 中的主页控制器公开名为 index （）、 CompanySecrets() 和 StephenSecrets() 的三个操作。 任何人都可以调用 index （） 操作。 但是，只有 Windows 本地管理员组的成员可以调用 CompanySecrets() 操作。 最后，仅指定 Stephen （在 Redmond 域中） 的 Windows 域用户可以调用 StephenSecrets() 操作。

**列表 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> 由于 Windows 用户帐户控制 (UAC)，使用 Windows Vista 或 Windows Server 2008 时，本地 Administrators 组将行为与其他组不同。 &lt;Authorize&gt;属性不会正确会识别本地 Administrators 组的成员，除非你修改你的计算机的 UAC 设置。


完全时会发生什么情况你尝试在不正确的权限调用的控制器操作取决于启用身份验证的类型。 使用 ASP.NET 开发服务器时，默认情况下，你只会收到一个空白页。 使用提供的页面**401 未授权**HTTP 响应状态。

如果，另一方面，你将 IIS 用于匿名身份验证禁用和启用，基本身份验证，则不断遇到登录对话框提示你请求的受保护的页每次 （参见图 4）。

**图 4 – 基本身份验证登录对话框**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>摘要

本教程所述的 ASP.NET MVC 应用程序的上下文中，你就可以使用 Windows 身份验证。 您学习了如何启用应用程序的 web 配置文件中的 Windows 身份验证以及如何使用 IIS 中配置身份验证。 最后，您学习了如何使用&lt;Authorize&gt;特性限制对特定 Windows 用户或组的控制器操作的访问。

>[!div class="step-by-step"]
[上一页](authenticating-users-with-forms-authentication-vb.md)
[下一页](preventing-javascript-injection-attacks-vb.md)
