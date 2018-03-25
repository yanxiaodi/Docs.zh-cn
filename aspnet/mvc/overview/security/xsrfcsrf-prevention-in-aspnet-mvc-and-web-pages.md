---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: "在 ASP.NET MVC 和 Web Pages XSRF/CSRF 预防 |Microsoft 文档"
author: Rick-Anderson
description: "跨站点请求伪造 （也称为 XSRF 或 CSRF） 是一种针对 web 托管的应用程序恶意网站凭此可以影响 interacti 攻击..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2013
ms.topic: article
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 6cf30daa7ed966b11405cec715c5bc803b567249
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>在 ASP.NET MVC 和 Web Pages XSRF/CSRF 防护
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 跨站点请求伪造 （也称为 XSRF 或 CSRF） 是针对恶意网站凭此可以影响客户端浏览器和受该浏览器信任的网站之间的交互的 web 托管的应用程序的攻击。 因为 web 浏览器将发送到网站的自动与每个请求的身份验证令牌，这些攻击都可能。 典型示例是身份验证 cookie，如 ASP。NET 的窗体身份验证票证。 但是，使用任何持久身份验证 （如 Windows 身份验证、 Basic 等） 的网站可以受攻击目标。
> 
> XSRF 攻击是不同于网络钓鱼攻击。 网络钓鱼攻击需要与受害者进行交互。 在网络钓鱼攻击中，恶意网站将仿冒目标网站，并向攻击者提供敏感信息受到欺骗的受害者。 在 XSRF 攻击中，没有通常无需交互不必与受害者。 相反，攻击者依赖于自动向目标网站发送所有相关 cookie 的浏览器。
> 
> 有关详细信息，请参阅[打开 Web 应用程序安全项目](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))。


## <a name="anatomy-of-an-attack"></a>攻击剖析

若要指导 XSRF 攻击，请考虑想要执行一些联机银行事务的用户。 此用户第一次访问 WoodgroveBank.com 和中，日志的位置响应标头将包含其身份验证 cookie:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

由于身份验证 cookie 的会话 cookie，它将自动清除浏览器时在浏览器进程退出。 但是，在此之前，浏览器将自动包括具有 WoodgroveBank.com 的每个请求 cookie。用户现在想要将 $1000 传输到另一个帐户，因此她填写银行站点上的表单和浏览器向服务器发出此请求：

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

因为此操作具有副作用 （它将启动资金事务），已选择银行站点需要才能启动此操作的 HTTP POST。 服务器读取请求的身份验证令牌、 查找当前用户的帐户数、 验证足够的资金存在，，然后启动到目标帐户事务。

她联机银行完成后，用户导航离开银行站点并访问 web 上的其他位置。 其中一个 – fabrikam.com – 这些站点包括嵌入在页面上的以下标记&lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

它会导致浏览器发出此请求：

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

攻击者利用事实，用户可能仍具有有效的身份验证令牌的目标 web 站点，并且她使用 Javascript 的小片段来导致浏览器对目标站点中自动进行 HTTP POST。 如果仍有效的身份验证令牌，银行网站将启动到攻击者选择的帐户的 250 美元的传输。

### <a name="ineffective-mitigations"></a>无效的缓解措施

