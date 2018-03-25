---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
title: "显示自定义错误页 (VB) |Microsoft 文档"
author: rick-anderson
description: "未用户看到的内容时出现运行时错误，则 ASP.NET web 应用程序中？ 答案取决于如何网站的&lt;customErrors&gt;配置..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 14873c5d-81a9-455b-bd71-30fb555583e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
msc.type: authoredcontent
ms.openlocfilehash: e8a2f88490de08f731f9737d15237ae445c5ec0d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="displaying-a-custom-error-page-vb"></a>显示自定义错误页 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_VB.zip)或[下载 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_vb.pdf)

> 未用户看到的内容时出现运行时错误，则 ASP.NET web 应用程序中？ 答案取决于如何网站的&lt;customErrors&gt;配置。 默认情况下，将显示用户屏幕的错误不美观黄色宣布发生运行时错误。 本教程演示如何自定义这些设置以匹配你的站点的外观和感觉的审美情趣显示自定义错误页。


## <a name="introduction"></a>介绍

在理想的环境中都将有任何运行时错误。 程序员应编写代码与 n 元 bug，并使用可靠的用户输入验证和外部资源，如数据库服务器和电子邮件服务器将永远不会转入脱机状态。 当然，实际上错误是不可避免的。 .NET Framework 中的类发出错误信号通过引发异常。 例如，调用 SqlConnection 对象的 Open 方法建立与连接字符串指定数据库的连接。 但是，如果数据库已关闭，或连接字符串中的凭据无效然后在 Open 方法将引发`SqlException`。 可以通过使用来处理异常`Try/Catch/Finally`块。 如果中的代码`Try`块引发了异常，控制将传递给相应的 catch 块开发人员可以尝试从错误中恢复。 如果没有匹配的 catch 块，或如果 try 块中不是引发异常的代码，该异常 percolates search 的调用堆栈中向上`Try/Catch/Finally`块。

如果异常冒泡一直到 ASP.NET 运行时不处理， [ `HttpApplication`类](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)的[`Error`事件](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)引发和配置*错误页*显示。 默认情况下，ASP.NET 才会显示一个错误页面，人们亲切地称为[黄色死亡屏幕](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)(YSOD)。 有两个版本的 YSOD： 一个显示异常详细信息，堆栈跟踪，以及其他信息有助于开发人员调试应用程序 (请参阅**图 1**); 另只是声明时运行时错误 （请参阅**图 2**)。

异常详细信息 YSOD 是非常有用对于开发人员调试应用程序，但向最终用户显示 YSOD 是黏性而且很不专业。 相反，最终用户应会转到维护站点的外观和感觉的更加友好的用户的文本说明这种情况的错误页。 好消息是创建这样的自定义错误页会相当简单。 本教程开头 ASP 一看。NET 的不同错误页。 然后，它演示如何配置 web 应用程序，以向用户显示在遇到错误时的自定义错误页。

### <a name="examining-the-three-types-of-error-pages"></a>检查错误页的三种类型

当未经处理的异常发生在 ASP.NET 应用程序的错误页的三种类型之一时将显示：

- 异常详细信息黄色屏幕的死亡错误页，
- 运行时错误黄色屏幕的死亡错误页，或
- 自定义错误页

错误页开发人员最熟悉与异常详细信息 YSOD。 默认情况下，此页显示的本地访问的用户，并且因此是在开发环境中测试该网站时出现错误时，将显示的页。 顾名思义，异常详细信息 YSOD 提供了详细描述异常的类型、 消息和堆栈跟踪。 更重要的是，如果异常由 ASP.NET 页的代码隐藏类中的代码和应用程序配置进行调试异常详细信息 YSOD 还将显示这行代码 （和上面的代码和它下面的几行）。

**图 1**显示异常详细信息 YSOD 页。 记下在浏览器的地址窗口中的 URL: `http://localhost:62275/Genre.aspx?ID=foo`。 回想一下，`Genre.aspx`页将列出某个特定流派的图书评论。 它要求`GenreId`值 ( `uniqueidentifier`) 先通过查询字符串; 例如，若要查看虚构评审相应 URL 是`Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`。 如果将非`uniqueidentifier`传递值在通过查询字符串 （如"foo") 引发异常。

