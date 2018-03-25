---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
title: "核心 IIS 和 ASP.NET Development Server (C#) 之间的差异 |Microsoft 文档"
author: rick-anderson
description: "当测试 ASP.NET 应用程序本地时，很有可能正在使用 ASP.NET 开发 Web 服务器。 但是，生产网站是最有可能 pow..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 13a5a423-9235-4dde-b408-2fd10f791d63
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 79f06707cadf027baa03652dc722cab31f494b09
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-c"></a>核心 IIS 和 ASP.NET Development Server (C#) 之间的差异
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_CS.zip)或[下载 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_cs.pdf)

> 当测试 ASP.NET 应用程序本地时，很有可能正在使用 ASP.NET 开发 Web 服务器。 但是，生产网站是最有可能已启动的 IIS。 有一些差异这些 web 服务器如何处理请求，并且这些差异可能会造成重要的后果。 本教程介绍了一些更 germane 差异。


## <a name="introduction"></a>介绍

每当的用户访问 ASP.NET 应用程序他浏览器会将请求发送到网站。 Web 服务器软件协调与 ASP.NET 运行时生成并返回所请求资源的内容将选取该请求。 [**我**nternet**我**璝**S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services)是一套常见基于 Internet 的为提供的功能的服务Windows 服务器。 IIS 是在生产环境中; 的 ASP.NET 应用程序的最常用的 web 服务器它很可能是由 web 宿主提供程序用来为 ASP.NET 应用程序提供服务的 web 服务器软件。 IIS 还可为 web 服务器软件在开发环境中，尽管这需要安装 IIS 并将其正确配置。


ASP.NET 开发服务器是用于开发环境中; 一个备用的 web 服务器选项它附带并集成到 Visual Studio。 除非已配置 web 应用程序为使用 IIS，ASP.NET 开发服务器是自动启动，并用作 web 服务器访问网页上从 Visual Studio 中的第一个时间。 我们返回在中创建的演示 web 应用程序[*确定什么文件需要部署到*](determining-what-files-need-to-be-deployed-cs.md)教程已未配置为使用 IIS 这两个文件基于系统的 web 应用程序。 因此，在访问这些从 Visual Studio 中的网站之一时将使用的 ASP.NET 开发服务器。

在理想的环境中，将相同开发和生产环境。 但是，如我们前面教程中所述不少见的环境为具有不同的配置设置。 在环境中使用另一个 web 服务器软件会在部署应用程序时必须考虑的另一个变量。 本教程介绍 IIS 和 ASP.NET 开发服务器之间的主要区别。 由于这些差异有一些的情形其中完美地在开发环境中运行的代码引发异常，或在生产环境中执行时的行为有所不同。

## <a name="security-context-differences"></a>安全上下文差异

每当 web 服务器软件处理的传入请求时，它将与特定的安全上下文关联该请求。 此安全上下文信息用于由操作系统确定什么动作允许该请求。 例如，ASP.NET 页可能包括将某些消息记录到磁盘上的文件的代码。 为了使此 ASP.NET 页后，可以执行而未出现错误，安全上下文必须具有相应的文件系统级权限，即写入该文件上的权限。

ASP.NET Development Server 将传入请求与当前登录用户的安全上下文相关联。 如果你登录到你的桌面以管理员身份，然后由 ASP.NET 开发服务器提供服务的 ASP.NET 页需要以管理员身份的相同访问权限。 但是，由 IIS 处理 ASP.NET 请求都与特定的计算机帐户关联。 默认情况下，网络服务计算机帐户使用 iis 版本 6 和 7，虽然你的 web 宿主提供程序可能已配置的唯一帐户，每个客户。 更重要的是，web 宿主提供程序可能已授予此计算机帐户的受限的权限。 最终结果是，您可能必须执行正常情况下，在开发环境中，但在生产环境中生成与授权相关的异常时托管的网页。

要在操作中显示此类型的错误我已在人创建存储的最新的日期和时间的磁盘上的文件的图书评论网站中创建页面查看*教授自己 ASP.NET 3.5 24 小时内*查看。 若要执行此操作，打开`~/Tech/TYASP35.aspx`页上，添加以下代码`Page_Load`事件处理程序：

[!code-csharp[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample1.cs)]

> [!NOTE]
> [ `File.WriteAllText`方法](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx)创建一个新文件，如果它不存在，然后向其中写入指定的内容。 如果该文件已存在，则会覆盖现有内容。


接下来，请访问*教授自己 ASP.NET 3.5 24 小时内*簿审阅页面上，在使用 ASP.NET 开发服务器开发环境中的。 假设你登录到你的计算机使用的帐户具有足够的权限来创建和修改的文本文件中 web 应用程序的根目录下的书籍评审出现之前，相同，但该页是每次访问日期和时间以及用户的 IP 地址存储在`LastTYASP35Access.txt`文件。 你的浏览器指向此文件;你应看到类似于图 1 中所示的消息。


