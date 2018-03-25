---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: "窗体身份验证配置和高级的主题 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将检查各种窗体身份验证设置，并请参阅如何通过窗体元素对其进行修改。 这将需要详细..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: c57722965b510ac4f5cf0c06c7c01c8cea26384f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="forms-authentication-configuration-and-advanced-topics-c"></a>窗体身份验证配置和高级的主题 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip)或[下载 PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> 在本教程中我们将检查各种窗体身份验证设置，并请参阅如何通过窗体元素对其进行修改。 例如，这将需要了解自定义窗体身份验证票证的超时值，使用 （如 SignIn.aspx 而不是 Login.aspx)，自定义 URL 和 cookieless 窗体身份验证票证的登录页的详细的信息。


## <a name="introduction"></a>介绍

在[以前一教程](an-overview-of-forms-authentication-cs.md)我们看了用于实现 ASP.NET 应用程序中，从指定配置设置在 Web.config 中的显示在页中创建日志不同的窗体身份验证所必需的步骤经过身份验证和匿名用户的内容。 回想一下，我们配置网站以使用窗体身份验证设置的模式特性&lt;身份验证&gt;向窗体的元素。 &lt;身份验证&gt;元素可以根据需要包含&lt;窗体&gt;子元素，可通过该指定多种类型的窗体身份验证设置。

在本教程中我们将检查各种窗体身份验证设置，并了解如何以修改通过&lt;窗体&gt;元素。 例如，这将需要了解自定义窗体身份验证票证的超时值，使用 （如 SignIn.aspx 而不是 Login.aspx)，自定义 URL 和 cookieless 窗体身份验证票证的登录页的详细的信息。 我们将更紧密地检查窗体身份验证票证的构成，并请参阅 ASP.NET 所需确保票证的数据从检查和篡改安全预防措施。 最后，我们将了解如何将额外的用户数据存储在窗体身份验证票证和如何通过自定义主体对象的此数据创建模型。

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>第 1 步： 检查&lt;窗体&gt;配置设置

ASP.NET 中的窗体身份验证系统提供的多种可以基于应用程序的应用程序自定义的配置设置。 这包括设置，例如： 窗体身份验证的生存期票证;哪种保护应用于票证;在哪些条件 cookieless 身份验证使用票证;到登录页中; 路径和其他信息。 若要修改的默认值，将添加[&lt;窗体&gt;元素](https://msdn.microsoft.com/library/1d3t3c61.aspx)的子级[&lt;身份验证&gt;元素](https://msdn.microsoft.com/library/532aee0e.aspx)，指定这些属性你想要自定义为 XML 属性的值如下所示：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

表 1 总结了可以通过自定义的属性&lt;窗体&gt;元素。 Web.config 是一个 XML 文件，因为左侧列中的属性名称是区分大小写。

| **特性** | **说明** |
| --- | --- |
| 无 cookie | 此属性指定在什么条件下的身份验证票证存储在与被嵌入在 URL 中的 cookie。 允许的值为： UseCookies;UseUri;自动检测;和 UseDeviceProfile （默认值）。 步骤 2 检查此设置在更多详细信息。 |
| defaultUrl | 指示用户将重定向到在登录后从登录页如果没有 RedirectUrl 值在查询字符串中指定的 URL。 默认值为 default.aspx。 |
| 域 | 当使用基于 cookie 的身份验证票证，则此设置指定 cookie 的域值。 默认值为空字符串，这会导致浏览器使用从该情况下，它已签发 （如 www.yourdomain.com) 的域。 在这种情况下，cookie 将**不**时进行请求定向到子域，例如 admin.yourdomain.com 发送。如果你想要传递给需要自定义将其设置为进行域属性的所有子域的 cookie。 |
| enableCrossAppRedirects | 一个布尔值，该值指示是否已经过身份验证的用户要记住过的同一服务器上其他 web 应用程序中定向到 Url 时。 默认值为 false。 |
| loginUrl | 登录页的 URL。 默认值为 login.aspx。 |
| name | 当使用基于 cookie 的身份验证票证的 cookie 的名称。 默认值为。ASPXAUTH。 |
| path | 当使用基于 cookie 的身份验证票证，则此设置指定 cookie 的路径属性。 Path 属性使开发人员可以限制到特定的目录层次结构的 cookie 的作用域。 默认值是，/，它通知浏览器将身份验证票证 cookie 发送到对域进行任何请求。 |
| 保护 | 指示哪些技术用于保护窗体身份验证票证。 允许的值包括： 所有 （默认）;加密;None;和验证。 在步骤 3 中详细讨论了这些设置。 |
| requireSSL | 一个布尔值，该值指示是否需要 SSL 连接来传输身份验证 cookie。 默认值为 False。 |
| slidingExpiration | 一个布尔值，该值指示用户是否每次重置身份验证 cookie 超时在单个会话期间访问该站点。 默认值为 true。 在指定的更详细地讨论身份验证票证超时策略的票证超时值部分。 |
| 超时 | 指定时间，以分钟为单位，身份验证票证 cookie 过期。 默认值为 30。 在指定的更详细地讨论身份验证票证超时策略的票证超时值部分。 |

**表 1**: 摘要&lt;窗体&gt;元素的属性

