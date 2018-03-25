---
title: "在 ASP.NET Core 防止跨站点请求伪造 (XSRF/CSRF) 攻击"
author: steve-smith
description: "了解如何防止对恶意网站可能会影响客户端浏览器和应用程序之间的交互的 web 应用的攻击。"
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 80651a3c3e4c722e0cb96d7cc07de366819f8d1d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>在 ASP.NET Core 防止跨站点请求伪造 (XSRF/CSRF) 攻击

[Steve Smith](https://ardalis.com/)， [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-attack-does-anti-forgery-prevent"></a>防伪阻止哪些攻击？

跨站点请求伪造 (也称为 XSRF 或 CSRF，发音*，请参阅冲浪*) 是针对恶意网站凭此可以影响客户端浏览器和信任的网站之间的交互的 web 托管的应用程序的攻击该浏览器。 因为 web 浏览器将自动与每个请求某些类型的身份验证令牌发送到网站，这些攻击都可能。 这种形式的攻击也称为的*一键式攻击*或*会话乘坐*，因为用户攻击利用的先前进行身份验证会话。

CSRF 攻击的示例：

1. 用户登录到`www.example.com`，使用窗体身份验证。
2. 服务器对用户进行身份验证，并发出包含身份验证 cookie 的响应。
3. 用户访问恶意站点。

   恶意站点包含类似于以下的 HTML 窗体：

   ```html
   <h1>You Are a Winner!</h1>
   <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click Me">
   </form>
   ```

请注意，窗体操作发送到易受攻击的站点，不适用于恶意站点。 这是 CSRF 的"跨站点"部分。

4. 用户可单击提交按钮。 浏览器会自动包括请求的域 （在此情况下易受攻击的站点） 与请求的身份验证 cookie。
5. 请求与用户的身份验证上下文的服务器上运行，并可以执行的已经过身份验证的用户可以执行任何操作。

此示例要求用户通过单击窗体按钮。 无法恶意页：

* 运行自动提交该表单的脚本。
* 将窗体提交作为 AJAX 请求中发送。 
* 使用 CSS 的隐藏的表单。 

使用 SSL，则不会阻止 CSRF 攻击，恶意的站点系统可以将发送`https://`请求。 

一些攻击目标响应的网站终结点`GET`请求，该用例的图像标记可以用于执行 （这种形式的攻击常见论坛在站点是允许映像但阻止 JavaScript） 的操作。 更改状态的应用程序`GET`请求是容易受到恶意攻击的攻击。

因为浏览器向目标网站发送所有相关 cookie，CSRF 攻击是可能对网站进行身份验证，使用 cookie 的。 但是，CSRF 攻击并不局限于利用 cookie。 例如，基本和摘要式身份验证也是易受攻击的。 用户登录时基本或摘要式身份验证后，浏览器会话结束之前会自动发送凭据。

注意： 在此上下文中，*会话*指的是在此期间用户进行身份验证的客户端会话。 它是与服务器端会话无关或[会话中间件](xref:fundamentals/app-state)。

用户可以防止通过 CSRF 漏洞：
* 在完成使用它们时，日志记录从网站中移出。
* 定期清除其浏览器 cookie。

但是，CSRF 漏洞基本上是 web 应用，而不是最终用户有问题。

## <a name="how-does-aspnet-core-mvc-address-csrf"></a>ASP.NET 核心 MVC 如何解决 CSRF？

> [!WARNING]
> ASP.NET 核心实现防 request 伪造使用[ASP.NET 核心数据保护堆栈](xref:security/data-protection/introduction)。 ASP.NET 核心数据保护必须配置为在服务器场中正常工作。 请参阅[配置数据保护](xref:security/data-protection/configuration/overview)有关详细信息。

ASP.NET 核心防 request 伪造默认数据保护配置 

在 ASP.NET 核心 MVC 2.0 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)插入 HTML 窗体元素的防伪令牌。 例如，Razor 文件中的以下标记将自动生成防伪令牌：

```html
<form method="post">
  <!-- form markup -->
</form>
```

自动生成的防伪令牌 HTML 窗体元素发生时：

* `form`标记包含`method="post"`属性 AND

  * 操作属性为空。 ( `action=""`) 或
  * 未提供操作属性。 (`<form method="post">`)