> [!NOTE]
> 若要重现演示 web 应用程序中的出现此错误可供下载可任一访问`Genre.aspx?ID=foo`直接单击中的"生成运行时错误"链接或`Default.aspx`。


请注意中提供的异常信息**图 1**。 异常消息中，"转换失败将字符串转换为 uniqueidentifier 时"不存在页顶部。 异常类型、 `System.Data.SqlClient.SqlException`，已列出，以及。 此外，还有堆栈跟踪。

[![](displaying-a-custom-error-page-vb/_static/image2.png)](displaying-a-custom-error-page-vb/_static/image1.png)

**图 1**： 异常详细信息 YSOD 包括有关异常的信息  
 ([单击以查看实际尺寸的图像](displaying-a-custom-error-page-vb/_static/image3.png))

另一种 YSOD 是运行时错误 YSOD，和中所示**图 2**。 运行时错误 YSOD 通知发生了运行时错误，访问者，但它不包括有关已引发的异常的任何信息。 (它，但是，提供说明如何使错误详细信息可查看通过修改`Web.config`文件，它是的作用是将查找不专业的此类 YSOD。)

默认情况下，运行时错误 YSOD 显示给用户访问远程 （通过 http://www.yoursite.com)，如在浏览器的地址栏中的 URL**图 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`。 存在两个不同的 YSOD 屏幕，因为开发人员想要了解错误详细信息，但此类信息应在实时站点上中显示，因为它可能会泄露潜在的安全漏洞或其他敏感信息到的任何用户访问你站点。

> [!NOTE]
> 如果你是内容的观众，并使用 DiscountASP.NET 作为你的 web 主机，你可能注意到运行时错误 YSOD 不会显示访问实时网站时。 这是因为 DiscountASP.NET 具有默认配置为显示异常详细信息 YSOD 其服务器。 好消息是，你可以通过添加来重写此默认行为`<customErrors>`部分为你`Web.config`文件。 "配置错误页显示"部分检查`<customErrors>`中详细信息部分。


[![](displaying-a-custom-error-page-vb/_static/image5.png)](displaying-a-custom-error-page-vb/_static/image4.png)

**图 2**： 运行时错误 YSOD 不包含任何错误详细信息  
 ([单击以查看实际尺寸的图像](displaying-a-custom-error-page-vb/_static/image6.png))

第三种类型的错误页是自定义错误页，这是你创建的网页。 自定义错误页的好处是你可以向页的外观和感觉; 以及用户显示的信息的完全控制自定义错误页可以作为其他页面使用的相同的母版页和样式。 "使用自定义错误页"部分将指导完成创建自定义错误页，并将其显示发生未经处理的异常时配置。 **图 3**提供要抢先了解此自定义错误页。 如你所见，错误页的外观会是死亡的更多的专业以外的黄色屏幕图 1 和 2 中所示。

[![](displaying-a-custom-error-page-vb/_static/image8.png)](displaying-a-custom-error-page-vb/_static/image7.png)

**图 3**： 自定义错误页提供了更定制的外观和感觉  
 ([单击以查看实际尺寸的图像](displaying-a-custom-error-page-vb/_static/image9.png))

花一些时间来检查在浏览器的地址栏**图 3**。 请注意地址栏中显示的自定义错误页的 URL (`/ErrorPages/Oops.aspx`)。 在图 1 和 2 黄色死亡屏幕显示错误源自同一页中 (`Genre.aspx`)。 自定义错误页传递通过发生错误页的 URL`aspxerrorpath`查询字符串参数。

## <a name="configuring-which-error-page-is-displayed"></a>显示配置的错误页

这三个可能的错误页会显示基于两个变量：

- 中的配置信息`<customErrors>`部分中，和
- 是否用户本地或远程访问网站。

