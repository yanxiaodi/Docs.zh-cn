---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: "ASP.NET 网页 (Razor) 常见问题 |Microsoft 文档"
author: tfitzmac
description: "本文列出了一些有关 ASP.NET Web 页 (Razor) 和 WebMatrix 的常见问题。 使用在教程的 ASP.NET Web Pages （.的软件版本"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 60cc4ca364923cb131d5e91cd7b6307b1e68644b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET 网页 (Razor) 常见问题
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix 是不再建议将其作为集成的开发环境的 ASP.NET Web 页。 使用[Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio)或[Visual Studio Code](https://code.visualstudio.com/)。
>
> 本文列出了一些有关 ASP.NET Web 页 (Razor) 和 WebMatrix 的常见问题。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2、 WebMatrix 2 和 Visual Studio 2012。


- [ASP.NET 网页、 ASP.NET Web 窗体和 ASP.NET MVC 之间的区别是什么？](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [若要使用网页是否需要 WebMatrix？](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [可以使用网页页面上的 ASP.NET Web 窗体控件？](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [可以将 ASP.NET Web Pages 站点部署而无需使用 WebMatrix](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [我是否必须使用 WebSecurity 帮助器以支持登录名？](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [ASP.NET 网页是否支持 HTML5？](#Does_ASP.NET_Web_Pages_support_HTML5)
- [可以将 JavaScript 和 jQuery 用于 Web 页？](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [其他资源](#AdditionalResources)

有关错误和其他问题的问题，请参阅[ASP.NET Web 页 (Razor) 故障排除指南 》](https://go.microsoft.com/fwlink/?LinkId=253001)。

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>ASP.NET 网页、 ASP.NET Web 窗体和 ASP.NET MVC 之间的区别是什么？

这三个用于创建动态 web 应用程序的 ASP.NET 技术：

- ASP.NET Web 页侧重于将动态 （服务器端） 代码和数据库访问权限添加到 HTML 页面和功能的简单和轻量语法。
- ASP.NET Web 窗体取决于页的对象模型和传统的窗口类型控件 （按钮、 列表等）。 Web 窗体使用基于事件的模型为那些使用基于客户端 （Windows 窗体） 开发过熟悉。
- ASP.NET MVC 为 ASP.NET 实现模型-视图-控制器模式。 重点是"分离问题"（处理、 数据和 UI 层）。

所有三个框架完全支持，并且继续由 ASP.NET 团队开发。 一般情况下，要使用哪个 framework 的选择取决于您的背景和 ASP.NET 经验。

ASP.NET 网页尤其旨在简化已经知道 HTML 中以将服务器处理添加到相应的页面的人员。 一般情况下人员不熟悉如何编程人员学生，热衷于，它是一个不错的选择。 也可以是一个不错的选择的开发人员有非 ASP.NET web 技术的经验。

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>若要使用网页是否需要 WebMatrix？

不是。 WebMatrix 是不再建议将其作为集成的开发环境的 ASP.NET Web 页。 使用[Visual Studio](program-asp-net-web-pages-in-visual-studio.md)或[Visual Studio Code](https://code.visualstudio.com/)。

如果你不想要使用 Visual Studio 或 Visual Studio 代码，则可以安装使用单独的组件产品[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)。 你需要以下产品：

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 （它会安装的 ASP.NET Web Pages framework）
- IIS Express （web 服务器）
- Microsoft SQL Server Compact 4.0 （数据库）

你可以使用文本编辑器编辑*.cshtml* (或*.vbhtml*) 页。

管理 SQL Server Compact 数据库 (*.sdf*文件) 不是有点困难的工具。 用于管理的 visual Studio containds 工具*.sdf*数据库。 此外可以在代码中执行许多 SQL Server 管理任务来运行 SQL 命令。

若要测试*.cshtml*页，而不使用的集成的开发环境 (IDE)，你可以将它们部署到实时的服务器。 (请参阅[可以将 ASP.NET Web Pages 站点部署而无需使用 WebMatrix？](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>正在运行的 IIS Express，而无需使用一个 IDE

如果在你的 web 服务器的计算机上安装 IIS Express，你可以使用，来测试页面。 你可以从命令行运行 IIS Express，并将其与特定的端口号。 请求时然后指定该端口*.cshtml*在浏览器中的文件。

在 Windows 中，使用管理员特权打开命令提示符，并将更改为*C:\Program Files\IIS Express。* (对于 64 位系统，使用文件夹*C:\Program Files (x86) \IIS Express。)*然后输入以下命令，你的站点使用的实际路径：

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

你可以使用任何不通过某些其他进程已保留的端口号。 （1024年以上的端口号是通常免费的。）有关`path`值，请使用网站文件夹的路径其中*.cshtml*文件。

运行此命令以设置 IIS Express 以服务页面后，你可以打开浏览器并浏览到*.cshtml*文件。 使用类似于以下 URL:

`http://localhost:35896/default.cshtml`

对于 IIS Express 命令行选项的帮助，请输入`iisexpress.exe /?`在命令行。

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>可以使用网页页面上的 ASP.NET Web 窗体控件？

不是。 Web 窗体控件，例如[复选框](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox)控件，[验证控件](https://msdn.microsoft.com/library/bwd43d0x)，和[GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview)控制仅在 Web 窗体页中的工作 (*.aspx*文件)。 这些控件需要 Web 窗体页框架。

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>可以将 ASP.NET Web Pages 站点部署而无需使用 WebMatrix

可以。 你可以手动网站文件复制到一台服务器 （通常通过使用 FTP）。 如果你执行手动副本时，你还必须将复制支持 SQL Server Compact （数据库） 的文件。 有关详细信息，请参阅博客文章[部署网页没有一种工具的应用程序](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317)。

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>我是否必须使用 WebSecurity 帮助器以支持登录名？

不是。 `SimpleMembership`提供程序的一部分的 ASP.NET Web 页是一个选项。 此外，还提供属于 ASP.NET （即你可能会习惯于在 Web 窗体中使用） 的安全提供程序。 例如，你可以使用窗体身份验证的 ASP.NET Web Pages 中就像在 Web 窗体中。 有关如何使用的一个示例窗体身份验证，请参阅 Microsoft 支持文章[How To Implement Forms-Based 身份验证在 ASP.NET 应用程序通过使用 C#.NET](https://support.microsoft.com/kb/301240)。 若要下载一个简单的示例，请参阅[ASP.NET 版本的"登录名&amp;密码](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm)。

有关如何使用 Windows 身份验证的信息，请参阅博客文章[使用 Windows 身份验证中的 ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298)。

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>ASP.NET 网页是否支持 HTML5？

可以。 创建与 ASP.NET Web 页的页 (*.cshtml*或*.vbhtml*页) 是实质上是 HTML 页面，其中还包含在呈现页之前，在服务器上，运行的代码。 只要用户的浏览器支持 HTML5，你可以使用中的 HTML5 元素*.cshtml*或*.vbhtml*页。

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>可以将 JavaScript 和 jQuery 用于 Web 页？

当然可以。 创建与 ASP.NET Web 页的页 (*.cshtml*或*.vbhtml*页) 只需用服务器代码中的 HTML 页。 因此，你可以使用执行操作在普通的 HTML 页面通过 JavaScript 或 jQuery 的任何内容还可以执行*.cshtml*或*.vbhtml*页。

**入门站点**模板在 WebMatrix 中包含大量的 jQuery 库。 如果你使用该模板，创建一个站点*脚本*文件夹包含 jQuery 核心库 (*jquery 1.6.2.js)*和 jQuery 验证库 (*jquery.validate.js 中定义*等。)。

下面是一些说明了使用 jQuery 与 ASP.NET Web 页的方法的博客文章：

- [添加到 ASP.NET Web Pages 使用 WebMatrix 的 jQuery 带来好处](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/)通过 Rachel Appel
- [5 分钟： WebMatrix + jQuery UI + json + jQuery 模板](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/)通过 Jonas Eriksson
- [WebMatrix 和 jQuery 窗体](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms)由 Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>其他资源


[ASP.NET 网页 (Razor) 疑难解答指南](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix 和 ASP.NET Web Pages 论坛](https://forums.asp.net/1224.aspx/1?WebMatrix)ASP.NET 网站上