在 ASP.NET 2.0 和到超过默认窗体身份验证值将是硬编码在.NET Framework 中的 FormsAuthenticationConfiguration 类。 必须在 Web.config 文件中的应用程序的应用程序基于应用任何修改。 这不同于 ASP.NET 1.x，其中默认窗体身份验证值在 machine.config 文件中存储 （并因此无法通过编辑 machine.config 修改）。 有关 ASP.NET 的主题的段 1.x，因此有必要提及大量的窗体身份验证系统设置在 ASP.NET 2.0 中具有不同的默认值和比在 ASP.NET 中的扩展 1.x。 如果要从 ASP.NET 1.x 环境迁移你的应用程序，务必要注意的这些差异。 请查阅[&lt;窗体&gt;元素技术文档](https://msdn.microsoft.com/library/1d3t3c61.aspx)有关的差异的列表。

> [!NOTE]
> 多个窗体身份验证设置，例如超时、 域和路径，指定生成的窗体身份验证票证 cookie 的详细信息。 在 cookie、 它们的工作原理和它们的各种属性的详细信息，请阅读[此 Cookie 教程](http://www.quirksmode.org/js/cookies.html)。


### <a name="specifying-the-tickets-timeout-value"></a>指定的票证超时值

窗体身份验证票证是表示标识令牌。 使用的基于 cookie 的身份验证票证，此标记是 cookie 的形式保存，并且发送到 web 服务器上每个请求。 拥有的令牌中，从根本上而言，声明，我*用户名*，我已登录，并使用，以便可以跨网页访问记住用户的标识。

窗体身份验证票证不仅包括用户的标识，也包含可帮助确保完整性和安全令牌的信息。 毕竟，我们不希望恶意用户能够创建盗版软件令牌，或以某种 underhanded 方式修改正当令牌。

此类包括票证中的信息的一个位是*到期*，即的日期和时间票证将不再有效。 每当 FormsAuthenticationModule 检查身份验证票证，它确保不具有尚未传递票证到期。 如果已更改，它将忽略该票证，并标识为匿名用户。 这种保护措施可帮助防止重播攻击。 而无需一个过期时间，如果黑客已能够通过物理访问其计算机并通过其 cookie 定位可能获得用户的有效的身份验证票证的她动手-它们无法将请求发送到此被盗的身份验证票证的服务器和获取项。 虽然到期不会阻止这种情况下，不会限制在此期间可以成功进行此类攻击的窗口。

> [!NOTE]
> 步骤 3 窗体身份验证系统用于保护身份验证票证的详细信息其他技术。


在创建时的身份验证票证，窗体身份验证系统将确定其到期通过参考超时设置。 表 1，设置默认值为 30 分钟的超时值中所述这意味着，窗体身份验证票证创建时其到期设置为日期和时间在将来 30 分钟。

到期定义绝对将来窗体身份验证票证的到期的时间。 但开发人员通常要实现滑动到期，一个每次用户将回顾站点重置。 此行为由 slidingExpiration 设置确定。 如果设置为 true （默认值），的每当 FormsAuthenticationModule 对用户进行身份验证，它将更新的票证到期。 如果设置为 false，过期不针对每个请求更新，从而导致票证过期完全超时分钟数票证最初创建。

> [!NOTE]
> 存储在身份验证票证的到期时间是绝对日期和时间值，如 2008 年 8 月 2 日上午 11:34。 此外的日期和时间是相对于 web 服务器的本地时间。 这种设计决策可能会有一些有趣的副作用围绕夏令时 (DST)，这是在美国国内的时钟向前移动一小时 （假定 web 服务器托管在其中观察到夏时制的区域） 时。 请考虑为 ASP.NET 网站且 DST 开始时间附近 30 分钟过期会发生 （这是在凌晨 2:00）。 假设访问者登录到站点在 2008 年 3 月 11 日凌晨 1:55。 这将生成将在到期日期： 2008 年 3 月 11 日上午 2:25 （将来 30 分钟） 的窗体身份验证票证。 但是，一旦围绕汇总凌晨 2:00，时钟跳转到 3:00 AM 由于 DST。 当用户加载新页 （在上午 3:01） 登录后超过 6 分钟时，FormsAuthenticationModule 说明票证已过期，将用户重定向到登录页。 有关此和其他身份验证票证超时问题，以及解决方法的更全面讨论，选取一份 Stefan Schackow *Professional ASP.NET 2.0 安全性、 成员资格，以及角色管理*(ISBN:978-0-7645-9698-8)。


图 1 说明工作流时 slidingExpiration 设置为 false，超时设置为 30。 请注意，在登录生成的身份验证票证包含到期日期，并在后续请求将不更新此值。 如果 FormsAuthenticationModule 找到此票证已到期，它将丢弃它，并将视为匿名请求。


[![窗体身份验证票证到期时 slidingExpiration 图形表示形式为 false](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**图 01**: 图形表示形式窗体身份验证票证到期时 slidingExpiration 为 false ([单击以查看实际尺寸的图像](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))


图 2 显示了工作流时 slidingExpiration 设置为 true 和超时设置为 30。 （使用未过期的票证） 收到经过身份验证的请求时其到期更新为在将来分钟的超时数。


[![窗体身份验证票证的图形表示形式时 slidingExpiration 为 true](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**图 02**: 的窗体身份验证票证的图形表示形式时 slidingExpiration 为 true ([单击以查看实际尺寸的图像](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))


在使用基于 cookie 的身份验证票证 （默认值） 时，此讨论将变得有点令人困惑，因为 cookie 还可以指定自己满。 Cookie 的到期 （或缺少） 会指示浏览器应销毁该 cookie 时。 如果 cookie 缺少到期，当浏览器关闭时，它是可以被销毁。 如果存在到期时间，但是，cookie 日期之前保持存储在用户的计算机上，并且已通过指定在到期时间。 Cookie 销毁时由浏览器，它不再发送到 web 服务器。 因此，cookie 析构相当于从站点日志记录的用户。

> [!NOTE]
> 当然，用户可以主动删除存储在其计算机上任何 cookie。 在 Internet Explorer 7，你将转到工具，选项，并单击浏览历史记录部分中的删除按钮。 在这里，单击删除 cookie 按钮。


窗体身份验证系统创建基于会话的还是基于过期的 cookie，具体取决于传递给值*persistCookie*参数。 回想一下，FormsAuthentication 类 GetAuthCookie、 SetAuthCookie 和 RedirectFromLoginPage 方法接受两个输入参数：*用户名*和*persistCookie*。 我们在前面的教程中创建的登录页包含一个记住我的复选框，确定是否已创建的持久性 cookie。 永久 cookie 是基于过期的;非持久性 cookie 是基于会话的。

已所述的超时和 slidingExpiration 概念适用于这两个基于会话和过期的 cookie。 没有在执行中只有一个细微的差别： 在使用基于过期的 cookie，slidingTimeout 设置为 true 时，cookie 的到期才会更新时超过一半的指定的时间已过。

让我们更新我们网站的身份验证票证超时策略，因此票证超时后一小时 （60 分钟），使用滑动过期。 若要此更改生效，请更新 Web.config 文件中，添加&lt;窗体&gt;元素&lt;身份验证&gt;元素替换为以下标记：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>使用以外 Login.aspx 登录页 URL

由于 FormsAuthenticationModule 自动重定向到登录页的未经授权的用户，它需要知道登录页的 URL。 通过中的登录 Url 属性指定此 URL&lt;窗体&gt;元素，默认值为 login.aspx。 如果要通过现有网站将迁移，你可能已使用其他 URL，另一个用于已标有书签和按搜索引擎编制索引的登录页。 而不是正在对现有的登录页重命名为 login.aspx 和重大链接和用户的书签，而是可以修改登录 Url 属性以指向您的登录页。

例如，如果您的登录页名为 SignIn.aspx 并且位于用户目录中，你无法点到 ~/Users/SignIn.aspx 的登录 Url 配置设置如下所示：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

由于我们的当前应用程序已具有一个名为 Login.aspx 的登录页，因此时无需指定中的自定义值&lt;窗体&gt;元素。

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>步骤 2： 使用 Cookieless 窗体身份验证票证

默认情况下窗体身份验证系统确定是否在 cookie 集合中存储其身份验证票证或将其嵌入在基于用户代理访问网站的 URL。 所有主流桌面浏览器类似 Internet Explorer、 Firefox、 Opera 和 Safari，支持 cookie，但并非所有移动设备执行。

使用窗体身份验证系统的 cookie 策略取决于中的无 cookie 设置&lt;窗体&gt;元素，它可以分配四个值之一：

- UseCookies-指定将始终使用基于 cookie 的身份验证票证。
- UseUri-指示，将永远不会使用基于 cookie 的身份验证票证。
- 不使用自动检测-如果设备配置文件不支持 cookie，基于 cookie 的身份验证票证;如果设备配置文件支持 cookie，探测机制用于确定是否已启用 cookie。
- UseDeviceProfile 的默认设置;仅当设备配置文件支持 cookie，请使用基于 cookie 的身份验证票证。 使用没有探测机制。

自动检测和 UseDeviceProfile 设置依赖于*设备配置文件*中有助于确定是否使用基于 cookie 的或无 cookie 身份验证票证。 ASP.NET 维护的各种设备和其功能，如它们是否支持 cookie，它们支持的 JavaScript 和等等的哪个版本的数据库。 每次设备请求 web 页从 web 服务器发送沿*用户代理*标识的设备类型的 HTTP 标头。 ASP.NET 自动指定在其数据库中的相应配置文件与匹配提供的用户代理字符串。

> [!NOTE]
> 设备功能的此数据库存储在多个 XML 文件符合[浏览器定义文件架构](https://msdn.microsoft.com/library/ms228122.aspx)。 默认设备配置文件位于 %windir%\microsoft.net\framework\v2.0.50727\config\browsers。 此外可以向应用程序的应用程序添加自定义文件\_浏览器文件夹。 有关详细信息，请参阅[How To： 检测浏览器类型中的 ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx)。


由于此默认设置为 UseDeviceProfile，通过其配置文件报告它不支持 cookie 的设备访问该站点时，将会用无 cookie 窗体身份验证票证。

### <a name="encoding-the-authentication-ticket-in-the-url"></a>编码的 URL 中的身份验证票证

Cookie 是一个用于将从浏览器的信息包含在特定网站，这是默认窗体身份验证设置使用 cookie，如果正在访问的设备支持它们的原因的每个请求的自然介质。 如果不支持 cookie，则必须采用，将从客户端的身份验证票证传递给服务器的备用方法。 在无 cookie 环境中使用的常见解决方法是编码的 URL 中的 cookie 数据。

请参阅如何在 URL 中嵌入此类信息的最佳方法是以强制站点以使用无 cookie 的身份验证票证。 可以将无 cookie 的配置设置设置为 UseUri 完成此操作：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

你一次进行此更改，请访问通过浏览器的站点。 当来访作为匿名用户，它们像以前一样将查找 Url。 例如，在访问 Default.aspx 页时我的浏览器地址栏将显示以下 URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

但是，在记录，窗体身份验证票证嵌入到 URL。 例如，在访问登录页和 Sam 的身份登录之后, 我就会返回到 Default.aspx 页上，但的 URL 这一次：

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

已在 URL 中嵌入的窗体身份验证票证。 字符串 (F (jaIOIDTJxIr12xYS VVgkqKCVAuIoW30Bu0diWi6flQC FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) 表示十六进制编码的身份验证票证信息，且它是相同的数据通常存储在一个 cookie。

为了使无 cookie 的身份验证票证工作，系统必须对所有 Url 的页上，以包括身份验证票证数据进行都编码，否则身份验证票证将当用户单击链接上丢失。 幸运的是，此嵌入逻辑自动执行。 为了演示此功能，打开 Default.aspx 页上，并添加超链接控件，分别将其文本和 NavigateUrl 属性设置为测试链接和 SomePage.aspx。 它并不重要，实际上没有页面名为 SomePage.aspx 项目中。

将所做的更改保存到 Default.aspx，然后通过浏览器中访问它。 以便在 URL 中嵌入的窗体身份验证票证登录到站点。 接下来，从 Default.aspx 中，单击测试链接链接。 这是怎么回事？ 如果不存在名为 SomePage.aspx 任何页，则表示出现 404 错误，但这是不重要的一点此处。 相反，专注于在你的浏览器的地址栏。 请注意它在 URL 中包含窗体身份验证票证 ！

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

链接中 URL SomePage.aspx 已自动转换为一个 URL，它包含的身份验证票证-我们不必编写缺乏对代码 ！ 窗体身份验证票证将自动在不是以 http:// 开头的任何超链接的 URL 中嵌入或 /。 如果在调用 Response.Redirect、 超链接控件，或定位 HTML 元素出现的超链接并不重要 (即， &lt;href ="..."&gt;...&lt;/a&gt;)。 只要 URL 不是如 http://www.someserver.com/SomePage.aspx 或 /SomePage.aspx，将为我们嵌入窗体身份验证票证。

> [!NOTE]
> 无 cookie 窗体身份验证票证遵循相同的超时策略作为基于 cookie 的身份验证票证。 但是，无 cookie 的身份验证票证是更倾向于重播攻击，因为直接在 URL 中嵌入的身份验证票证。 假设访问网站，登录，然后将 URL 粘贴到同事的电子邮件中的用户。 如果该同事会在达到到期之前单击该链接，它们将作为其发送电子邮件的用户记录 ！


## <a name="step-3-securing-the-authentication-ticket"></a>步骤 3： 保护身份验证票证

通过网络传输的窗体身份验证票证在 cookie 或直接在 URL 内嵌入。 标识信息，除了身份验证票证还可以包含用户数据，（正如我们将在步骤 4 中看到的）。 因此，很重要的票证的数据进行加密从有人和 （更重要的是） 的窗体身份验证系统可以保证票证被未篡改。

若要确保的票证的数据的隐私，窗体身份验证系统可以对票证数据进行加密。 失败的票证数据进行加密通过网络在纯文本中发送可能敏感的信息。

若要确保票证的真实性，窗体身份验证系统必须*验证*票证。 验证是一种确保数据的特定部分未进行修改，并通过来完成行为*[消息身份验证代码 (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*。 简而言之，MAC 是信息的标识需要验证 （在此情况下，该票证） 的数据的一小部分。 如果修改 MAC 所表示的数据，然后 MAC 和数据将不匹配。 此外，它是计算硬黑客同时修改的数据并生成自己的 MAC，以与已修改的数据一致。

创建 （或修改） 时票证，窗体身份验证系统将创建 MAC，并将其附加到票证的数据。 在后续请求到达时，窗体身份验证系统比较 MAC 和票证数据，以验证票证数据的真实性。 图 3 说明了此工作流以图形方式。


[![票证的真实性确保通过 MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**图 03**: 票证的真实性确保通过 MAC ([单击以查看实际尺寸的图像](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))


哪些安全措施应用于身份验证票证取决于中的保护设置&lt;窗体&gt;元素。 可以用的保护设置分配给以下三个值之一：

- 所有的票证已加密，并进行数字签名 （默认值）。
- 加密-仅加密已应用-生成没有 MAC。
- None-票证是加密和数字签名。
- 生成验证-MAC，但通过网络在纯文本中发送的票证数据。

Microsoft 强烈建议使用的所有设置。

### <a name="setting-the-validation-and-decryption-keys"></a>设置验证和解密密钥

加密和哈希算法的窗体身份验证系统中用于加密和验证身份验证票证是通过可自定义[ &lt;machineKey&gt;元素](https://msdn.microsoft.com/library/w8h3skw9.aspx)在 Web.config 中。表 2 概述了&lt;machineKey&gt;元素的特性和其可能的值。

| **特性** | **说明** |
| --- | --- |
| 解密 | 指示用于加密的算法。 此属性可以具有以下四个值之一:-自动-默认设置;确定基于 decryptionKey 属性的长度的算法。 -AES-使用[高级加密标准 (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard)算法。 DES-使用[数据加密标准 (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard)此算法属于计算弱，不应使用。 -3DES-使用[三重 DES](http://en.wikipedia.org/wiki/Triple_DES)算法，可通过应用三次的 DES 算法配合工作。 |
| decryptionKey | 加密算法使用的密钥。 此值必须是相应的长度 （基于解密中的值）、 自动生成或附加，其中任何一个值的十六进制字符串 IsolateApps。 添加 IsolateApps 指示 ASP.NET 为每个应用程序使用的唯一值。 默认值为 AutoGenerate，IsolateApps。 |
| 验证 | 指示用于验证的算法。 此属性可以具有以下四个值之一:-AES-使用高级加密标准 (AES) 算法。 -MD5-使用[消息摘要 5 (MD5)](http://en.wikipedia.org/wiki/MD5)算法。 -SHA1-使用[SHA1](http://en.wikipedia.org/wiki/Sha1)算法 （默认值）。 -3DES-使用三重 DES 算法。 |
| validationKey | 使用的验证算法密钥。 此值必须是相应的长度 （基于验证中的值）、 自动生成或附加，其中任何一个值的十六进制字符串 IsolateApps。 添加 IsolateApps 指示 ASP.NET 为每个应用程序使用的唯一值。 默认值为 AutoGenerate，IsolateApps。 |

**表 2**: &lt;machineKey&gt;元素特性

这些加密和验证的选项，和专业人员的全面讨论和缺点的各种算法，不在本教程的范围。 有关深入查看这些问题，包括有关哪些加密和验证算法，若要使用，指导哪些密钥的长度，若要使用，以及如何最好地生成这些密钥，请参阅*Professional ASP.NET 2.0 安全性、 成员资格，以及角色管理*.

默认情况下，对于每个应用程序，会自动生成用于加密和验证的密钥和这些键存储在本地安全机构 (LSA)。 简单地说，默认设置保证 web 服务器的 web 服务器和应用程序的应用程序的基础上的唯一键。 因此，此默认行为将不能用于以下两种情况：

- **Web 场**-在[web 场](http://en.wikipedia.org/wiki/Web_farm)出于可伸缩性和冗余的多个 web 服务器上托管方案中，一个 web 应用程序。 每个传入请求调度到的服务器场，这意味着，通过用户的会话的生存期内，不同的服务器可用于处理其各种请求中。 因此，每个服务器必须使用相同的加密和验证密钥，以便窗体身份验证票证创建、 加密和验证上一台服务器可以解密和验证场中不同的服务器上。
- **跨应用程序票证共享**-单个 web 服务器可以托管多个 ASP.NET 应用程序。 如果你需要有关这些不同应用程序共享单个窗体身份验证票证，则务必，他们加密和验证的密钥匹配。

在处理在 web 场中设置或在同一服务器上的应用程序之间共享身份验证票证时，你将需要配置&lt;machineKey&gt;受影响的应用程序中的元素，以便其 decryptionKey 和validationKey 值相匹配。

而上述方案都不适用于我们的示例应用程序，我们仍可以指定显式 decryptionKey 和 validationKey 值并定义要使用的算法。 添加&lt;machineKey&gt;将设置为 Web.config 文件：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

有关详细信息，请查看[How To： 在 ASP.NET 2.0 中配置 MachineKey](https://msdn.microsoft.com/library/ms998288.aspx)。

> [!NOTE]
> 从已执行的 decryptionKey 和 validationKey 值[Steve Gibson](http://www.grc.com/stevegibson.htm)的[完美密码网页](https://www.grc.com/passwords.htm)，这将在每个页访问生成 64 随机的十六进制字符。 若要减少到生产应用程序进行如何使用这些密钥的可能性，不过，建议您的上述密钥将替换为从完美密码页的随机生成的。


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>步骤 4： 票证中存储的其他用户数据

许多 web 应用程序显示有关的信息，或基于当前登录用户的页面的显示。 例如，web 页可能会显示用户的名称和她最后一次登录中每一页的左上角的日期。 窗体身份验证票证存储当前登录的用户的用户名，但当不需要任何其他信息，请页必须转到要查找不会存储在身份验证票证的信息的用户存储区-通常为数据库的。

很少的代码下，我们可以存储在窗体身份验证票证的其他用户信息。 此类数据可以通过表示[FormsAuthenticationTicket 类](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)的[UserData 属性](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx)。 这是信息的一个有用地方安置的少量有关的用户的通常所需。 中 UserData 属性过程中加入的身份验证票证 cookie，并像其他票证字段、 是加密和验证指定的值基于窗体身份验证系统的配置。 默认情况下，UserData 是空字符串。

为身份验证票证中存储用户数据，我们需要在登录页上获取特定于用户的信息并将它存储在票证编写的代码。 由于 UserData 是字符串类型的属性，必须正确将在其中存储的数据序列化为字符串。 例如，假设我们用户存储区包含每个用户的出生日期和其雇主的名称，并且我们要将这些两个属性值存储在身份验证票证。 我们无法序列化这些值为一个字符串串联的出生的字符串以竖线 (|) 的用户的日期跟雇主名称。 出生日期上 1974 年 8 月 15，用户，适用于 Northwind Traders，我们将 UserData 属性将字符串分配： 1974年-08-15 |Northwind Traders。

每当我们需要访问票证中存储的数据，我们可以通过抓取当前请求的 FormsAuthenticationTicket 和反序列化 UserData 属性来实现。 在出生和雇主名称示例的日期，我们要将 UserData 字符串拆分为两个多个子字符串基于分隔符 (|)。


[![其他用户信息可以存储在身份验证票证](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**图 04**： 其他用户信息可以存储在身份验证票证 ([单击以查看实际尺寸的图像](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))


### <a name="writing-information-to-userdata"></a>将信息写入 UserData

遗憾的是，将特定于用户的信息添加到窗体身份验证票证不像简单如您所料。 FormsAuthenticationTicket 类 UserData 属性是只读的并且只可以指定通过 FormsAuthenticationTicket 类构造函数。 在指定 UserData 属性构造函数中时，我们还需要提供票证的其他值： 用户名、 问题日期、 过期和等等。 在前面的教程，我们创建的登录页，这是所有为我们 FormsAuthentication 类处理。 添加到 FormsAuthenticationTicket UserData，我们将需要编写代码来复制大部分 FormsAuthentication 类已提供的功能。

让我们了解使用 UserData 通过更新 Login.aspx 页后，可以记录有关用户身份验证票证的其他信息的必要代码。 假设我们用户存储包含有关用户适用于公司和其标题信息，我们想要的身份验证票证中获取此信息。 更新 Login.aspx 页面 LoginButton Click 事件处理程序，以便的代码看起来如下所示：

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

让我们逐步完成此一行代码一次。 通过定义四个字符串数组的方法启动： 用户、 密码、 companyName 和 titleAtCompany。 这些数组是否包含用户名、 密码、 公司名称和标题的用户帐户在系统中的这三种： Scott、 Jisun 和 Sam。 在实际应用中，将从用户存储中，没有硬编码页面的源代码中查询这些值。

在前面的教程，如果提供的凭据有效我们只需调用 FormsAuthentication.RedirectFromLoginPage （UserName.Text、 RememberMe.Checked） 执行以下步骤：

1. 创建窗体身份验证票证
2. 已票证写入到合适的存储区。 对于基于 cookie 的身份验证票证，则使用浏览器的 cookie 的集合;为无 cookie 的身份验证票证的票证数据序列化到 URL
3. 重定向到相应的页面的用户

在上面的代码中复制这些步骤。 首先，我们将最终存储 UserData 属性中的字符串是通过合并的公司名称和标题，分隔两个值的竖线 (|) 形成的。

string userDataString = string.Concat(companyName[i], "|", titleAtCompany[i]);

接下来，调用方法时，这将创建身份验证票证，FormsAuthentication.GetAuthCookie 加密和验证根据配置设置中，并将其放在 HttpCookie 对象。

HttpCookie authCookie = FormsAuthentication.GetAuthCookie(UserName.Text, RememberMe.Checked);

若要使用嵌入 cookie FormAuthenticationTicket，我们需要先调用 FormAuthentication 类[解密方法](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)，并传递在 cookie 值中。

FormsAuthenticationTicket ticket = FormsAuthentication.Decrypt(authCookie.Value);

然后，我们创建*新*FormsAuthenticationTicket 实例基于现有 FormsAuthenticationTicket 的值。 但是，此新票证包括特定于用户的信息 (userDataString)。

FormsAuthenticationTicket newTicket = new FormsAuthenticationTicket(ticket.Version, ticket.Name, ticket.IssueDate, ticket.Expiration, ticket.IsPersistent, userDataString);

我们然后加密 （和验证） 通过调用新的 FormsAuthenticationTicket 实例[加密方法](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)，并将其置于 authCookie 返回此加密 （和验证） 的数据。

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

最后，authCookie 添加到 Response.Cookies 集合和 GetRedirectUrl 方法调用来确定相应页后，可以向用户发送。

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

所有此代码被必须的因为 UserData 属性是只读的和 FormsAuthentication 类不提供任何方法来指定其 GetAuthCookie、 SetAuthCookie 或 RedirectFromLoginPage 方法中的用户数据信息。

> [!NOTE]
> 我们只需检查的代码将存储在基于 cookie 的身份验证票证的特定于用户的信息。 负责序列化到的 URL 的窗体身份验证票证的类是.NET Framework 的内部。 长情景短，不能在无 cookie 窗体身份验证票证来存储用户数据。


### <a name="accessing-the-userdata-information"></a>访问用户数据信息

此时每个用户的公司名称和标题存储在窗体身份验证票证 UserData 属性在登录时。 而无需用户存储到的行程，可以从任何页面上的身份验证票证访问此信息。 为了说明如何从 UserData 属性检索此信息，让我们更新 Default.aspx，以使其欢迎消息包括不仅用户的名称，但它们适用于公司，而且其标题。

目前，Default.aspx 将 AuthenticatedMessagePanel 面板包含一个名为 WelcomeBackMessage 的标签控件。 向经过身份验证的用户仅显示此面板。 更新 Default.aspx 的页中的代码\_加载事件处理程序，以便其类似于下面所示：

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

如果 Request.IsAuthenticated 为 true，则 WelcomeBackMessage 的 Text 属性首先设置为欢迎回来，*用户名*。 然后，User.Identity 属性被强制转换为 FormsIdentity 对象，以便我们可以访问基础 FormsAuthenticationTicket。 一旦我们有 FormsAuthenticationTicket，我们反 UserData 属性序列化为的公司名称和标题。 这被通过拆分的管道字符的字符串。 公司名称和标题将 WelcomeBackMessage 标签中显示。

图 5 所示为正在运行的此显示屏幕快照。 作为 Scott 日志记录显示一条欢迎后消息，其中包括 Scott 的公司和标题。


[![显示当前登录的用户的公司和标题](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**图 05**： 显示当前登录的用户的公司和标题 ([单击以查看实际尺寸的图像](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))


> [!NOTE]
> 身份验证票证 UserData 属性用作缓存中，以便用户存储区。 像任何缓存一样，它需要时修改基础数据进行更新。 例如，如果没有用户可以从中更新其配置文件的网页，必须刷新缓存 UserData 属性中的字段以反映用户所做的更改。


## <a name="step-5-using-a-custom-principal"></a>步骤 5： 使用自定义主体

在每个传入请求 FormsAuthenticationModule 尝试验证用户身份。 如果存在未过期的身份验证票证，则 FormsAuthenticationModule 就会将 HttpContext.User 属性分配给新的 GenericPrincipal 对象中。 此 GenericPrincipal 对象具有类型 FormsIdentity，包括对窗体身份验证票证的引用的标识。 GenericPrincipal 类包含所需的一个类以实现 IPrincipal 的裸机最少功能-它只需具有 Identity 属性和 IsInRole 方法。

主体对象具有以下两项职责： 指示哪些用户所属的角色，以便标识信息。 这通过 IPrincipal 接口的 IsInRole (*roleName*) 方法和标识属性，分别。 GenericPrincipal 类允许的角色名称的字符串数组以通过其构造函数; 指定其 IsInRole (*roleName*) 方法只检查是否已传递的中*roleName*字符串数组中存在。 当 FormsAuthenticationModule 创建 GenericPrincipal 时，它将数组中传递空字符串到 GenericPrincipal 的构造函数。 因此，对 IsInRole 的任何调用将始终返回 false。

GenericPrincipal 类满足大多数不使用角色的窗体基于身份验证方案的需求。 这种情况下，默认角色处理不足或者当你需要与用户相关联的自定义的 IIdentity 对象，可以在身份验证工作流过程中创建自定义的 IPrincipal 对象并将其分配给 HttpContext.User 属性。

> [!NOTE]
> 正如我们将在将来看到教程中，当 ASP。已启用 NET 的角色框架创建类型的自定义主体对象[RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx)并覆盖窗体身份验证创建 GenericPrincipal 对象。 若要定制该主体的 IsInRole 方法，以便与角色框架 API 做到这一点。


由于我们具有不关心自己的角色尚未，我们将具有此时创建自定义主体的唯一原因是关联到主体的自定义 IIdentity 对象。 在步骤 4 中我们讨论在将其他用户信息存储在身份验证票证 UserData 属性，具体而言，用户的公司名称和其标题。 但是，UserData 信息只是将可通过身份验证票证访问，然后仅为序列化字符串，这意味着每当我们想要查看用户信息存储在票证我们需要分析 UserData 属性。

我们可以通过创建一个类，实现 IIdentity 包括 CompanyName 和标题属性来提高开发人员的体验。 这样一来，开发人员可以访问当前登录的用户的公司名称和标题直接通过 CompanyName 和 Title 属性，而不需要了解如何分析 UserData 属性。

### <a name="creating-the-custom-identity-and-principal-classes"></a>创建自定义标识和主体类

对于本教程，让我们将创建在应用程序中的自定义的主体和标识对象\_代码文件夹。 首先，通过添加应用\_代码到你的项目的文件夹-右键单击解决方案资源管理器中的项目名称，选择添加 ASP.NET 文件夹选项中，并选择应用\_代码。 应用程序\_代码文件夹是一个包含类文件特定于网站的特殊 ASP.NET 文件夹。

> [!NOTE]
> 应用程序\_管理通过网站项目模型项目时，应仅使用代码文件夹。 如果你使用[Web 应用程序项目模型](https://msdn.microsoft.com/asp.net/Aa336618.aspx)，创建标准文件夹并将类添加到该。 例如，你无法添加新的文件夹名为类，并将你的代码置于其中。


接下来，将两个新类文件添加到应用程序\_代码文件夹，一个命名的 CustomIdentity.cs，另一个名为 CustomPrincipal.cs。


[![将 CustomIdentity 和 CustomPrincipal 类添加到你的项目](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**图 06**： 将 CustomIdentity 和 CustomPrincipal 类添加到你的项目 ([单击以查看实际尺寸的图像](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))


CustomIdentity 类负责实现定义的 AuthenticationType、 IsAuthenticated 和名称的属性的 IIdentity 接口。 除了这些必需的属性，我们感兴趣公开基础窗体身份验证票证，以及适用于用户的公司名称和标题的属性。 下列代码输入到 CustomIdentity 类。

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

请注意，类包括 FormsAuthenticationTicket 成员变量 (\_票证) 并且必须通过构造函数提供此信息。 在返回的标识的名称，则会使用该票证数据其 UserData 属性进行分析，以返回 CompanyName 和标题属性的值。

接下来，创建 CustomPrincipal 类。 因为我们不关心角色此时，CustomPrincipal 类的构造函数接受仅 CustomIdentity 对象;其 IsInRole 方法始终返回 false。

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>将 CustomPrincipal 对象分配到传入请求的安全上下文

我们现在有扩展要包括 CompanyName 和标题属性的默认 IIdentity 规范的类，以及使用自定义标识的自定义主体类。 我们已准备好单步执行 ASP.NET 管道和将我们自定义主体对象分配到传入请求的安全上下文。

ASP.NET 管道获取传入请求，并处理它通过多个步骤。 在每个步骤中，将引发特定事件，并使开发人员能够利用 ASP.NET 管道并修改在其生命周期中的某些点请求。 FormsAuthenticationModule，例如，等待 ASP.NET 引发[AuthenticateRequest 事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)，此时它将检查身份验证票证的传入请求。 如果找到的验证票证，则是创建 GenericPrincipal 对象并将其分配给 HttpContext.User 属性。

AuthenticateRequest 事件之后，ASP.NET 管道引发[PostAuthenticateRequest 事件](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx)，后者是，我们可以将由的实例与 FormsAuthenticationModule 创建 GenericPrincipal 对象我们CustomPrincipal 对象。 图 7 显示了此工作流。


[![GenericPrincipal 替换为在 PostAuthenticationRequest 事件 CustomPrincipal](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**图 07**: GenericPrincipal 替换为在 PostAuthenticationRequest 事件 CustomPrincipal ([单击以查看实际尺寸的图像](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))


若要在响应 ASP.NET 管道事件中执行代码，我们可以在 Global.asax 中创建相应的事件处理程序，或创建我们自己 HTTP 模块。 对于本教程让我们在 Global.asax 中创建事件处理程序。 首先，通过添加 Global.asax 到你的网站。 右键单击解决方案资源管理器中的项目名称，然后添加一个名为 Global.asax 的全局应用程序类类型的项。


[![将 Global.asax 文件添加到你的网站](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**图 08**： 将 Global.asax 文件添加到您的网站 ([单击以查看实际尺寸的图像](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))


默认 Global.asax 模板包括事件处理程序的数目的 ASP.NET 管道事件，包括开始时，结束和[错误事件](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)，等等。 随意删除这些事件处理程序，因为我们不需要这些服务为此应用程序。 我们感兴趣的事件是 PostAuthenticateRequest。 更新你的 Global.asax 文件，使其标记看起来类似于以下：

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

应用程序\_OnPostAuthenticateRequest 方法执行 ASP.NET 运行时引发 PostAuthenticateRequest 事件，这种情况发生在每个传入的页请求上的一次每次。 事件处理程序首先检查用户进行身份验证，并且通过窗体身份验证进行身份验证。 如果是这样，新的 CustomIdentity 对象创建，并在其构造函数中传递的当前请求的身份验证票证。 接下来，CustomPrincipal 对象创建并在其构造函数中传递刚创建 CustomIdentity 对象。 最后，将当前请求的安全上下文被分配给新创建的 CustomPrincipal 对象。

请注意-将 CustomPrincipal 对象与请求的安全上下文关联的最后一步将主体分配到两个属性： HttpContext.User 和 Thread.CurrentPrincipal。 由于安全上下文会在 ASP.NET 中处理的方法，这些两个分配是必需的。 .NET Framework 将与每个正在运行的线程; 相关联的安全上下文此信息是可用作通过 IPrincipal 对象[线程对象](https://msdn.microsoft.com/library/system.threading.thread.aspx)的[CurrentPrincipal 属性](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx)。 什么是令人费解是 ASP.NET 有其自己的安全上下文信息 (HttpContext.User)。

在确定的安全上下文; 在某些情况下，检查 Thread.CurrentPrincipal 属性在其他情况下，将使用 HttpContext.User。 例如，.NET 中的安全功能，允许开发人员以声明方式状态决定哪些用户或角色可以实例化类或调用特定方法时 (请参阅[添加到业务和数据层使用的授权规则PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx))。 实际上，这些声明性方法确定通过 Thread.CurrentPrincipal 属性的安全上下文。

在其他情况下，使用 HttpContext.User 属性。 例如，在以前的教程，我们使用此属性以显示当前登录的用户的用户名。 很明显，然后，它是命令性，匹配的 Thread.CurrentPrincipal 和 HttpContext.User 属性中的安全上下文信息。

ASP.NET 运行时自动同步为我们的这些属性值。 但是，此同步在事件后发生 AuthenticateRequest，但*之前*PostAuthenticateRequest 事件。 因此，我们 PostAuthenticateRequest 事件中添加自定义主体时需要请务必将 Thread.CurrentPrincipal 或 else Thread.CurrentPrincipal 手动分配，HttpContext.User 将同步。请参阅[Context.User vs。Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/)有关此问题的更多详细讨论。

### <a name="accessing-the-companyname-and-title-properties"></a>访问 CompanyName 和标题属性

每当一个请求到达，且分派给 ASP.NET 引擎，应用程序\_OnPostAuthenticateRequest Global.asax 中的事件处理程序时，都会激发。 如果请求进行了成功身份验证 FormsAuthenticationModule，事件处理程序将创建一个新的 CustomPrincipal 对象与基于窗体身份验证票证的 CustomIdentity 对象。 通过使用就地此逻辑，访问有关当前登录的用户的公司名称和标题信息是非常简单。

返回到页\_在 Default.aspx 中，其中在步骤 4 中，我们编写了代码以检索窗体身份验证票证和分析 UserData 属性为了显示用户的公司名称和标题的 Load 事件处理程序。 在使用现在的 CustomPrincipal 和 CustomIdentity 对象，没有无需分析外的票证 UserData 属性的值。 相反，只需获取对 CustomIdentity 对象的引用，并使用其 CompanyName 和标题属性：

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>摘要

在本教程中，我们将探讨如何自定义通过 Web.config 的窗体身份验证系统的设置。我们介绍了如何处理的身份验证票证的到期日期，以及如何使用加密和验证安全措施来防止检查和修改的票证。 最后，我们讨论了使用身份验证票证 UserData 属性来存储本身，票证中的其他用户信息以及如何使用自定义的主体和标识对象来公开此信息更加便于开发人员的方式。

本教程到结束我们检查在 ASP.NET 中的窗体身份验证。 下一教程启动到成员资格框架我们之旅。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [将窗体身份验证](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [在 ASP.NET 2.0 中所述： 窗体身份验证](https://msdn.microsoft.com/library/aa480476.aspx)
- [如何： 保护 ASP.NET 2.0 中的窗体身份验证](https://msdn.microsoft.com/library/ms998310.aspx)
- [专业 ASP.NET 2.0 安全、 成员资格和角色管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [保护登录控件](https://msdn.microsoft.com/library/ms178346.aspx)
- [&lt;身份验证&gt;元素](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;窗体&gt;元素&lt;身份验证&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [&lt;MachineKey&gt;元素](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [了解的窗体身份验证票证和 Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教程中包含的主题的视频培训

- [如何更改窗体身份验证属性](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [如何在 ASP.NET 应用程序的设置和使用 Cookie 无身份验证](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [ASP 窗体登录重定位](../../../videos/authentication/asp-forms-login-relocation.md)
- [窗体登录自定义密钥配置](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [向身份验证方法添加自定义数据](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [使用自定义主体对象](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Alicja Maziarz。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com)。

>[!div class="step-by-step"]
[上一页](an-overview-of-forms-authentication-cs.md)
[下一页](security-basics-and-asp-net-support-vb.md)