[![文本文件包含的最后一个日期和时间簿评审的访问](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image1.png)

**图 1**： 文本文件包含的最后一个日期和时间簿评审的访问 ([单击以查看实际尺寸的图像](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image3.png))


部署到生产 web 应用程序，然后访问托管*教授自己 ASP.NET 3.5 24 小时内*册审阅页面。 此时可以看到册查看页面为普通或图 2 所示的错误消息。 某些 web 主机提供程序允许匿名的 ASP.NET 计算机帐户，将在其中用例页上工作正常的写入权限。 如果你的 web 主机提供商但是，禁止匿名帐户的写入访问权限则[`UnauthorizedAccessException`异常](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx)时引发`TYASP35.aspx`页尝试将写入当前日期和时间`LastTYASP35Access.txt`文件。


[![IIS 使用的默认计算机帐户没有权限来写入到文件系统](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image4.png)

**图 2**: 默认计算机帐户由 IIS 不具有的权限为写入到文件系统 ([单击以查看实际尺寸的图像](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image6.png))


好消息是大多数 web 主机提供具有某种形式的权限工具，可在你的网站中指定文件系统权限。 授予匿名 ASP.NET 帐户写入访问权限的根目录，然后重新审视册审阅页面。 （如果需要请联系你的 web 宿主提供程序获取帮助如何授予写入权限的默认 ASP.NET 帐户。）此页面应加载而未出现错误的时间和`LastTYASP35Access.txt`应成功创建文件。

Take 掉下面是 IIS 比不同的安全上下文下运行 ASP.NET 开发服务器，因为它可能会 ASP.NET 页面读取或写入文件系统中，从读取或写入到 Windows 事件日志中，或读取或写入到 Windows 注册表将按预期方式上开发工作，但生成在生产环境时的异常。 生成的 web 应用将部署到的共享 web 承载环境中，不可读取或写入事件日志或 Windows 注册表。 此外请注意的任何读取或写入文件系统，因为你可能需要授予读取和写入相应的文件夹的权限在生产环境中的 ASP.NET 网页。

## <a name="differences-on-serving-static-content"></a>在提供静态内容的差异

IIS 和 ASP.NET 开发服务器之间的另一个核心区别是处理对静态内容的请求的方式。 是否为 ASP.NET 页、 一个映像或一个 JavaScript 文件，提供到 ASP.NET 开发服务器时，每个请求由 ASP.NET 运行时处理。 默认情况下，IIS 仅调用 ASP.NET 运行时的一个 ASP.NET 资源，如 ASP.NET 网页、 Web 服务等的请求到达时。 IIS 而不需要参与 ASP.NET 运行时检索的静态内容-映像，CSS 文件、 JavaScript 文件、 PDF 文件、 ZIP 文件和类似的请求。 （它可以指示 IIS 以使用 ASP.NET 运行时提供静态内容; 在本教程针对的详细信息，请查阅的"执行基于窗体的身份验证和具有 IIS 7 的静态文件的 URL 身份验证"部分。）

ASP.NET 运行时执行若干步骤来生成请求的内容，包括 （标识请求者） 的身份验证和授权 （确定如果请求程序有权查看请求的内容）。 常用窗体的身份验证是*基于窗体的身份验证*，在其用户标识可通过在网页上文本框中输入其凭据，通常的用户名和密码。 网站存储时验证他们的凭据，*身份验证票证*上用户的浏览器中，也不能随每个后续请求一起发送到网站什么是用于对用户进行身份验证 cookie。 此外，也可以指定*URL 授权*规则，规定哪些用户可以或无法访问特定文件夹。 很多 ASP.NET 网站中使用基于窗体的身份验证和 URL 授权，以支持用户帐户并定义才经过身份验证的用户或属于某一特定角色的用户访问的站点的部分。

> [!NOTE]
> 用于 ASP 全面检查。NET 的基于窗体的身份验证、 URL 授权和其他用户帐户相关的功能，请务必了解我[网站安全教程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。


请考虑支持使用基于窗体的授权的用户帐户，并且拥有的文件夹，使用 URL 授权，配置为仅允许经过身份验证的用户的网站。 假设此文件夹包含 ASP.NET 页面和 PDF 文件和其目的是，仅身份验证的用户可以查看这些 PDF 文件。

如果访问者尝试通过直接在其浏览器的地址栏中输入 URL 来查看这些 PDF 文件之一，会发生什么情况？ 若要了解，让我们在图书评论站点中创建一个新文件夹、 添加某些 PDF 文件，以及配置站点以使用 URL 授权要禁止匿名用户访问此文件夹。 如果你下载演示应用程序，你将看到我创建一个名为文件夹`PrivateDocs`和添加从 PDF 我[网站安全教程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)（如何最合适 ！）。 `PrivateDocs`文件夹还包含`Web.config`指定 URL 授权规则，以拒绝匿名用户的文件：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample2.xml)]

最后，我配置 web 应用程序以使用基于窗体的身份验证通过更新`Web.config`的根目录中的文件替换：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample3.xml)]

