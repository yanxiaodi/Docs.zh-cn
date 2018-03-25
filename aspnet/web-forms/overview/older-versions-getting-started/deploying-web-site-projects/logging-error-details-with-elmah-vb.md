---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: "使用 ELMAH (VB) 日志记录错误详细信息 |Microsoft 文档"
author: rick-anderson
description: "错误日志记录模块和处理程序 (ELMAH) 提供日志运行时错误记录在生产环境中的另一种方法。 ELMAH 是一个免费、 开源错误..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: b4bba02449debff17422f6b7008247fdf61856c8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="logging-error-details-with-elmah-vb"></a>使用 ELMAH (VB) 日志记录错误详细信息
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip)或[下载 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> 错误日志记录模块和处理程序 (ELMAH) 提供日志运行时错误记录在生产环境中的另一种方法。 ELMAH 是一个免费、 开源错误日志记录库，包括错误筛选和具有为 RSS 源，请查看错误日志在网页上，或若要下载作为以逗号分隔文件的能力等功能。 本教程将指导完成下载和配置 ELMAH。


## <a name="introduction"></a>介绍

[前面教程](logging-error-details-with-asp-net-health-monitoring-vb.md)检查 ASP。NET 的运行状况监视系统，它提供用于记录多种不同的 Web 事件框库外。 许多开发人员使用运行状况监视日志，并通过电子邮件发送的未经处理的异常的详细信息。 但是，没有与此系统的一些难点。 首先是缺少任何种类的用户界面，用于查看有关记录的事件信息。 如果你想要查看的 10 个的最后一个错误，摘要或查看上周发生了错误的详细信息，则必须通过数据库任一挤出、 浏览你的电子邮件收件箱，或生成显示来自信息的网页`aspnet_WebEvent_Events`表。

另一个难题是围绕运行状况监视的复杂性。 由于运行状况监视可用于记录大量不同的事件，并且有各种选项指示如何以及何时记录事件，正确配置运行状况监视系统可以繁重任务。 最后，有兼容性问题。 由于运行状况监视第一次添加到.NET Framework 2.0 版中，也无法供较旧的 web 应用程序生成使用 ASP.NET 版本 1.x。 此外，`SqlWebEventProvider`类，该类使用在前面的教程到数据库的日志错误详细信息，仅适用于 Microsoft SQL Server 数据库。 你将需要创建自定义日志提供程序类应需要将错误记录到 XML 文件或 Oracle 数据库等一个备用数据存储区。

监视系统的运行状况的替代方法是错误日志记录模块和处理程序 (ELMAH)，通过创建免费的开源错误日志记录系统[Atif Aziz](http://www.raboof.com/)。 两个系统之间最显著的差异是 ELAMH 的能力，以显示为 RSS 源的错误和从网页和特定错误的详细信息列表。 ELMAH 很容易配置比运行状况监视，因为它仅记录错误。 此外，ELMAH 包括对 ASP.NET 1.x、 ASP.NET 2.0 中，和 ASP.NET 3.5 应用程序和附带了各种日志源提供程序支持。

本教程将指导通过将 ELMAH 添加到 ASP.NET 应用程序所涉及的步骤。 让我们进入正题！

> [!NOTE]
> 监视系统资源和 ELMAH 的运行状况都具有其自己的优点和缺点的集。 我建议你尝试这两个系统，并决定哪一个最适合你的需求。


## <a name="adding-elmah-to-an-aspnet-web-application"></a>将 ELMAH 添加到 ASP.NET Web 应用程序

将 ELMAH 集成到一个新的或现有的 ASP.NET 应用程序是一个简单、 直接的过程采用在五分钟。 简而言之，它涉及四个简单步骤：

1. 下载 ELMAH 并将添加`Elmah.dll`到 web 应用程序，程序集
2. 注册 ELMAH 的 HTTP 模块和处理程序中的`Web.config`，
3. 指定 ELMAH 的配置选项，并
4. 如果需要请创建错误日志中的源基础结构。

让我们演练一下每一次一个地以下四个步骤。

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>步骤 1： 下载 ELMAH 项目文件并添加`Elmah.dll`到 Web 应用程序

ELMAH 1.0 BETA 3 (生成 10617)，在本文撰写之际，最新版本包括在本教程提供下载。 或者，也可以访问[ELMAH 网站](https://code.google.com/p/elmah/)以获取最新版本，或若要下载源代码。 提取 ELMAH 下载到你的桌面上的文件夹并找到 ELMAH 程序集文件 (`Elmah.dll`)。

> [!NOTE]
> `Elmah.dll`文件位于中下载的`Bin`文件夹，它具有不同的.NET Framework 版本和发行版本和调试生成的子文件夹。 使用合适的框架版本发布版本。 例如，如果您构建了一个 ASP.NET 3.5 web 应用，则将复制`Elmah.dll`文件从`Bin\net-3.5\Release`文件夹。


接下来，打开 Visual Studio，然后通过右键单击解决方案资源管理器和选择添加的引用，从上下文菜单中的网站名称添加到你的项目的程序集。 这会打开添加引用对话框。 导航到浏览选项卡，然后选择`Elmah.dll`文件。 此操作将添加`Elmah.dll`文件复制到 web 应用程序的`Bin`文件夹。

> [!NOTE]
> Web 应用程序项目 (WAP) 类型不会显示`Bin`在解决方案资源管理器的文件夹。 相反，它列出在引用文件夹下的这些项。


`Elmah.dll`程序集包括 ELMAH 系统使用的类。 这些类划分为三个类别之一：

- **HTTP 模块**-HTTP 模块是用于定义事件处理程序类`HttpApplication`事件，如`Error`事件。 ELMAH 包括多个 HTTP 模块，三个最 germane 的正在： 

    - `ErrorLogModule`-将未经处理的异常记录到日志源。
    - `ErrorMailModule`-发送的电子邮件中的未经处理的异常的详细信息。
    - `ErrorFilterModule`-将应用开发人员指定筛选器，以确定记录哪些异常的方式以及是将被忽略。
- **HTTP 处理程序**-一个 HTTP 处理程序是负责生成特定类型的请求的标记的类。 ELMAH 包括呈现错误详细信息，作为网页、 RSS 源，或以逗号分隔的文件 (CSV) 的 HTTP 处理程序。
- **错误日志源**-现成 ELMAH 可以记录到内存中，到 Microsoft SQL Server 数据库，到 Microsoft Access 数据库，到 Oracle 数据库，错误到 XML 文件，到 SQLite 数据库，或 Vista DB 数据库。 监视系统的运行状况，如 ELMAH 的体系结构是使用提供程序模型，这意味着你可以创建和无缝集成自己自定义日志的源提供程序，如果需要生成的。

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>步骤 2： 注册 ELMAH 的 HTTP 模块和处理程序

虽然`Elmah.dll`文件包含 HTTP 模块和处理程序需要自动登录未经处理的异常并显示在网页上的错误详细信息，这些必须显式注册 web 应用程序的配置中。 `ErrorLogModule` HTTP 模块，注册后，订阅`HttpApplication`的`Error`事件。 每当将引发此事件`ErrorLogModule`将异常的详细信息记录到指定的日志源。 我们将了解如何在下一步的部分中，定义日志源提供程序"配置 ELMAH。" `ErrorLogPageFactory` HTTP 处理程序工厂负责生成标记，查看错误日志在网页上的时。

注册 HTTP 模块和处理程序的特定语法取决于正在该站点的 web 服务器。 ASP.NET Development Server 和 Microsoft 的 iis 版本 6.0 及更早版本，HTTP 模块和处理程序中注册`<httpModules>`和`<httpHandlers>`各节，其中会出现在`<system.web>`元素。 如果你使用的 IIS 7.0，则它们需要在中注册`<system.webServer>`元素的`<modules>`和`<handlers>`部分。 幸运的是，你可以在其中定义的 HTTP 模块和处理程序中的*同时*放置而不考虑所使用的 web 服务器。 此选项是最可移植的一个，因为它允许而不考虑所使用的 web 服务器的开发和生产环境中使用的相同配置。

通过注册开始`ErrorLogModule`HTTP 模块和`ErrorLogPageFactory`中的 HTTP 处理程序`<httpModules>`和`<httpHandlers>`主题中`<system.web>`。 如果你配置已定义这两个元素然后只需包括`<add>`ELMAH 的 HTTP 模块和处理程序元素。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

接下来，注册 ELMAH 的 HTTP 模块和处理程序中的`<system.webServer>`元素。 和前面一样，如果此元素不存在你的配置中将其添加。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

默认情况下，IIS 7 会指出如果在注册 HTTP 模块和处理`<system.web>`部分。 `validateIntegratedModeConfiguration`属性中`<validation>`元素指示 IIS 7 以禁止显示此类错误消息。

请注意，注册的语法`ErrorLogPageFactory`HTTP 处理程序包括`path`属性，它设置为`elmah.axd`。 此特性通知 web 应用程序，如果为一个名为页的请求到达时`elmah.axd`然后应处理此请求`ErrorLogPageFactory`HTTP 处理程序。 我们将看到`ErrorLogPageFactory`中更高版本在本教程中的操作的 HTTP 处理程序。

### <a name="step-3-configuring-elmah"></a>步骤 3： 配置 ELMAH

ELMAH 查找其配置选项在网站的`Web.config`名为的自定义配置节中的文件`<elmah>`。 若要使用中的自定义节`Web.config`首先必须在中定义它`<configSections>`元素。 打开`Web.config`文件并添加以下标记`<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> 如果你要配置 ELMAH ASP.NET 1.x 应用程序然后删除`requirePermission="false"`属性从`<section>`上面的元素。


以上语法注册自定义`<elmah>`部分和其各小节： `<security>`， `<errorLog>`， `<errorMail>`，和`<errorFilter>`。

接下来，添加`<elmah>`部分到`Web.config`。 本部分应出现在与相同的级别`<system.web>`元素。 内部`<elmah>`部分添加`<security>`和`<errorLog>`各节如下所示：

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

`<security>`部分的`allowRemoteAccess`属性指示是否允许远程访问。 如果此值设置为 0，然后错误日志 web 页可以仅查看本地。 如果此属性设置为 1 然后是否为远程和本地访客启用了错误日志 web 页。 现在，让我们为远程访客禁用错误日志 web 页。 我们有机会讨论这样做的安全问题后，我们将更高版本允许远程访问。

`<errorLog>`部分定义错误日志源，其中记录错误详细信息; 它是类似于哪些决定`<providers>`中监视系统的运行状况的部分。 上面的语法指定`SqlErrorLog`类作为错误日志源，将错误记录到指定的 Microsoft SQL Server 数据库`connectionStringName`属性值。

> [!NOTE]
> ELMAH 附带可用于 XML 文件、 Microsoft Access 数据库、 Oracle 数据库和其他数据存储中记录错误的其他错误日志提供程序。 请参阅示例`Web.config`是包含有关如何使用这些备用错误日志提供程序的信息在 ELMAH 下载的文件。


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>步骤 4： 创建错误日志源基础结构

ELMAH 的`SqlErrorLog`提供程序错误详细信息记录到指定的 Microsoft SQL Server 数据库。 `SqlErrorLog`提供程序要求此数据库，才能具有名为的表`ELMAH_Error`和三个存储过程： `ELMAH_GetErrorsXml`， `ELMAH_GetErrorXml`，和`ELMAH_LogError`。 ELMAH 下载内容还包括名为的文件`SQLServer.sql`中`db`文件夹包含用于创建此表并将这些 T-SQL 存储过程。 你将需要在你使用的数据库上运行这些语句`SqlErrorLog`提供程序。

**图 1**和**2**后所需的数据库对象显示在 Visual Studio 中的数据库资源管理器`SqlErrorLog`已添加提供程序。

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**图 1**:`SqlErrorLog`提供程序日志将错误记录到`ELMAH_Error`表

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**图 2**:`SqlErrorLog`提供程序使用三个存储的过程

## <a name="elmah-in-action"></a>在操作中的 ELMAH

此时我们已添加到注册的 web 应用程序的 ELMAH `ErrorLogModule` HTTP 模块和`ErrorLogPageFactory`HTTP 处理程序中，指定中的 ELMAH 的配置选项`Web.config`，并添加所需的数据库对象的`SqlErrorLog`错误日志提供程序。 现在，我们已准备好在操作中看到 ELMAH ！ 访问图书评论网站，访问生成运行时错误，例如页面`Genre.aspx?ID=foo`，或不存在页，如`NoSuchPage.aspx`。 在访问这些页时看到的内容取决于`<customErrors>`配置和是否正在访问本地或远程。 (回头参考[*显示自定义错误页*教程](displaying-a-custom-error-page-vb.md)上本主题复习。)

ELMAH 不会影响发生未经处理的异常; 时，向用户显示的内容它只记录了其详细信息。 此错误日志文件已可从 web 页访问`elmah.axd`的根中你的网站，如`http://localhost/BookReviews/elmah.axd`。 (此文件不以物理方式存在在项目中，但请求传入的`elmah.axd`运行时将调度到`ErrorLogPageFactory`HTTP 处理程序，用于生成发送回浏览器的标记。)

> [!NOTE]
> 你还可以使用`elmah.axd`页后，可以指示 ELMAH 生成一个测试错误。 访问`elmah.axd/test`(作为 in `http://localhost/BookReviews/elmah.axd/test`) 会导致引发异常的类型的 ELMAH `Elmah.TestException`，其中包含的错误消息:"This is 可以安全地忽略测试异常"。


**图 3**显示错误日志时访问`elmah.axd`从开发环境。

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**图 3**:`Elmah.axd`显示在网页上的错误日志  
([单击以查看实际尺寸的图像](logging-error-details-with-elmah-vb/_static/image7.png))

错误日志**图 3**包含六个错误条目。 每个条目包括 HTTP 状态代码 （404 或 500，这些错误）、 类型、 说明、 时出错，请登录的用户的名称和日期和时间。 单击详细信息链接显示包括错误详细信息黄色屏幕的死亡中所示相同的错误消息的页面 (请参阅**图 4**) 以及错误时服务器变量的值 (请参阅**图 5**)。 你还可以查看在其中保存错误详细信息，其中包括 HTTP POST 标头中的值之类的其他信息的原始 XML。

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**图 4**： 查看错误详细信息 YSOD  
([单击以查看实际尺寸的图像](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**图 5**： 浏览服务器变量集合时的错误的值  
([单击以查看实际尺寸的图像](logging-error-details-with-elmah-vb/_static/image13.png))

部署到生产网站 ELMAH 必需操作：

- 复制`Elmah.dll`文件为`Bin`上生产，文件夹
- 复制到的 ELMAH 特定配置设置`Web.config`在生产环境，使用文件和
- 将错误日志源基础结构添加到生产数据库。

我们已介绍了用于复制文件从开发到生产环境中前面的教程的技术。 获取生产数据库上的错误日志源基础结构的最简单方法是使用 SQL Server Management Studio 以连接到生产数据库，然后执行可能`SqlServer.sql`脚本文件，这将创建所需的表和存储过程。

### <a name="viewing-the-error-details-page-on-production"></a>在生产上查看错误详细信息页

之后将您的网站部署到生产，访问生产网站，并生成未经处理的异常。 与开发环境中，ELMAH 产生任何影响在出现未经处理的异常; 时显示的错误页相反，它仅记录的错误。 如果你尝试访问错误日志页 (`elmah.axd`) 从生产环境中，你将看到与禁止访问页中所示**图 6**。

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**图 6**： 默认情况下，远程访问者不能查看错误日志 Web 页  
([单击以查看实际尺寸的图像](logging-error-details-with-elmah-vb/_static/image16.png))

回想一下，中的 ELMAH 配置`<security>`部分，我们将设置`allowRemoteAccess`属性设为 0，这将禁止远程用户查看错误日志。 请务必禁止匿名访问者查看错误日志，如错误详细信息可能会显示安全漏洞或其他敏感信息。 如果你决定将此属性设置为 1 并启用远程访问错误日志，则务必锁定`elmah.axd`路径，因此，只有经过授权的访问者可以访问它。 这可以通过添加`<location>`元素`Web.config`文件。

下面的配置允许管理员角色，若要访问错误日志 web 页中只有用户：

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> 在中添加了管理员角色和 Scott、 Jisun 和 Alice 的系统中的三个用户[*配置网站，使用应用程序服务*教程](configuring-a-website-that-uses-application-services-vb.md)。 用户 Scott 和 Jisun 是管理员角色的成员。 有关身份验证和授权的详细信息，请参阅我[网站安全教程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。


现在可以通过远程用户; 查看错误日志在生产环境将回指**图 3**， **4**，和**5**有关的错误日志 web 页的屏幕截图。 但是，如果为匿名或非管理员用户尝试查看错误日志页它们会自动重定向到登录页 (`Login.aspx`)，作为**图 7**显示。

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**图 7**： 未经授权的用户将自动重定向到登录页  
([单击以查看实际尺寸的图像](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>以编程方式日志记录错误

ELMAH 的`ErrorLogModule`HTTP 模块将会自动记录到指定的日志源的未经处理的异常。 或者，你可以登录而无需通过引发未处理的异常的错误`ErrorSignal`类并将其`Raise`方法。 `Raise`方法传递`Exception`对象并将其记录像该异常被引发了异常，而无需处理已经达到 ASP.NET 运行时。 但是，之处在于，请求将继续正常之后执行`Raise`已调用了方法，而引发，未经处理的异常中断请求的正常执行，并将导致 ASP.NET 运行时显示配置错误页。

`ErrorSignal`类很有用，其中可能会失败，某些操作，但其失败不是灾难性的总体所执行的操作的情况。 例如，网站可能包含一个窗体，获取用户的输入，将其存储在数据库中，然后将用户发送一封电子邮件，告知他们他们处理信息。 如果成功，信息保存到数据库但没有错误时发送电子邮件，则应发生什么情况？ 一个选项将引发异常，并向用户发送到错误页。 但是，这可能会将用户混淆误以为不保存他们输入的信息。 另一种方法将记录与电子邮件相关的错误，但不是能改变以任何方式的用户的体验。 这就是`ErrorSignal`类很有用。

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>通过电子邮件的错误通知

到数据库的日志记录错误，以及 ELMAH 还可以配置为通过电子邮件发送给指定收件人的错误详细信息。 此功能由`ErrorMailModule`HTTP 模块中; 因此，必须注册此 HTTP 模块`Web.config`才能发送错误详细信息，通过电子邮件。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

接下来，指定有关中的错误电子邮件信息`<elmah>`元素的`<errorMail>`部分中，电子邮件的发件人和收件人，使用者，该值指示是否以异步方式发送电子邮件。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

使用就地上述设置，每当出现运行时错误，则 ELMAH 发送到电子邮件support@example.com具有错误详细信息。 ELMAH 的错误电子邮件包含错误详细信息 web 页，即错误消息、 堆栈跟踪和服务器变量中所示的相同信息 (回头参考**图 4**和**5**)。 错误电子邮件还包括作为附件的异常详细信息黄色屏幕的死亡内容 (`YSOD.html`)。

**图 8**显示 ELMAH 的错误电子邮件，请访问生成`Genre.aspx?ID=foo`。 虽然**图 8**显示仅错误消息和堆栈跟踪，则服务器变量是包含进一步向下电子邮件的正文中。

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**图 8**： 你可以配置 ELMAH 发送错误详细信息，通过电子邮件  
([单击以查看实际尺寸的图像](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>仅记录感兴趣的错误

默认情况下，ELMAH 记录每个未处理的异常，包括 404 和其他 HTTP 错误的详细信息。 你可以指示 ELMAH 以忽略这些或其他类型的使用错误筛选的错误。 筛选逻辑由 ELMAH 的`ErrorFilterModule`HTTP 模块，你将需要在注册`Web.config`若要使用的筛选逻辑。 有关筛选规则中指定`<errorFilter>`部分。

以下标记指示 ELMAH 不记录 404 错误。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> 不要忘记，若要使用错误筛选必须注册`ErrorFilterModule`HTTP 模块。


`<equal>`内的元素`<test>`部分被称为断言。 如果断言的计算结果为 true 则错误筛选从 ELMAH 的日志。 有可用的其他断言，包括： `<greater>`， `<greater-or-equal>`， `<not-equal>`， `<lesser>`， `<lesser-or-equal>`，依次类推。 你也可以组合使用断言`<and>`和`<or>`布尔运算符。 更重要的是，你甚至可以包括一个简单的 JavaScript 表达式，用作断言，或在 C# 或 Visual Basic 中编写您自己的断言。

有关 ELMAH 的错误筛选功能的详细信息，请参阅[错误筛选部分](https://code.google.com/p/elmah/wiki/ErrorFiltering)中[ELMAH wiki](https://code.google.com/p/elmah/w/list)。

## <a name="summary"></a>摘要

ELMAH 提供有关 ASP.NET web 应用程序中的日志记录错误的简单但强大机制。 Microsoft 的运行状况监视系统，如 ELMAH 可以将错误记录到数据库，并可以向开发人员通过电子邮件发送错误详细信息。 与监视系统的运行状况，不同 ELMAH 包括外框支持广泛的错误日志数据存储，包括： Microsoft SQL Server、 Microsoft Access、 Oracle、 XML 文件以及其他几个人。 此外，ELMAH 提供内置机制来查看错误日志和 web 页上，从特定错误有关的详细信息`elmah.axd`。 `elmah.axd`页也可呈现为 RSS 源或逗号分隔值文件 (CSV)，您可以使用 Microsoft Excel 读取的错误信息。 你还可以从使用声明性或以编程方式断言的日志筛选器的错误指示 ELMAH。 并可与 ASP.NET 版本 1.x 应用程序使用 ELMAH。

每个已部署的应用程序应具有一些机制来自动日志记录未经处理的异常以及将通知发送给开发团队。 是否使用运行状况监视或 ELMAH 完成这是辅助。 换而言之，它不会真正有意义得多是否使用运行状况监视或 ELMAH;评估这两个系统，然后选择最适合你需要的一个。 从根本上而言重要的是某种机制将的位置，以在生产环境中记录未经处理的异常。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ELMAH 的错误日志记录模块和处理程序](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [ELMAH 项目页](https://code.google.com/p/elmah/)（源的代码、 示例、 wiki）
- [插入 ELMAH 到 Web 应用程序到捕获未经处理的异常](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx)（视频）
- [安全错误日志页](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [使用 HTTP 模块和处理程序创建可插入 ASP.NET 组件](https://msdn.microsoft.com/library/aa479332.aspx)
- [网站安全教程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

>[!div class="step-by-step"]
[上一页](logging-error-details-with-asp-net-health-monitoring-vb.md)
[下一页](precompiling-your-website-vb.md)