值得注意的是在上面的方案中，则表明 WoodgroveBank.com 正在通过 SSL 访问，并且必须仅 SSL 身份验证 cookie 已不足以阻止攻击。 攻击者是能够指定[URI 方案](http://en.wikipedia.org/wiki/URI_scheme)(https) 在她&lt;窗体&gt;元素，并浏览器将继续发送到目标站点未过期的 cookie，只要这些 cookie 是一致的 uri预期目标的方案。

有人可能会说，用户应只需不访问不受信任的站点，作为访问仅受信任的站点可帮助联机保持安全。 没有为此，某些真实但遗憾的是此建议并不总是可行的。 用户可能是"信任"本地新闻站点 ConsolidatedMessenger。 ConsolidatedMessenger.com，直至相反，站点的访问，但该站点具有一个 XSS 漏洞，这样攻击者将注入 fabrikam.com 正在运行的代码的同一代码段。

你可以验证传入的请求具有[Referer 标头](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14)引用你的域。 这将停止从第三方域无意地提交请求。 但是，一些人禁用出于隐私原因，其浏览器的引用站点标头和牺牲品未安装某些不安全软件时，攻击者可以有时伪装成该标头。 验证[Referer 标头](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14)不被视为阻止 XSRF 攻击的安全方法。

## <a name="web-stack-runtime-xsrf-mitigations"></a>Web 堆栈运行时 XSRF 缓解措施

ASP.NET Web 堆栈运行时使用的一个变体[同步器令牌模式](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern)防御 XSRF 攻击。 同步器令牌模式的一般形式是两个 ANTI-XSRF 令牌使用 （除了身份验证令牌） 的每个 HTTP POST 向服务器提交： 作为 cookie，窗体值作为另一个令牌。 ASP.NET 运行时生成的令牌值不是确定性或攻击者可预测。 提交令牌，服务器将允许的请求仅在两种令牌通过比较检查时，才继续。

XSRF 请求验证*会话令牌*HTTP cookie 作为存储和当前包含其有效负载中的以下信息：

- 安全令牌中，包含随机的 128 位标识符。   
 下图显示了与 Internet Explorer F12 开发人员工具显示 XSRF 请求验证会话令牌: (请注意这是当前的实现，并且主题，甚至可能会，更改。)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*字段标记*存储为`<input type="hidden" />`并包含其有效负载中的以下信息：

- 登录的用户的用户名 （如果通过身份验证）。
- 提供的任何其他数据[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)。

ANTI-XSRF 令牌的负载进行加密和签名，以便使用工具来检查令牌时，不能查看的用户名。 如果 web 应用程序目标 ASP.NET 4.0，加密服务提供的[MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx)例程。 当 web 应用程序针对 ASP.NET 4.5 或更高版本的加密服务提供的[MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110))例程，可提供更好的性能、 可扩展性和安全性。 请参阅以下博客文章的更多详细信息：

