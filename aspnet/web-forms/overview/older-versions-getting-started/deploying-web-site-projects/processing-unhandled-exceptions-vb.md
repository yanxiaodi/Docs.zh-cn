---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: "处理未经处理的异常 (VB) |Microsoft 文档"
author: rick-anderson
description: "对 web 应用程序在生产中发生运行时错误时务必通知开发人员，并记录错误，以便它可能在诊断，a la..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: f5bf39bc2433a2fb277cb56554ddf39bea7a9c79
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
---
<a name="processing-unhandled-exceptions-vb"></a>处理未经处理的异常 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples)（[如何下载](/aspnet/core/tutorials/index#how-to-download-a-sample)）

> 对 web 应用程序在生产中发生运行时错误时务必通知开发人员，并记录错误，以便它可能诊断在稍后的时间。 本教程提供如何 ASP.NET 处理运行时错误，并考察的执行时最多 ASP.NET 运行时未处理的异常气泡的自定义代码的一种方法的概述。


## <a name="introduction"></a>介绍

ASP.NET 应用程序中未经处理的异常时，它将冒泡到 ASP.NET 运行时，将引发`Error`事件，并显示相应的错误页。 有三种不同类型的错误页： 运行时错误黄色屏幕的死亡 (YSOD);异常详细信息 YSOD;和自定义错误页。 在[前面教程](displaying-a-custom-error-page-vb.md)我们配置应用程序以用于远程用户和本地访问的用户异常详细信息 YSOD 的自定义错误页。

使用匹配的站点的外观和感觉人工友好自定义错误页是首选方法为默认值运行时错误 YSOD，但显示自定义错误页是仅一个全面的错误处理解决方案的一部分。 在生产环境中的应用程序错误时，很重要，以便他们可以暴露异常的原因和解决方法，开发人员会通知错误。 此外，以便可以检查错误，并将其时间稍后诊断，则应记录错误的详细信息。

本教程演示如何访问未经处理的异常的详细信息，以便可以记录以及开发人员通知。 按照此一个的两个教程浏览错误日志记录库之后，执行少量配置，将自动通知的运行时错误的开发人员和记录及其详细信息。

> [!NOTE]
> 检查在本教程中的信息是最有用，如果你需要以某种唯一或自定义方式处理未经处理的异常。 在其中你只需将异常记录并通知开发人员的情况下，使用错误日志记录库是转的方法。 下面的两个教程提供了两个这种库的概述。


## <a name="executing-code-when-theerrorevent-is-raised"></a>执行代码时`Error`引发事件

事件提供一种机制信号性沉寂一些有趣的内容已发生的以及要在响应中执行代码的另一个对象的对象。 作为 ASP.NET 开发人员你习惯于在事件方面的考虑。 如果你想要在距访客单击特定的按钮时运行某些代码，该按钮创建一个事件处理程序`Click`事件和放置在那里的你的代码。 假设 ASP.NET 运行时将引发其[`Error`事件](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)每次发生未经处理的异常时，它遵循，用于记录错误的详细信息的代码将进入事件处理程序。 如何创建的事件处理程序，但`Error`事件？

`Error`事件是中的许多事件之一[`HttpApplication`类](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)期间请求的生存期，会引发在 HTTP 管道中的某些阶段。 例如，`HttpApplication`类的[`BeginRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx)引发在开始时每个请求; 其[`AuthenticateRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)安全模块已标识请求者时，将引发。 这些`HttpApplication`事件为页开发人员提供了一种方式来执行请求的生存期内的各个点的自定义逻辑。

事件处理程序`HttpApplication`事件可以放置在名为的特殊文件`Global.asax`。 在你的网站中创建此文件，将新项添加到全局应用程序类模板与名称使用您网站的根目录`Global.asax`。

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**图 1**： 添加`Global.asax`到 Web 应用程序  
 ([单击以查看实际尺寸的图像](processing-unhandled-exceptions-vb/_static/image3.png))

内容和结构`Global.asax`略有根据使用的 Web 应用程序项目 (WAP) 或网站项目 (WSP) Visual Studio 创建的文件会有所不同。 与 WAP`Global.asax`作为两个单独的文件-实现`Global.asax`和`Global.asax.vb`。 `Global.asax`文件不包含任何内容，但`@Application`引用的指令`.vb`文件; 感兴趣的处理程序中定义的事件`Global.asax.vb`文件。 对于 WSPs，都会创建单个文件， `Global.asax`，并且事件处理程序将在定义`<script runat="server">`块。

`Global.asax`在 WAP 中创建 Visual Studio 的全局应用程序类模板的文件包含名为的事件处理程序`Application_BeginRequest`， `Application_AuthenticateRequest`，和`Application_Error`，这是事件处理程序`HttpApplication`事件`BeginRequest`， `AuthenticateRequest`，和`Error`分别。 也有事件处理程序名为`Application_Start`， `Session_Start`， `Application_End`，和`Session_End`，后者是 web 应用程序启动时、 时将新的会话启动，应用程序结束时，会触发的事件处理程序并在会话结束时，分别。 `Global.asax` WSP 在 Visual Studio 中创建的文件只包含`Application_Error`， `Application_Start`， `Session_Start`， `Application_End`，和`Session_End`事件处理程序。

> [!NOTE]
> 部署 ASP.NET 应用程序时，你需要进行复制`Global.asax`到生产环境的文件。 `Global.asax.vb`文件，在 WAP 中创建的是，不需要复制到生产环境中，因为此代码会编译成项目的程序集。


由 Visual Studio 的全局应用程序类模板创建的事件处理程序并不详尽。 您可以为任何添加事件处理程序`HttpApplication`通过命名事件处理程序的事件`Application_EventName`。 例如，可以添加到下面的代码`Global.asax`文件以创建的事件处理程序[`AuthorizeRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

同样，你可以删除全局应用程序类模板创建的任何事件处理程序，则不需要。 本教程中我们只需要一个事件处理程序`Error`事件; 随意删除从其他事件处理程序`Global.asax`文件。

> [!NOTE]
> *HTTP 模块*提供另一种方法来定义事件处理程序`HttpApplication`事件。 HTTP 模块创建为一个类文件，可以直接在 web 应用程序项目内放置或分隔到单独的类库。 因为它们可以分为一个类库，HTTP 模块提供用于创建更加灵活和可重用模型`HttpApplication`事件处理程序。 而`Global.asax`文件是特定于它所在的位置的 web 应用，HTTP 模块可以编译到程序集，此时将 HTTP 模块添加到网站非常简单，只将程序集放入适当`Bin`文件夹和注册中的模块`Web.config`。 本教程不会查找在创建和使用 HTTP 模块，但在以下两个教程中使用的两个错误日志记录库实现为 HTTP 模块。 有关 HTTP 模块的优势的更多背景指[使用 HTTP 模块和处理程序以创建可插入 ASP.NET 组件](https://msdn.microsoft.com/library/aa479332.aspx)。


## <a name="retrieving-information-about-the-unhandled-exception"></a>检索有关未经处理的异常的信息

此时我们可以具有的 Global.asax 文件`Application_Error`事件处理程序。 此事件处理程序执行时我们需要通知开发人员的错误和日志其详细信息。 若要完成这些任务我们首先需要确定引发异常的详细信息。 使用服务器对象的[`GetLastError`方法](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx)若要检索的未经处理的异常导致的详细信息`Error`事件激发。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

`GetLastError`方法返回类型的对象`Exception`，这是.NET Framework 中的所有异常的基类型。 但是，在上面的代码我正在强制转换返回的异常对象`GetLastError`到`HttpException`对象。 如果`Error`因为抛出异常 ASP.NET 资源在处理过程然后已引发的异常包装在激发事件`HttpException`。 若要获取引发错误事件使用的实际异常`InnerException`属性。 如果`Error`事件被引发因为基于 HTTP 的异常，如不存在页，请求`HttpException`引发，但它不具有内部异常。

下面的代码使用`GetLastErrormessage`检索有关触发的异常信息`Error`事件，存储`HttpException`在名为`lastErrorWrapper`。 它然后将存储类型、 消息和堆栈跟踪的原始异常在三个字符串变量，检查以查看是否`lastErrorWrapper`触发实际异常`Error`事件 （对于基于 HTTP 的异常） 或是否仅在处理请求时引发的异常的包装器。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

此时，你具有需要编写代码将记录到数据库表的异常的详细信息的所有信息。 为每个感兴趣的类型、 消息、 堆栈跟踪和等等的其他有用信息段，如请求的页面的 URL 和当前登录用户的名称以及错误详细信息，无法创建带有列的数据库表。 在`Application_Error`事件处理程序然后将连接到数据库，并将一个记录插入表。 同样，你可以添加代码以警报通过电子邮件错误的开发人员。

检查下面的两个教程中的错误日志记录库提供在初始状态下，此类功能，因此无需自行生成此错误日志记录和通知。 但是，为了说明这一点`Error`正在引发事件且`Application_Error`事件处理程序可以用于记录错误详细信息和通知开发人员，让我们添加发生错误时通知开发人员的代码。

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>未经处理的异常发生时通知开发人员

在生产环境中发生未经处理的异常时务必警告开发团队，以便它们可以评估错误，并确定需要采取的操作。 例如，如果没有中连接到数据库，然后你将需要为双精度的错误检查连接字符串，并且可能是，打开支持票证与您的 web 托管公司。 如果由于编程错误导致出现异常，可能需要其他代码或验证逻辑要添加到在将来防止此类错误。

.NET Framework 中的类[`System.Net.Mail`命名空间](https://msdn.microsoft.com/library/system.net.mail.aspx)方便地发送一封电子邮件。 [ `MailMessage`类](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx)表示电子邮件并具有属性，例如`To`， `From`， `Subject`， `Body`，和`Attachments`。 `SmtpClass`用于发送`MailMessage`对象使用指定的 SMTP 服务器; 可以以编程方式或以声明方式在指定的 SMTP 服务器设置[`<system.net>`元素](https://msdn.microsoft.com/library/6484zdc1.aspx)中`Web.config file`。 有关详细信息发送电子邮件 ASP.NET 应用程序中的消息签出我文章[在 ASP.NET 中发送电子邮件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)，和[System.Net.Mail 常见问题](http://systemnetmail.com/)。

> [!NOTE]
> `<system.net>`元素包含使用的 SMTP 服务器设置`SmtpClient`类发送一封电子邮件时。 您的 web 托管公司可能具有可用于从你的应用程序发送电子邮件的 SMTP 服务器。 有关应在 web 应用程序中使用的 SMTP 服务器设置的信息，请查阅你的 web 主机支持部分。


以下代码添加到`Application_Error`事件处理程序来发送一封电子邮件开发人员时发生错误：

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

上面的代码非常长时，它的大容量在发送给开发人员的电子邮件中创建显示的 HTML。 代码通过引用将启动`HttpException`返回`GetLastError`方法 (`lastErrorWrapper`)。 通过检索实际引发的请求的异常`lastErrorWrapper.InnerException`并分配给变量`lastError`。 类型、 消息和堆栈跟踪信息检索从`lastError`并存储在三个字符串变量。

接下来，`MailMessage`对象名为`mm`创建。 电子邮件正文是 HTML 格式，并显示所请求的页面的 URL、 当前登录的用户，以及有关异常 （类型、 消息和堆栈跟踪） 的信息的名称。 有关的冷项目之一`HttpException`类是你可以生成用于创建异常详细信息黄色屏幕的死亡 (YSOD) 通过调用的 HTML [GetHtmlErrorMessage 方法](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx)。 此处使用此方法检索的异常详细信息 YSOD 标记并将其添加到作为附件的电子邮件。 需要注意的一个字： 如果异常，触发`Error`事件已基于 HTTP 的异常 （如不存在页的请求） 则`GetHtmlErrorMessage`方法将返回`null`。

最后一步是将发送`MailMessage`。 这可通过创建一个新`SmtpClient`方法并调用其`Send`方法。

> [!NOTE]
> Web 应用程序中使用此代码之前你将需要更改中的值`ToAddress`和`FromAddress`常量从support@example.com到任何电子邮件地址的错误通知电子邮件应发送到和来自。 你还需要指定 SMTP 服务器设置中的`<system.net>`主题中`Web.config`。 请查阅你的 web 主机提供商，以确定要使用的 SMTP 服务器设置。


对于就地此代码时出现错误的任何时候开发人员将发送电子邮件对错误进行概述，包括 YSOD。 我们可以在前面的教程来演示通过访问 Genre.aspx，传递中无效的运行时错误`ID`值通过查询字符串，如`Genre.aspx?ID=foo`。 访问的页`Global.asax`就地文件生成相同的用户体验与在前面的教程-在开发环境中你将继续时在生产环境中你将看到异常详细信息黄色死机，请参阅自定义错误页。 除了此现有的行为，开发人员发送一封电子邮件。

**图 2**显示访问时收到的电子邮件`Genre.aspx?ID=foo`。 电子邮件正文总结了异常的信息，而`YSOD.htm`附件，显示异常详细信息 YSOD 所示的内容 (请参阅**图 3**)。

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**图 2**： 开发人员在未处理的异常时发送电子邮件通知  
 ([单击以查看实际尺寸的图像](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**图 3**： 电子邮件通知中包括异常详细信息作为附件 YSOD  
 ([单击以查看实际尺寸的图像](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>怎样使用自定义错误页？

本教程说明了如何使用`Global.asax`和`Application_Error`事件处理程序代码发生时要执行未经处理的异常。 具体而言，我们使用此事件处理程序以通知的错误; 开发人员我们无法扩展它还在数据库中记录错误详细信息。 是否存在`Application_Error`事件处理程序不会影响最终用户体验。 他们仍将看到配置的错误页面，错误详细信息 YSOD、 运行时错误 YSOD，或自定义错误页。

自然想知道是否`Global.asax`文件和`Application_Error`事件时是必需的使用自定义错误页。 出现错误时向用户显示自定义错误页因此为什么无法我们将代码以通知开发人员并登录到自定义错误页的代码隐藏类的错误详细信息？ 当然，你可以将代码添加到自定义错误页的代码隐藏类时没有触发异常的详细信息的访问`Error`事件时使用我们探讨了前面教程中的技术。 调用`GetLastError`从自定义错误页的方法返回`Nothing`。

此行为的原因是因为自定义错误页达到通过重定向。 在未经处理的异常到达 ASP.NET 运行时 ASP.NET 引擎引发其`Error`事件 (其中执行`Application_Error`事件处理程序)，然后*将重定向*对自定义错误页通过发出用户`Response.Redirect(customErrorPageUrl)`. `Response.Redirect`方法可发送到客户端，HTTP 302 状态代码，指示要请求新的 URL，即自定义错误页的浏览器的响应。 然后浏览器会自动请求此新页。 你可以判断，从页面中自定义错误页单独请求错误源于何处因为浏览器的地址栏中更改为自定义错误页的 URL (请参阅**图 4**)。

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**图 4**： 发生错误时浏览器将重定向到自定义错误页的 URL  
 ([单击以查看实际尺寸的图像](processing-unhandled-exceptions-vb/_static/image12.png))

净效果是未经处理的异常发生在请求结束时服务器的响应使用 HTTP 302 重定向。 对自定义错误页的后续请求是一个全新的请求;此时 ASP.NET 引擎已丢弃的错误信息，此外，就无法将上一个请求中未处理的异常与新的自定义错误页请求相关联。 正因如此`GetLastError`返回`null`从自定义错误页中调用时。

但是，很可能能够在同一导致错误的请求期间执行自定义错误页。 [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx)方法将执行传送到指定的 URL，并在同一请求对其进行处理。 无法将代码移动`Application_Error`到自定义错误页的代码隐藏类，替换中的事件处理程序`Global.asax`替换为以下代码：

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

未经处理的异常发生时现在`Application_Error`事件处理程序将控制权转交给根据 HTTP 状态代码的相应的自定义错误页。 自定义错误页由于传送控件，有权访问通过未经处理的异常信息`Server.GetLastError`和可以通知开发人员的错误和日志其详细信息。 `Server.Transfer`调用停止中将用户重定向到自定义错误页的 ASP.NET 引擎。 相反，作为对生成错误的页的响应返回自定义错误页的内容。

## <a name="summary"></a>摘要

ASP.NET 运行时在 ASP.NET web 应用程序中发生未经处理的异常时引发`Error`事件，并显示配置的错误页。 我们可以通知开发人员的错误，记录其详细信息，或者处理其以某种其他方式，通过创建的事件处理程序错误事件。 有两种方法创建的事件处理程序`HttpApplication`事件，如`Error`： 在`Global.asax`文件或从 HTTP 模块。 本教程介绍了如何创建`Error`中的事件处理程序`Global.asax`通过电子邮件通知的错误的开发人员的文件。

创建`Error`事件处理程序是你需要以某种唯一或自定义方式处理未经处理的异常的情况下很有用。 但是，创建你自己`Error`事件处理程序将异常记录或通知开发人员不最有效的时间使用，因为已存在可以在几分钟内中的设置的可用且易于使用的错误日志记录库。 下面的两个教程检查两个这种库。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET HTTP 模块和 HTTP 处理程序概述](https://support.microsoft.com/kb/307985)
- [正常响应未经处理的异常的处理未经处理的异常](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` 类和 ASP.NET 应用程序对象](http://www.eggheadcafe.com/articles/20030211.asp)
- [HTTP 处理程序和 ASP.NET 中的 HTTP 模块](http://www.15seconds.com/Issue/020417.htm)
- [在 ASP.NET 中发送电子邮件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [了解`Global.asax`文件](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [使用 HTTP 模块和处理程序创建可插入 ASP.NET 组件](https://msdn.microsoft.com/library/aa479332.aspx)
- [使用 ASP.NET`Global.asax`文件](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [使用`HttpApplication`实例](https://msdn.microsoft.com/library/a0xez8f2.aspx)

>[!div class="step-by-step"]
[上一页](displaying-a-custom-error-page-vb.md)
[下一页](logging-error-details-with-asp-net-health-monitoring-vb.md)