您可以禁用自动生成的防伪令牌通过 HTML 窗体元素：

* 显式禁用`asp-antiforgery`。 例如

  ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* 使用标记帮助器选择标记帮助程序外的窗体元素[！ 选择退出符号](xref:mvc/views/tag-helpers/intro#opt-out)。

  ```html
  <!form method="post">
  </!form>
  ```

* 删除`FormTagHelper`从视图。 你可以删除`FormTagHelper`从视图中向 Razor 视图中添加以下指令：

  ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor 页](xref:mvc/razor-pages/index)会自动防范 XSRF/CSRF。 你无需额外编写任何代码。 请参阅[XSRF/CSRF 和 Razor 页](xref:mvc/razor-pages/index#xsrf)有关详细信息。

防御 CSRF 攻击的最常见方法是同步器令牌模式 (STP)。 STP 是在用户请求具有窗体数据的页时使用的方法。 服务器将发送到客户端的当前用户的标识与关联的令牌。 客户端返回将令牌发送到服务器以进行验证。 如果服务器收到与经过身份验证的用户的标识不匹配的令牌，而拒绝该请求。 该令牌的唯一且不可预测。 该令牌还可用来确保正确地执行序列化的一系列 （确保第 1 页之前之前第 3 页的页 2） 的请求。 ASP.NET 核心 MVC 模板中的所有窗体生成 antiforgery 令牌。 下面的两个示例的视图逻辑生成 antiforgery 令牌：

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

你可以显式添加到 antiforgery 令牌`<form>`没有标记帮助程序使用的 HTML 帮助程序元素`@Html.AntiForgeryToken`:


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

在每个前面的情况下，ASP.NET Core 将添加类似于以下的隐藏的表单字段：
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw">
```

ASP.NET 核心包括三个[筛选器](xref:mvc/controllers/filters)来处理 antiforgery 令牌： `ValidateAntiForgeryToken`， `AutoValidateAntiforgeryToken`，和`IgnoreAntiforgeryToken`。

### <a name="validateantiforgerytoken"></a>ValidateAntiForgeryToken

`ValidateAntiForgeryToken`是操作筛选器，可以应用于单个操作，一个控制器或全局范围内。 将阻止对已应用此筛选器的操作发出的请求，除非请求包含有效的 antiforgery 令牌。

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

`ValidateAntiForgeryToken`属性修饰，包括对操作方法的请求需要使用令牌`HTTP GET`请求。 如果广泛应用，你可以重写它与`IgnoreAntiforgeryToken`属性。

### <a name="autovalidateantiforgerytoken"></a>AutoValidateAntiforgeryToken

ASP.NET Core 应用通常不生成 antiforgery 令牌安全 HTTP 方法 （GET、 HEAD、 选项和跟踪）。 而不是广泛应用`ValidateAntiForgeryToken`属性，然后重写它与`IgnoreAntiforgeryToken`属性，可以使用``AutoValidateAntiforgeryToken``属性。 此属性适用类似`ValidateAntiForgeryToken`特性，只不过它不需要使用以下的 HTTP 方法发出的请求令牌：

* GET
* HEAD
* 选项
* TRACE

我们建议你使用`AutoValidateAntiforgeryToken`广泛的非 API 方案。 这可确保你的 POST 操作保护默认情况下。 替代项是默认情况下，将忽略 antiforgery 令牌除非`ValidateAntiForgeryToken`应用于各个操作的方法。 在此方案中的 POST 操作方法是更有可能不受保护，使你的应用程序容易受到 CSRF 攻击的左侧。 即使匿名文章应发送 antiforgery 令牌。

注意： Api 没有一种用于发送令牌; 的非 cookie 一部分的自动机制您的实现可能将取决于你的客户端代码实现。 下面显示了一些示例。

示例 （类级别）：

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

示例 （全局）：

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a>IgnoreAntiforgeryToken

`IgnoreAntiforgeryToken`使用筛选器以消除 antiforgery 令牌为给定操作 （或控制器） 的需要。 当应用时，此筛选器将重写`ValidateAntiForgeryToken`和/或`AutoValidateAntiforgeryToken`（全局或在控制器上） 在高级别指定筛选器。

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
  [HttpPost]
  [IgnoreAntiforgeryToken]
  public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
  {
    // no antiforgery token required
  }
}
```

## <a name="javascript-ajax-and-spas"></a>JavaScript、 AJAX 和 Spa

在传统的基于 HTML 的应用程序，antiforgery 令牌将传递到使用隐藏的表单域的服务器。 在基于 JavaScript 的现代应用和单页面应用程序 (Spa)，以编程方式进行多请求。 这些 AJAX 请求可能使用其他方法 （如请求标头或 cookie） 将该令牌发送。 如果使用 cookie 来存储身份验证令牌，并在服务器上的 API 请求进行身份验证，则 CSRF 将的潜在问题。 但是，如果本地存储用于存储令牌，CSRF 漏洞，可以缓解，因为从本地存储的值不会自动发送到每个新请求服务器。 因此，使用本地存储来存储客户端和发送令牌，因为请求标头是建议的方法上的 antiforgery 令牌。

### <a name="angularjs"></a>AngularJS

AngularJS 使用到地址 CSRF 的约定。 如果服务器发送具有该名称的 cookie `XSRF-TOKEN`，角`$http`服务将添加的值此 cookie 到标头时它将请求发送到此服务器。 此过程是自动;你不需要显式设置标头。 标头名称是`X-XSRF-TOKEN`。 服务器应检测此标头，并验证其内容。

有关 ASP.NET 核心 API 使用此约定：

* 配置你的应用程序提供在 cookie 中调用的令牌 `XSRF-TOKEN`
* 配置 antiforgery 服务以查找名为的标头 `X-XSRF-TOKEN`

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[查看示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample)。

### <a name="javascript"></a>JavaScript

使用视图支持 JavaScript，你可以创建使用从您的视图中的服务的令牌。 为此，请将注入`Microsoft.AspNetCore.Antiforgery.IAntiforgery`到视图并调用服务`GetAndStoreTokens`，如所示：

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,28)]