[ `<customErrors>`部分](https://msdn.microsoft.com/library/h0hfz6fc.aspx)中`Web.config`有影响显示哪些错误页的两个属性：`defaultRedirect`和`mode`。 `defaultRedirect` 属性是可选项。 如果提供，它指定自定义错误页的 URL，并指示应而不是运行时错误 YSOD 显示自定义错误页。 `mode`属性是必需的接受三个值之一： `On`， `Off`，或`RemoteOnly`。 这些值具有以下行为：

- `On`-指明自定义错误页或运行时错误 YSOD 显示所有访问者，而不考虑它们是否本地或远程。
- `Off`-指定异常详细信息 YSOD 对所有访问者，而不考虑它们是否本地或远程显示。
- `RemoteOnly`-指示，自定义错误页或运行时错误 YSOD 显示给远程访问者，而异常详细信息 YSOD 显示供本地使用访客。

除非另外指定，否则 ASP.NET 看起来就像你已将模式属性设置为`RemoteOnly`且具有未指定`defaultRedirect`值。 换而言之，默认行为是异常详细信息 YSOD 对本地访问者显示，而运行时错误 YSOD 显示远程访问者。 可以通过添加重写此默认行为`<customErrors>`到 web 应用程序的部分`Web.config file.`

## <a name="using-a-custom-error-page"></a>使用自定义错误页

每个 web 应用程序应具有自定义错误页。 它提供了运行时错误 YSOD 的外观更专业的替代方法，很容易创建，和配置用于自定义错误页的应用程序需要只有几分钟。 第一步创建自定义错误页。 我向名为簿评审应用程序添加了一个新文件夹`ErrorPages`并添加到新的 ASP.NET 页名为`Oops.aspx`。 具有你的站点上页的其余部分所在使用相同的母版页，以便它自动继承相同的外观和感觉的页。

[![](displaying-a-custom-error-page-vb/_static/image11.png)](displaying-a-custom-error-page-vb/_static/image10.png)

**图 4**： 创建自定义错误页

接下来，花几分钟时间创建的错误页的内容。 我已与一个消息，表明时发生意外的错误和链接回站点的主页中创建一个相当简单的自定义错误页。

[![](displaying-a-custom-error-page-vb/_static/image13.png)](displaying-a-custom-error-page-vb/_static/image12.png)

**图 5**： 设计你的自定义错误页  
 ([单击以查看实际尺寸的图像](displaying-a-custom-error-page-vb/_static/image14.png))

已完成的错误页面，配置用于运行时错误 YSOD 替代自定义错误页的 web 应用程序。 这通过指定的 URL 中的错误页面实现`<customErrors>`部分的`defaultRedirect`属性。 将以下标记添加到你的应用程序`Web.config`文件：

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample1.xml)]

上面的标记将向用户访问本地，为远程访问这些用户使用自定义错误页 Oops.aspx 时显示异常详细信息 YSOD 应用程序配置。 若要查看此操作中，将你的网站部署到生产环境，然后具有无效的查询字符串值的实时站点上访问 Genre.aspx 该页。 你应看到自定义错误页 (回头参考**图 3**)。

若要验证，自定义错误页仅显示给远程用户，请访问`Genre.aspx`页具有无效的查询字符串从开发环境。 你仍应看到异常详细信息 YSOD (回头参考**图 1**)。 `RemoteOnly`设置可以确保在生产环境中访问网站的用户看到自定义错误页，而在本地工作的开发人员在不断查看异常的详细信息。

## <a name="notifying-developers-and-logging-error-details"></a>通知开发人员和日志记录错误详细信息

由开发人员坐在她的计算机前，在开发环境中发生的错误所引起。 她所示的异常信息异常详细信息 YSOD，并且她知道在发生错误时，她正在执行哪些步骤。 但开发人员生产错误时，可以不知道，除非最终用户访问网站将报告此错误的时间时出错。 并且，即使用户超出其警报出现了错误，而不必知道异常类型、 消息和堆栈跟踪它在某些情况下可能很难诊断原因的错误，更不必说在修复此错误的开发团队的办法。

出于这些原因是至关重要，在生产环境中的任何错误记录到某个持久存储区 （如数据库），开发人员收到此错误的警报。 自定义错误页可能看起来像执行此日志记录和通知的良好开端。 遗憾的是，自定义错误页则无权访问的错误详细信息，因此不能用于记录此信息。 好消息是有多种方法来截获错误详细信息及它们，记录和接下来三个教程浏览本主题中更多详细信息。

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>使用不同的自定义错误页适用于不同的 HTTP 错误状态

如果异常由 ASP.NET 页面引发，并且不处理，异常 percolates 最多的 ASP.NET 运行时显示配置的错误页。 如果请求发送到 ASP.NET 引擎时，但出于某种原因无法处理-可能是请求的文件未找到或读取文件-对于已禁用了权限，则 ASP.NET 引擎将引发`HttpException`。 此异常，如从 ASP.NET 页中，引发的异常冒泡到运行，从而导致相应的错误页后，可以显示。