- [在 ASP.NET 4.5 中的加密改进、 pt。1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [在 ASP.NET 4.5 中的加密改进、 pt。2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [在 ASP.NET 4.5 中的加密改进、 pt。3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>生成令牌

若要生成的 ANTI-XSRF 令牌，请调用[ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx)从的 MVC 视图的方法或@AntiForgery.GetHtml从 Razor 页 （)。 然后，运行时将执行以下步骤：

1. 如果当前 HTTP 请求已包含的 ANTI-XSRF 会话令牌 (ANTI-XSRF cookie \_ \_RequestVerificationToken)，从其提取的安全令牌。 如果 HTTP 请求不包含 ANTI-XSRF 会话令牌或安全令牌提取失败，将生成新的随机的 ANTI-XSRF 令牌。
2. ANTI-XSRF 字段标记为生成使用从上面的步骤 (1) 和标识的当前登录的用户的安全令牌。 (有关确定用户标识的详细信息，请参阅**[具有特殊的支持方案](#_Scenarios_with_special)**下面一节。)此外，如果[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx)是配置，运行时将调用其[GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx)方法并将返回的字符串包括在字段标记。 (请参阅**[配置和可扩展性](#_Configuration_and_extensibility)**部分以了解更多信息。)
3. 如果在步骤 (1) 中生成新的 ANTI-XSRF 令牌，新的会话令牌将创建包含它，并且将添加到出站 HTTP cookie 集合。 步骤 (2) 中的字段令牌将包装在`<input type="hidden" />`元素，并且此 HTML 标记将是的返回值`Html.AntiForgeryToken()`或`AntiForgery.GetHtml()`。

## <a name="validating-the-tokens"></a>验证令牌

若要验证传入的 ANTI-XSRF 令牌，开发人员包括[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx)她 MVC 操作或控制器或她调用特性`@AntiForgery.Validate()`从她 Razor 页。 运行时将执行以下步骤：

1. 读取的传入会话令牌和字段标记，并从每个提取 ANTI-XSRF 令牌。 ANTI-XSRF 令牌必须每个步骤 (2) 生成例程中相同。
2. 如果当前用户进行身份验证，与字段标记中存储的用户名进行比较她的用户名。 用户名必须与匹配。
3. 如果[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)配置，则运行时会调用其*ValidateAdditionalData*方法。 该方法必须返回布尔值*true*。

如果验证成功，则允许请求以继续。 如果验证失败，将引发 framework *HttpAntiForgeryException*。

## <a name="failure-conditions"></a>失败条件

任何从 ASP.NET Web 堆栈运行 v2 *HttpAntiForgeryException*期间引发验证将包含有关发生的问题的详细的信息。 当前定义的失败条件是：

- 不存在请求中的会话令牌或窗体标记。
- 会话令牌或窗体令牌不可读。 此最可能的原因是在运行的 ASP.NET Web 堆栈运行时或场的版本不匹配的场其中&lt;machineKey&gt;机之间不同，在 Web.config 中的元素。 Fiddler 等工具可用于通过篡改任一 ANTI-XSRF 令牌强制此异常。
- 会话令牌和字段令牌已交换。
- 会话令牌和字段标记包含不匹配的安全令牌。
- 字段标记中嵌入用户名与当前登录的用户的用户名不匹配。
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* 方法返回*false*。

ANTI-XSRF 设施可能还执行生成令牌或验证过程的其他检查，这些检查期间出现的故障可能会导致引发异常。 请参阅[WIF / ACS / 基于声明的身份验证](#_WIF_ACS)和**[配置和可扩展性](#_Configuration_and_extensibility)**部分以获取更多信息。

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>使用特殊的支持方案

### <a name="anonymous-authentication"></a>匿名身份验证

ANTI-XSRF 系统包含额外支持匿名用户，其中"匿名"指用户其中*IIdentity.IsAuthenticated*属性返回*false*。 方案包括提供 XSRF 保护的登录页 （之前用户进行身份验证） 和自定义身份验证方案，其中应用程序使用一种机制不*IIdentity*来识别用户。

若要支持这些方案，请记住的会话和字段令牌通过安全令牌，这是一个 128 位随机生成不透明标识符联接。 此安全令牌用于跟踪单个用户会话，当她导航站点，以便有效地提供服务的匿名标识符的用途。 空字符串用于代替用户名上面所述的生成和验证例程。

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / 基于声明的身份验证

通常情况下， *IIdentity*内置于.NET Framework 的类具有属性， *IIdentity.Name*足以唯一地标识特定的应用程序内的特定用户。 例如， *FormsIdentity.Name*返回存储在成员资格数据库 （这是唯一的具体取决于该数据库的所有应用程序），用户名*WindowsIdentity.Name*返回域限定标识用户，依次类推。 这些系统提供不仅身份验证;它们还*标识*到应用程序的用户。

基于声明的身份验证，另一方面，不一定需要标识特定用户。 相反， *ClaimsPrincipal*和*ClaimsIdentity*类型都与一组相关联*声明*实例，其中的单个声明可能是"is 岁 18 +"或"是的管理员"为任何其他值。 由于尚未一定标识该用户，不能使用运行时*ClaimsIdentity.Name*属性作为该特定用户的唯一标识符。 团队已发现实际应用示例其中*ClaimsIdentity.Name*返回*null*、 返回友好 （显示） 名称，或否则将返回一个字符串，并不适合用作唯一标识符为用户。

许多使用基于声明的身份验证的部署使用[Azure Access Control 服务](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx)(ACS) 尤其。 ACS 允许开发人员配置单个*标识提供程序*（ADFS，Microsoft 帐户提供程序，如 OpenID 提供程序如 yahoo ！ 等），并标识提供程序返回*命名标识符*. 这些名称标识符可能包含个人身份信息 (PII)，如电子邮件地址，或它们无法将匿名处理如专用个人标识符 (PPID)。 无论如何，元组 （名称标识符中的标识提供程序） 足够作为特定用户的适当的跟踪令牌，而她浏览站点，以便在生成时，ASP.NET Web 堆栈运行时可以使用此元组代替用户名和验证 ANTI-XSRF 字段令牌。 标识提供程序和名称标识符的特定 Uri 是：

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(请参阅此[ACS 文档页](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx)有关详细信息。)

当生成或验证令牌时，ASP.NET Web 堆栈运行时将在运行时尝试绑定到类型：

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35`（有关 WIF SDK。)
- `System.Security.Claims.ClaimsIdentity`（用于.NET 4.5)。

如果这些类型存在，并且当前用户的*IIIIdentity*实现或子类其中一种类型，（标识提供程序，名称标识符），将使用的 ANTI-XSRF 设施代替用户名生成时的元组和验证令牌。 如果存在任何此类元组，不则请求将失败并出错到开发人员描述如何配置 ANTI-XSRF 系统，以便了解中使用的特定基于声明的身份验证机制。 请参阅**[配置和可扩展性](#_Configuration_and_extensibility)**部分以了解更多信息。

### <a name="oauth--openid-authentication"></a>OAuth / OpenID 身份验证

最后，ANTI-XSRF 设施具有应用程序的使用 OAuth 或 OpenID 身份验证的特殊的支持。 此支持是基于启发式方法的： 如果当前*IIdentity.Name*开始 http:// 或 https:// 开头，则将完成用户名比较使用序号比较器，而不是默认 OrdinalIgnoreCase 比较器。

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>配置和可扩展性

有时，开发人员可能想更加严格地控制 ANTI-XSRF 生成和验证行为。 例如，可能会自动将 HTTP cookie 添加到响应的 MVC 和 Web Pages 帮助器的默认行为是不可取，和开发人员可能想要保留在其他位置的令牌。 存在两个 Api，以对此有帮助：

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens*方法接受的输入现有 XSRF 请求验证会话令牌 （其中可能为 null） 和作为生成输出新 XSRF 请求验证会话令牌和字段令牌。 令牌是采用不带修饰符; 只是不透明字符串*formToken*值将为实例不能进行包装在&lt;输入&gt;标记。 *NewCookieToken*值可能为 null; 如果发生这种情况，则*oldCookieToken*值是仍然有效，并且需要设置任何新的响应 cookie。 调用方*GetTokens*负责保持任何必要的响应 cookie 或生成任何必要的标记; *GetTokens*方法本身不会更改的副作用的响应。 *验证*方法采用传入会话和字段标记，并且在其运行前面提到的验证逻辑。

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

开发人员可以配置应用程序中的 ANTI-XSRF 系统\_启动。 以编程方式配置。 静态属性*AntiForgeryConfig*类型如下所述。 使用声明的大多数用户将想要设置 UniqueClaimTypeIdentifier 属性。

| **Property** | **说明** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) ，令牌生成期间提供的附加数据和令牌验证期间使用额外数据。 默认值是*null*。 有关详细信息，请参阅[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)部分。 |
| **CookieName** | 提供用于存储的 ANTI-XSRF 会话令牌的 HTTP cookie 的名称的字符串。 如果未设置此值，名称将自动生成基于应用程序的已部署的虚拟路径。 默认值是*null*。 |
| **RequireSsl** | 一个布尔值，指示是否需要通过安全 SSL 通道提交的 ANTI-XSRF 令牌。 如果此值为*true*，任何自动生成的 cookie 将具有"安全"标志设置，并且从调用中时不会通过 SSL 提交的请求，则会引发 ANTI-XSRF Api。 默认值为“false”。 |
| **SuppressIdentityHeuristicChecks** | 一个布尔值，指示是否 ANTI-XSRF 系统应停用它对基于声明的标识的支持。 如果此值为*true*，系统将假定*IIdentity.Name*适合作为唯一的每个用户标识符的使用和不会尝试向特殊用例*IClaimsIdentity*或*ClClaimsIdentity*中所述[WIF / ACS / 基于声明的身份验证](#_WIF_ACS)部分。 默认值为 `false`。 |
| **UniqueClaimTypeIdentifier** | 一个字符串，指示哪些声明类型是适用于作为唯一的每个用户标识符。 如果此值是组和当前*IIdentity*基于声明的则系统将尝试提取一个声明的类型由指定*UniqueClaimTypeIdentifier*，并且将使用相应的值代替生成字段标记时的用户的用户名。 如果未找到的声明类型，系统将无法发送请求。 默认值是*null*，指示系统应使用 （标识提供程序，名称标识符） 代替用户的用户名如上文所述的元组。 |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* 类型允许开发人员通过往返中每个令牌的其他数据扩展的 ANTI-XSRF 系统的行为。 *GetAdditionalData*每次调用方法生成的字段标记，和在生成的标记内嵌入的返回值。 实施者无法通过此方法返回时间戳、 一个 nonce 或她希望的任何其他值。

同样， *ValidateAdditionalData*每次调用方法将验证字段标记，并且已在令牌内嵌入的"其他数据"字符串传递给方法。 验证例程无法实现超时 （通过检查当前时间，但创建令牌时存储的时间）、 nonce 检查例程，或任何其他所需的逻辑。

## <a name="design-decisions-and-security-considerations"></a>设计决策和安全注意事项

链接的会话和字段令牌的安全令牌从技术上讲时才需要尝试保护免受 XSRF 攻击匿名 / 未经身份验证的用户。 当用户进行身份验证时，可使用的身份验证令牌本身 （大概 cookie 形式提交） 作为一个一半同步器令牌对。 但是，有保护命中由未经身份验证的用户的登录页的有效方案和 ANTI-XSRF 逻辑进行更简单始终生成和验证安全令牌，即使对于经过身份验证的用户。 中，字段标记曾经受到攻击，作为设置或猜测会话令牌攻击者能够克服的另一个障碍，它还未提供一些额外的保护。

当多个应用程序承载在单个域中时，开发人员应小心。 例如，即使*example1.cloudapp.net*和*example2.cloudapp.net*是不同的主机下的所有主机之间没有隐式信任关系 *\*。 cloudapp.net*域。 此隐式信任关系[让影响对方的 cookie 可能不受信任的主机](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks)（控制 AJAX 请求的同源策略不一定适用于 HTTP cookie）。 ASP.NET Web 堆栈运行时，用户名将嵌入到字段令牌中，因此即使恶意的子域是能够覆盖会话令牌将无法生成有效字段标记以该用户提供一些缓解。 但是，在这种环境中承载时的内置的 ANTI-XSRF 例程仍不能抵御会话劫持或登录名 XSRF。

ANTI-XSRF 例程当前是否不抵御[clickjacking](https://www.owasp.org/index.php/Clickjacking)。 想要针对 clickjacking 保护他们自己的应用程序可以轻松完成此操作发送 X 框架选项： SAMEORIGIN 与每个响应的标头。 所有新的浏览器都支持此标头。 有关详细信息，请参阅[IE 博客](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx)、 [SDL 博客](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)，和[OWASP](https://www.owasp.org/index.php/Clickjacking)。 ASP.NET Web 堆栈运行时可在一些将来的版本进行 MVC 和 Web 页的 ANTI-XSRF 帮助器自动设置此标头，以便应用程序已自动受到保护针对这种攻击。

Web 开发人员应继续以确保它们的站点不是很容易受到 XSS 攻击。 XSS 攻击是非常强大，并成功利用此漏洞也将会破坏 ASP.NET Web 堆栈运行时防御 XSRF 攻击。

## <a name="acknowledgment"></a>确认

[@LeviBroderick](https://twitter.com/LeviBroderick)谁写入 ASP.NET 安全代码大部分此信息的大容量。
