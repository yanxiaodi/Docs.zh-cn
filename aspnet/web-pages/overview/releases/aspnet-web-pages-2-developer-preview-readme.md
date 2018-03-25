---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: "ASP.NET Web 页 2 开发人员预览自述文件 |Microsoft 文档"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 119265c62abb3f3110cdc7f0b94a7c9b16b4251c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 开发人员预览自述文件
====================
通过[Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 开发人员预览自述文件

2011 年 9 月 14日

### <a name="contents"></a>内容

#### <a id="_Toc303701284"></a>安装说明

若要安装 Web Pages 2 开发者预览版，你具有以下选项：

- 安装 WebMatrix 2 Beta 使用[Web 平台安装程序](https://go.microsoft.com/fwlink/?LinkId=226883)。 WebMatrix 是一组可用的 web 开发工具，包括 ASP.NET 网页。 有关详细信息，请参阅中的安装部分[顶部功能在 ASP.NET 网页 2 开发者预览版](https://go.microsoft.com/fwlink/?LinkID=227824)。

- 安装 Web Pages 2 开发者预览版直接使用[下载链接](https://go.microsoft.com/fwlink/?LinkID=226335)。 如果你想要创建网页应用程序使用文本编辑器 （如记事本），请使用此方法。 若要运行网页 2 应用程序，您必须 IIS Express 7.5。 （这是自动附带 WebMatrix。有关如何使用 IIS Express，对网页页进行测试提示，请参阅在侧栏"创建并测试 ASP.NET 页使用你自己的文本编辑器" [Getting Started with WebMatrix 和 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889)。

所需的 ASP.NET Web Pages 2 开发者预览版可以安装，并可以运行的并行与 ASP.NET Web Pages 1。 <a id="a"></a>有关详细信息，参阅的部分"运行 Web 页面应用程序的并行"[网页 2 开发者预览版中的顶部功能](https://go.microsoft.com/fwlink/?LinkID=227824)。

#### <a id="_Toc303701285"></a>文档

教程和其他信息有关的 ASP.NET Web Pages 页还提供网页的 ASP.NET 网站 ([https://www.asp.net/web-pages/](../../index.md))。 有关新功能和增强功能网页 2 中的信息，请参阅[网页 2 开发者预览版中的顶部功能](https://go.microsoft.com/fwlink/?LinkID=227824)。

#### <a id="_Toc303701286"></a>支持

<a id="_Toc209852135"></a><a id="_Toc255833657"></a>这是预览版本，未正式受到支持。 如果你有关于使用此版本的问题，则将其发布到 ASP.NET Web Pages 论坛 ([https://forums.asp.net/1224.aspx/1?WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) )，其中 ASP.NET 社区的成员都是经常能够提供非正式的支持。

#### <a id="_Toc303701287"></a>软件要求

ASP.NET Web Pages 2 要求.NET Framework 4。 它还适用于.NET Framework 4.5 开发者预览版版本。

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>修补程序、 已知的问题和重大更改

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **是\*适用于所有区域性的方法 (例如，IsDateTime) 现在返回正确值。** 等某些方法*IsDateTime*以前返回*false*时它们应具有返回*true*因为它们以前已执行特定于区域性的检查。 已修复这些方法现在将区域性考虑在内。 这是一项重大更改;如果你的应用程序依赖于这一旧行为，它将中断。
- **Href 方法的行为已更改。** 以前，调用 Href("~/SomeFile") 将返回相对于当前正在执行的文件的 URL。 现在 Href("~/SomeFile") 始终返回绝对路径从应用程序的根目录。 对于大多数情况下，此行为不会带来返回值中的差异。 进行此更改将解决某些 Ajax 方案。 例如，考虑下面的代码示例： 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    此代码之前将解析为 Images/Logo.jpg，这将会是为到该页面 Ajax 请求不正确。 它现在会解析到的根 (/ MySite/Images/Logo.jpg)。
- **HttpContext.RedirectLocal 方法已更改**。 此方法现在接受向相对于当前应用程序的 Url。 完全限定的 Url 会被拒绝。
- **ModelState.IsValid 方法现在要求您首先调用验证**。 如果你要将转换应用程序以使用新的输入的验证方法并调用*ModelState.IsValid*方法，现在，您必须调用*Validation.Validate*事先。 例如，你现在必须遵循以下模式： 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

 但是，我们建议，如果你使用新的输入的验证方法，不要使用*ModelState.IsValid*。 相反，构建如下代码： 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **在 Internet Explorer 7 和 Internet Explorer 8 上，客户端验证不起作用**。 客户端验证不工作，因为不兼容内容 jQuery 1.6.2，这是随附的默认项目模板。 （服务器端验证的工作方式。）。

#### <a id="_Toc303701289"></a>免责声明

© 2011 Microsoft Corporation。 保留所有权利。 本文档提供"作为-是。" 信息和包括 URL 和其他 Internet 网站引用，本文档中表达的观点可能更改恕不另行通知。 您自行承担其使用风险。