这在生产中 web 应用程序意味着在于如果用户请求未找到，则他们将看到自定义错误页的页。 **图 6**显示此类示例。 因为请求的不存在页 (`NoSuchPage.aspx`)、`HttpException`引发，并显示自定义错误页面 (请注意对引用`NoSuchPage.aspx`中`aspxerrorpath`查询字符串参数)。

[![](displaying-a-custom-error-page-vb/_static/image16.png)](displaying-a-custom-error-page-vb/_static/image15.png)**图 6**: ASP.NET 运行时显示配置的错误页，以响应无效的请求  
 ([单击以查看实际尺寸的图像](displaying-a-custom-error-page-vb/_static/image17.png)) 

默认情况下，所有类型的错误都导致相同的自定义错误页，以显示。 但是，你可以指定特定 HTTP 状态代码使用一个不同的自定义错误页`<error>`内的子元素`<customErrors>`部分。 例如，若要找不到错误，具有 HTTP 状态代码为 404，页面的情况下显示一个不同的错误页面更新`<customErrors>`部分，以包含以下标记：

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample2.xml)]

进行此更改到位后，每当用户远程访问请求 ASP.NET 资源不存在，它们将重定向到`404.aspx`而不是自定义错误页`Oops.aspx`。 作为**图 7**所示，`404.aspx`页可以包括比常规的自定义错误页的详细消息。

> [!NOTE]
> 签出[404 错误页，一更次](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)有关创建有效的 404 错误页的指导。


[![](displaying-a-custom-error-page-vb/_static/image19.png)](displaying-a-custom-error-page-vb/_static/image18.png)**图 7**： 自定义 404 错误页显示比更有针对性的消息`Oops.aspx`  
 ([单击以查看实际尺寸的图像](displaying-a-custom-error-page-vb/_static/image20.png)) 

因为您知道`404.aspx`仅当用户进行对找不到页的请求到达页，你可以增强了此自定义错误页，以包含功能，以帮助用户解决此指定类型的错误。 例如，您无法生成映射到正确的 Url，已知错误的 Url 的数据库表，然后让`404.aspx`运行针对查询表，建议用户可能在尝试访问的页面的自定义错误页。

> [!NOTE]
> 当资源由 ASP.NET 引擎处理请求时，仅显示自定义错误页。 如我们所述[核心差异之间 IIS 和 ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-vb.md)教程，则 web 服务器可能会以处理特定请求本身。 默认情况下，IIS web 服务器进程请求的静态内容，例如映像和 HTML 文件而无需调用 ASP.NET 引擎。 因此，如果用户请求一个不存在的映像文件用户将会得到 IIS 的默认 404 错误消息，而不是 ASP。NET 的配置错误页。


## <a name="summary"></a>摘要

ASP.NET 应用程序中未经处理的异常时，向用户显示三个错误页之一： 异常详细信息黄色屏幕的死亡;运行时错误黄色屏;或自定义错误页。 显示的错误页面取决于应用程序的`<customErrors>`配置和用户是否访问本地或远程。 默认行为是以供远程访客显示异常详细信息 YSOD 供本地使用访客和运行时错误 YSOD。

时运行时错误 YSOD 隐藏可能敏感的错误信息，从用户访问网站，它将中断从你的站点的外观和感觉，并使你应用程序，看上去错误。 更好的方法是使用自定义错误页，需要创建和设计自定义错误页并指定在其 URL`<customErrors>`部分的`defaultRedirect`属性。 你甚至可以有不同的 HTTP 错误状态的多个自定义错误页。

自定义错误页是一个全面的错误处理策略，以便在生产环境中网站的第一步。 警报错误的开发人员和日志记录其详细信息也是重要的步骤。 接下来三个教程浏览技术错误通知和日志记录。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [错误页，一次](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [异常的设计准则](https://msdn.microsoft.com/library/ms229014.aspx)
- [用户友好的错误页](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [处理和引发异常](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [正确在 ASP.NET 中使用自定义错误页](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

>[!div class="step-by-step"]
[上一页](strategies-for-database-development-and-deployment-vb.md)
[下一页](processing-unhandled-exceptions-vb.md)