此方法不需要直接处理从服务器设置 cookie 或从客户端读取它们。

前面的示例使用 jQuery 的 AJAX POST 标头中读取的隐藏的字段值。 若要使用 JavaScript 来获取令牌的值，使用`document.getElementById('RequestVerificationToken').value`。

JavaScript 可以还访问 cookie 中, 提供的令牌，然后使用 cookie 的内容创建的标头与令牌的值，如下所示。

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

然后，假设构造脚本请求将该令牌发送调用标头中`X-CSRF-TOKEN`，配置 antiforgery 服务以查找`X-CSRF-TOKEN`标头：

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

下面的示例使用 jQuery 发出 AJAX 请求与相应的标头：

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a>配置 Antiforgery

`IAntiforgery` 提供要配置 antiforgery 系统的 API。 它可以在请求`Configure`方法`Startup`类。 下面的示例使用从应用程序的主页上的中间件生成 antiforgery 令牌并将其发送响应中将其作为 cookie （使用上文所述的默认角度命名约定）：


```csharp
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a>选项

你可以自定义[antiforgery 选项](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary)中`ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "mydomain.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|选项        | 描述 |
|------------- | ----------- |
|CookieDomain  | Cookie 的域。 默认为 `null`。 |
|CookieName    | Cookie 的名称。 如果未设置，系统将生成一个唯一的名称开头`DefaultCookiePrefix`("。AspNetCore.Antiforgery。") 下。 |
|CookiePath    | 在 cookie 上设置的路径。 |
|FormFieldName | Antiforgery 系统用于呈现 antiforgery 令牌在视图中的隐藏的表单字段的名称。 |
|HeaderName    | Antiforgery 系统使用的标头的名称。 如果`null`，则系统将考虑仅窗体数据。 |
|RequireSsl    | 指定是否由 antiforgery 系统需要 SSL。 默认为 `false`。 如果`true`，非 SSL 请求将会失败。 |
|SuppressXFrameOptionsHeader | 指定是否禁止生成`X-Frame-Options`标头。 默认情况下，值为"SAMEORIGIN"生成标头。 默认为 `false`。 |

请参阅 https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions 有关详细信息。

### <a name="extending-antiforgery"></a>扩展 Antiforgery

