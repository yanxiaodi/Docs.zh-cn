---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: "阻止打开重定向攻击 (C#) |Microsoft 文档"
author: jongalloway
description: "本教程介绍如何在 ASP.NET MVC 应用程序阻止打开重定向攻击。 本教程讨论所做的更改..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 17944c0600a174176e3e9940f414b34f0835b800
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="preventing-open-redirection-attacks-c"></a>阻止打开重定向攻击 (C#)
====================
通过[Jon Galloway](https://github.com/jongalloway)

> 本教程介绍如何在 ASP.NET MVC 应用程序阻止打开重定向攻击。 本教程讨论了已在 ASP.NET MVC 3 中 AccountController 中进行的更改，并演示你现有的 ASP.NET MVC 1.0 和 2 的应用程序中，你就可以应用这些更改。


## <a name="what-is-an-open-redirection-attack"></a>什么是打开的重定向攻击？

任何 web 应用程序将重定向到通过如查询字符串或窗体数据请求指定的 URL 可能可能被篡改将用户重定向到外部、 恶意 URL。 此篡改称为打开重定向攻击。

每当应用程序逻辑将重定向到指定的 URL，你必须验证重定向 URL 尚未被篡改。 默认情况下 AccountController 中用于 ASP.NET MVC 1.0 和 ASP.NET MVC 2 的登录名极易收到打开重定向攻击。 幸运的是，很容易地更新您现有的应用程序，以使用从 ASP.NET MVC 3 预览版更正操作。

若要了解漏洞的修补程序，让我们看一下默认 ASP.NET MVC 2 Web 应用程序项目中的登录重定向工作原理。 在此应用程序，尝试访问的控制器操作具有 [Authorize] 特性将重定向未经授权的用户到 /Account/LogOn 视图。 此重定向到 /Account/LogOn 将包括 returnUrl 查询字符串参数，以便用户可以在用户成功登录后返回最初请求的 url。

在下面的屏幕截图中，我们可以看到，尝试访问 /Account/ChangePassword 视图未登录时导致重定向到 /Account/LogOn？ReturnUrl = %2faccount%2fChangePassword %2f。

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**图 01**： 与打开的重定向的登录页

由于 ReturnUrl 查询字符串参数未通过验证，攻击者可以修改它以将任何 URL 地址注入要执行的打开的重定向攻击的参数。 若要进行此演示，我们可以修改的 ReturnUrl 参数[http://bing.com](http://bing.com)，因此将生成的登录 URL/帐户/登录？ ReturnUrl = http://www.bing.com/。 在成功登录到站点，我们将重定向到[http://bing.com](http://bing.com)。此重定向未通过验证，因为它无法改为指向恶意站点，试图欺骗用户。

### <a name="a-more-complex-open-redirection-attack"></a>更复杂的打开重定向攻击

打开重定向攻击是非常危险的因为攻击者知道，我们将尝试登录到特定的网站，这将使我们易受[网络钓鱼攻击](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx)。 例如，攻击者无法尝试捕获其密码，将恶意电子邮件发送到网站用户。 让我们看一下这工作原理 NerdDinner 站点上。 （请注意，已更新实时 NerdDinner 站点以防止打开重定向攻击。）

首先，攻击者向我们发送链接到 NerdDinner 包括重定向到他们伪造的页面上的登录页：

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

请注意，返回的 URL 指向 nerddiner.com，缺少"n"从 word dinner。 在此示例中，这是攻击者控制的域。 当我们访问上面的链接时，我们将会转到合法 NerdDinner.com 登录页。

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**图 02**: NerdDinner 与打开的重定向的登录页

我们正确登录时，ASP.NET MVC AccountController 登录操作将重我们定向到 returnUrl 查询字符串参数中指定的 URL。 在这种情况下，它是攻击者已进入，即 URL [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn)。 除非我们极 watchful，它是很有可能，我们不会发现这种情况，尤其是因为攻击者已小心，确保其伪造的页的外观完全一样的合法的登录页。 此登录页包括错误消息，请求我们重新登录。 非常笨拙我们，我们必须有误我们的密码。

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**图 03**： 伪造 NerdDinner 登录屏幕

当我们重新键入我们用户名和密码时，伪造的登录页将信息保存，并向我们发送回合法 NerdDinner.com 站点。 此时，NerdDinner.com 站点具有已进行身份验证 us，因而伪造的登录页可以直接进入该页面将重定向。 最终结果是攻击者将有我们用户名和密码，并且我们不会意识到，我们已向其提供它。

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>查看在 AccountController 登录操作易受攻击的代码

ASP.NET MVC 2 应用程序中的登录操作的代码所示。 请注意，一旦成功登录，则控制器将返回到的重定向到 returnUrl。 你可以看到与 returnUrl 参数执行任何验证。

**列表 1 – 中的 ASP.NET MVC 2 登录操作`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

现在让我们看一下 ASP.NET MVC 3 登录操作的更改。 此代码已更改，以通过调用中名为的 System.Web.Mvc.Url 帮助器类的新方法验证 returnUrl 参数`IsLocalUrl()`。

**列出 2 – 在 ASP.NET MVC 3 登录操作`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

已更改来验证的调用中 System.Web.Mvc.Url 帮助器类的新方法的返回 URL 参数`IsLocalUrl()`。

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>保护你的 ASP.NET MVC 1.0 和 MVC 2 应用程序

通过添加 IsLocalUrl() 帮助器方法和更新的登录操作来验证该 returnUrl 参数，我们可以在我们现有的 ASP.NET MVC 1.0 和 2 的应用程序充分利用 ASP.NET MVC 3 更改。

ASP.NET Web Pages 应用程序也使用 UrlHelper IsLocalUrl() 方法实际上就调用到中 System.Web.WebPages，作为此验证的方法。

**列出 3-从 ASP.NET MVC 3 UrlHelper IsLocalUrl() 方法`class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

IsUrlLocalToHost 方法包含实际的验证逻辑，列出 4 中所示。

**列出 4 – 从 System.Web.WebPages RequestExtensions 类 IsUrlLocalToHost() 方法**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

在我们的 ASP.NET MVC 1.0 或 2 的应用程序中，我们将向 AccountController 中，添加 IsLocalUrl() 方法，但你建议如有可能将其添加到一个单独的帮助器类。 我们会使两个小型更改到 IsLocalUrl() 的 ASP.NET MVC 3 版本，以便它将在 AccountController 中工作。 首先，我们将更改它通过公共方法为私有方法，因为可作为控制器操作访问控制器中的公共方法。 其次，我们将修改检查 URL 主机针对应用程序主机的调用。 调用，将使用本地 requestcontext 已 UrlHelper 类中的字段。 而不是使用这种情况。RequestContext.HttpContext.Request.Url.Host，我们将使用它。Request.Url.Host。 下面的代码演示在 ASP.NET MVC 1.0 和 2 的应用程序使用了控制器类修改后的 IsLocalUrl() 方法。

**列出 5 – IsLocalUrl() 方法，这被修改用于 MVC 控制器类**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

IsLocalUrl() 方法已到位，我们就可以调用它从我们的登录操作来验证该 returnUrl 参数，如下面的代码中所示。

**列出 6-验证 returnUrl 参数更新登录方法**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

现在我们可以通过尝试使用外部的返回 URL 登录测试打开重定向攻击。 让我们使用/帐户/登录？ ReturnUrl = http://www.bing.com/ 试。

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**图 04**： 测试更新后的登录操作

已成功登录后，我们将重定向到 Home/Index 控制器操作而不是外部 URL。

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**图 05**： 打开重定向攻击失效

## <a name="summary"></a>摘要

重定向 Url 作为应用程序的 URL 中的参数传递时，会发生打开重定向攻击。 模板包括代码，以防止对 ASP.NET MVC 3 打开重定向攻击。 你可以添加与 ASP.NET MVC 1.0 和 2 的应用程序进行一些修改此代码。 若要防止打开重定向攻击，登录到 ASP.NET 1.0 和 2 的应用程序时，将 IsLocalUrl() 方法添加并验证中的登录操作的 returnUrl 参数。
