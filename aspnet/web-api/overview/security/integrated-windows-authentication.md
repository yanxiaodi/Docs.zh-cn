---
uid: web-api/overview/security/integrated-windows-authentication
title: "集成 Windows 身份验证 |Microsoft 文档"
author: MikeWasson
description: "描述如何在 ASP.NET Web API 中使用集成 Windows 身份验证。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a>集成的 Windows 身份验证
====================
通过[Mike Wasson](https://github.com/MikeWasson)

集成的 Windows 身份验证使用户能够使用其 Windows 凭据，使用 Kerberos 或 NTLM 进行登录。 客户端将 Authorization 标头中发送凭据。 Windows 身份验证是最适合 intranet 环境。 有关详细信息，请参阅[Windows 身份验证](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)。

| 优点 | 缺点 |
| --- | --- |
| 的内置于 IIS 中。 -不会在请求中发送的用户凭据。 -如果客户端计算机属于域 （例如，intranet 应用程序），用户不必输入凭据。 | -不建议用于 Internet 应用程序。 -需要在客户端的 Kerberos 或 NTLM 支持。 -客户端必须在 Active Directory 域中。 |

> [!NOTE]
> 如果你的应用程序托管在 Azure 上具有本地 Active Directory 域，请考虑将你的本地 AD 与 Azure Active Directory 联合。 这样一来，用户可以登录其本地凭据，但由 Azure AD 执行身份验证。 有关详细信息，请参阅[Azure 身份验证](../../../visual-studio/overview/2012/windows-azure-authentication.md)。


若要创建使用集成 Windows 身份验证的应用程序，请在 MVC 4 项目向导中选择"Intranet 应用程序"模板。 此项目模板将放置在 Web.config 文件的以下设置：

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

在客户端集成 Windows 身份验证适用于任何支持的浏览器[Negotiate](http://www.ietf.org/rfc/rfc4559.txt)身份验证方案，其中包括大多数主要的浏览器。 对于.NET 客户端应用程序， **HttpClient**类支持 Windows 身份验证：

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Windows 身份验证是易受到跨站点请求伪造 (CSRF) 攻击。 请参阅[防止跨站点请求伪造 (CSRF) 攻击](preventing-cross-site-request-forgery-csrf-attacks.md)。