[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)类型允许开发人员通过往返中每个令牌的其他数据扩展的 ANTI-XSRF 系统的行为。 [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_)每次调用方法生成的字段标记，和在生成的标记内嵌入的返回值。 实施者无法返回时间戳、 nonce 或任何其他值，然后调用[ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_)时验证令牌验证此数据。 客户端的用户名已嵌入在生成的令牌中，因此无需包括此信息。 如果令牌包括补充数据但不是`IAntiForgeryAdditionalDataProvider`已配置，不验证补充数据。

## <a name="fundamentals"></a>基础知识

CSRF 攻击依赖于发送与每个请求都会到该域的域关联的 cookie 的默认浏览器的行为。 这些 cookie 存储在浏览器中。 它们通常包括身份验证的用户的会话 cookie。 基于 cookie 的身份验证是一种身份验证的常用形式。 基于令牌的身份验证系统一直增长中受欢迎程度，特别是对于 Spa 和其他"智能客户端"方案。

### <a name="cookie-based-authentication"></a>基于 cookie 的身份验证

一旦用户已经使用其用户名和密码身份验证，它们被颁发一个令牌，可用来将它们标识和验证它们进行了身份验证。 随着工作的附带的每个请求客户端的 cookie 的令牌存储。 生成和验证此 cookie 可通过 cookie 身份验证中间件。 ASP.NET Core 提供 cookie[中间件](xref:fundamentals/middleware/index)其用户主体序列化为的加密 cookie，然后，在后续请求中，将验证该 cookie，重新创建主体，并将它分配给`User`属性`HttpContext`.

当使用 cookie 时，身份验证 cookie 只是一个容器的窗体身份验证票证。 票证作为随每个请求的窗体身份验证 cookie 的值传递和 forms 身份验证，在服务器上，用于标识已经过身份验证的用户。

当用户登录到系统时，用户会话在服务器端上创建并存储在数据库或某些其他持久存储区中。 系统生成在数据存储指向实际会话的会话密钥，并将其作为客户端端 cookie 发送。 Web 服务器将检查此会话密钥的用户请求的资源要求获得授权的任何时间。 系统会检查关联的用户会话是否有权访问请求的资源。 如果是这样，请求将继续。 否则，请求将返回未授权。 在此方法中，使用 cookie 使似乎是有状态应用程序，因为它是能够"记住"的用户具有先前进行身份验证与服务器。

### <a name="user-tokens"></a>用户令牌

基于令牌的身份验证不存储在服务器上的会话。 当用户登录时，它们被颁发一个令牌 （不 antiforgery 令牌）。 此令牌包含要求该令牌进行验证的数据。 它还包含中的窗体的用户信息[声明](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model)。 当用户想要访问要求进行身份验证的服务器资源时，令牌将发送至的其他授权标头中的持有者 {令牌} 形式的服务器。 这使得应用程序无状态，因为在每个后续请求令牌中传递请求服务器端验证。 此令牌不*加密*; 而是*编码*。 在服务器端，都可以解码令牌来访问令牌中的原始信息。 若要在后续请求中发送令牌，或者将其存储在浏览器的本地存储中或在一个 cookie。 不必担心 XSRF 漏洞，如果的令牌存储在本地存储区，但如果的令牌存储在一个 cookie，则会出现问题。

### <a name="multiple-applications-are-hosted-in-one-domain"></a>在一个域中托管多个应用程序

尽管`example1.cloudapp.net`和`example2.cloudapp.net`是不同的主机下的主机之间没有隐式信任关系`*.cloudapp.net`域。 此隐式信任关系允许影响对方的 cookie （控制 AJAX 请求的同源策略不一定适用于 HTTP cookie） 可能不受信任的主机。 ASP.NET 核心运行时，用户名将嵌入到字段标记提供一些缓解。 即使恶意子域能够覆盖会话令牌，它无法生成用户的有效字段令牌。 当承载于此类环境中，内置的 ANTI-XSRF 例程仍不能抵御会话劫持或登录名 CSRF 攻击。 共享宿主环境为 vunerable 会话劫持、 登录 CSRF，和其他的攻击。

### <a name="additional-resources"></a>其他资源

* [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))上[打开 Web 应用程序安全项目](https://www.owasp.org/index.php/Main_Page)(OWASP)。
