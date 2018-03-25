---
title: "阻止在 ASP.NET Core 应用中的打开重定向攻击"
author: ardalis
description: "演示如何阻止对 ASP.NET Core 应用打开重定向攻击"
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/preventing-open-redirects
ms.openlocfilehash: d6cd65a2516c4d5e41428f0c1f2dbbe913ac2123
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a>阻止在 ASP.NET Core 应用中的打开重定向攻击

Web 应用程序将重定向到通过如查询字符串或窗体数据请求指定的 URL 可能可能被篡改将用户重定向到外部、 恶意 URL。 此篡改称为打开重定向攻击。

每当应用程序逻辑将重定向到指定的 URL，你必须验证重定向 URL 尚未被篡改。 ASP.NET 核心具有内置功能来防止应用打开重定向 （也称为打开重定向） 攻击。

## <a name="what-is-an-open-redirect-attack"></a>打开重定向攻击是什么？

Web 应用程序频繁地将用户重定向到登录页访问要求进行身份验证的资源时。 重定向 typlically 包括`returnUrl`查询字符串参数，以便用户可以在用户成功登录后返回最初请求的 url。 用户进行身份验证后，它们在重定向到他们最初具有请求的 URL。

因为请求的查询字符串中指定的目标 URL，则恶意用户可能篡改查询字符串。 篡改过的查询字符串可能导致要将用户重定向到外部、 恶意站点的站点。 这种技术称为打开重定向 （或重定向） 攻击。

### <a name="an-example-attack"></a>示例攻击

恶意用户可以开发用于允许对用户的凭据或您的应用程序上的敏感信息的恶意用户访问的攻击。 若要开始攻击，它们使用户链接到网站的登录页，单击与`returnUrl`添加到的 URL 查询字符串值。 例如， [NerdDinner.com](http://nerddinner.com) （编写 ASP.NET MVC 的） 的示例应用程序包括此处这样的登录页： ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``。 这种攻击然后执行下列步骤：

1. 用户单击的链接``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn``(请注意，第二个 URL 是 nerddi**n**er，不 nerddi**nn**er)。
2. 用户成功登录。
3. 用户 （通过站点） 重定向到``http://nerddiner.com/Account/LogOn``（如下所示的真正的站点的恶意站点）。
4. 用户再次登录 （提供恶意站点其凭据） 和重定向回真正的站点。

用户将可能认为其第一次尝试登录失败，并且其第二个已成功。 它们将很可能仍然无法察觉其凭据已泄漏。

![打开重定向攻击过程](preventing-open-redirects/_static/open-redirection-attack-process.png)

除了登录页的某些站点提供重定向页或终结点。 假设你的应用程序的页的打开的重定向， ``/Home/Redirect``。 攻击者可以创建，例如，将转到一封电子邮件中的链接``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``。 典型的用户将看一看 URL，并请参阅开始使用你的站点名称。 信任的用户将单击此链接。 打开重定向将为网络钓鱼站点，这看起来相同地区，然后发送用户和用户很可能会登录到他们认为是你的站点。

## <a name="protecting-against-open-redirect-attacks"></a>针对打开重定向攻击提供保护

当开发 web 应用程序，会将所有用户提供数据视为不受信任。 如果你的应用程序具有基于 URL 的内容将用户重定向的功能，请确保，此类重定向仅这样的是本地应用程序中 （或已知的 URL，没有任何可能在查询字符串中提供的 URL）。

### <a name="localredirect"></a>LocalRedirect

使用``LocalRedirect``帮助器方法的基`Controller`类：

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

``LocalRedirect``如果指定非本地 URL，将引发异常。 否则，其行为就像``Redirect``方法。

### <a name="islocalurl"></a>IsLocalUrl

使用[IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_)方法以测试 Url，然后重定向：

下面的示例演示如何检查 URL 重定向之前是本地。

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

`IsLocalUrl`方法可防止用户无意中重定向到恶意的站点。 你可以登录时需要本地 URL 的位置的情况下提供非本地 URL 提供的 URL 的详细信息。 日志记录重定向 Url 可用于诊断重定向攻击。