替换为：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample4.xml)]

使用 ASP.NET 开发服务器，请访问站点和直接 URL 输入到你的浏览器地址栏中的 PDF 文件之一。 如果你下载相关 URL 应类似于本教程的网站：`http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

地址栏中输入此 URL 导致浏览器将请求发送到该文件的 ASP.NET 开发服务器。 关闭对 ASP.NET 运行时进行处理的请求的 ASP.NET Development Server 手中。 由于我们尚未登录，并且`Web.config`中`PrivateDocs`文件夹配置为拒绝匿名访问，ASP.NET 运行时自动将重定向我们到登录页上， `Login.aspx` （请参见图 3）。 当将用户重定向到登录页，包括 ASP.NET`ReturnUrl`用户尝试查看查询字符串参数指示的页。 在用户成功登录后可以返回到此页。


[![未经授权的用户将自动重定向到登录页](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image7.png)

**图 3**： 未经授权的用户将自动重定向到登录页 ([单击以查看实际尺寸的图像](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image9.png))


现在我们来看看这在生产上的行为方式。 部署应用程序和直接 URL 输入到一个在 Pdf`PrivateDocs`在生产环境中的文件夹。 这将提示你的浏览器发送的文件的 IIS 请求。 因为请求的静态文件时，IIS 检索，并且不调用 ASP.NET 运行时返回该文件。 因此，执行; 没有 URL 授权检查应该私有 PDF 内容都知道到文件的直接 URL 的任何人可以访问。


[![匿名用户可以通过输入与该文件的直接 URL 下载私有 PDF 文件](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image10.png)

**图 4**： 匿名用户可以下载私有 PDF 文件通过输入直接 URL 到文件 ([单击以查看实际尺寸的图像](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>在带有 IIS 7 的静态文件上执行基于窗体的身份验证和 URL 身份验证

有几个可以用来防止未经授权的用户的静态内容的方法。 IIS 7 引入*集成的管道*，其中结婚与 ASP.NET 运行时的工作流的 IIS 的工作流。 简而言之，你可以指示 IIS 来调用 ASP.NET 运行时的身份验证和授权模块 （包括之类的 PDF 文件的静态内容） 的所有传入请求。 联系你的 web 主机提供商，若要了解如何配置你的网站以使用集成的管道。

后配置 IIS 以使用集成的管道将添加到以下标记`Web.config`的根目录中的文件：

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample5.xml)]

此标记指示 IIS 7 以使用基于 ASP.NET 的身份验证和授权模块。 重新部署你的应用程序，然后重新访问 PDF 文件。 当 IIS 处理请求这次它，ASP.NET 运行时的身份验证和授权逻辑能够检查请求。 因为只有经过身份验证的用户有权查看中的内容`PrivateDocs`文件夹，匿名访问者将自动重定向到登录页 （回头参考图 3 中）。

> [!NOTE]
> 如果你的 web 宿主提供程序仍在使用 IIS 6，则无法使用集成的管道功能。 一种解决办法是将私有文档放入禁止 HTTP 访问的文件夹 (如`App_Data`)，然后创建页后，可以提供这些文档。 此页可能会调用`GetPDF.aspx`，并传递 PDF 通过查询字符串参数的名称。 `GetPDF.aspx`页将首先验证用户有权查看该文件并且，如果是这样，将使用[ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx)方法以返回到请求的客户端发送请求的 PDF 文件的内容。 如果你不希望启用集成的管道，这种技术也将适用于 IIS 7。


## <a name="summary"></a>摘要

使用 Microsoft 的 IIS web 服务器软件承载在生产环境中的 web 应用程序。 在开发环境中，但是，应用程序可能会使用承载 IIS 或 ASP.NET 开发服务器。 理想情况下，相同的 web 服务器软件应能在这两个环境因为使用不同的软件的组合中添加另一个变量。 但是，使用 ASP.NET 开发服务器的易用性使其成为不错的选择在开发环境中。 好消息是有仅几个基本差异 IIS 和 ASP.NET 开发服务器之间，并且如果你是注意这些区别你可以采取措施以帮助确保应用程序工作原理和函数相同的方式而不考虑环境。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [与 IIS 7.0 的 ASP.NET 集成](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [对所有类型的内容在 IIS 7 上使用 ASP.NET 论坛身份验证](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx)（视频）
- [Visual Web Developer 中的 web 服务器](https://msdn.microsoft.com/library/58wxa9w5.aspx)

>[!div class="step-by-step"]
[上一页](common-configuration-differences-between-development-and-production-cs.md)
[下一页](deploying-a-database-cs.md)
