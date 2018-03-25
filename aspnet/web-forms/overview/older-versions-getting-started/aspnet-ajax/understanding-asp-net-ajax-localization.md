---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: "了解 ASP.NET AJAX 本地化 |Microsoft 文档"
author: scottcate
description: "本地化是设计并将对特定的语言和区域性的支持集成到应用程序或应用程序组件的过程。 Mic..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 5b801586ea77af78284f780fe47fe09cafb984af
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-localization"></a>了解 ASP.NET AJAX 本地化
====================
通过[Scott 类别](https://github.com/scottcate)

[下载 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> 本地化是设计并将对特定的语言和区域性的支持集成到应用程序或应用程序组件的过程。 Microsoft ASP.NET 平台提供广泛支持用于本地化为标准的 ASP.NET 应用程序通过集成标准.NET 本地化模型;Microsoft AJAX Framework 利用集成的模型，以支持将在其中执行本地化的各种方案。


## <a name="introduction"></a>介绍

Microsoft ASP.NET 技术将的面向对象及事件驱动的编程模型，并将它具有已编译代码的好处结合在一起。 但是，其服务器端处理模型具有很多这样的可以通过封装.NET Framework 中的 Microsoft AJAX 服务的 system.web.extensions 的引用命名空间中包括的新功能进行寻址的技术中的固有的几个缺点3.5。 这些扩展使许多丰富的客户端功能，以前可为属于 ASP.NET 2.0 AJAX Extensions 中，但现在的 Framework 基类库的一部分。 控件和此命名空间中的功能而无需完整的页面刷新，能够访问 Web 服务通过客户端 （包括 ASP.NET 分析 API） 脚本，包括部分呈现页上，提供广泛的客户端 API 用于镜像的许多ASP.NET 服务器端控件集中所示控制方案。

本白皮书检查中的 Microsoft AJAX Framework 和 Microsoft AJAX 脚本库，本地化支持并检查已集成支持 web 的本地化的业务需求的上下文中提供的本地化功能提供的.NET Framework 的应用程序。 Microsoft AJAX 脚本库利用.resx 文件格式已由.NET 应用程序，它提供集成的 IDE 支持和可共享的资源类型。

基于本白皮书在 Microsoft Visual Studio 2008 Beta 2 版本上。 此白皮书还假定你将使用 Visual Studio 2008 中，不 Visual Web Developer Express，并且将提供根据 Visual Studio 的用户界面的演练。 一些代码示例将利用可能 Visual Web Developer 学习版中不可用的项目模板。

## <a name="the-need-for-localization"></a>*对本地化的需求*

特别是对于企业应用程序开发人员，组件开发人员能够创建可以识别区域文化和语言之间的差异的工具已变得越来越必要。 设计组件能够适应客户端的区域设置提高开发人员工作效率，缩短全局函数的一个组件适应所需的工作量。

本地化是设计并将对特定的语言和区域性的支持集成到应用程序或应用程序组件的过程。 Microsoft ASP.NET 平台提供广泛支持用于本地化为标准的 ASP.NET 应用程序通过集成标准.NET 本地化模型;Microsoft AJAX Framework 利用集成的模型，以支持将在其中执行本地化的各种方案。 通过正在部署到附属程序集，或通过利用静态文件系统结构来，也可以借助 Microsoft AJAX 框架，本地化脚本。

## <a name="embedding-scripts-with-satellite-assemblies"></a>*嵌入附属程序集使用的脚本*

与标准的.NET Framework 本地化策略一致，资源可以包含在附属程序集。 附属程序集具有多项优点通过传统的资源包含在二进制文件的任何给定的本地化可以更新而更新更大的映像，可以只需通过安装到的附属程序集部署其他本地化信息而不会导致主项目程序集重新加载，可以部署的项目文件夹和附属程序集。 尤其是在 ASP.NET 项目，这将是有益的因为它可以显著减少所使用的增量更新的系统资源量，并按最小方式会中断生产网站使用情况。

脚本嵌入到程序集，它们包含在托管.resx （或编译.resources） 文件，都将包括在编译时程序集。 然后，其资源可用于通过 AJAX 运行时生成代码，程序集级别属性通过脚本应用程序

*嵌入的脚本文件的命名约定*

Microsoft AJAX 框架脚本管理支持多种选项用于部署和测试脚本，并且提供了准则，以便这些选项。

*为了便于调试：*

版本 （生产） 脚本不应包含`.debug`文件名中的限定符。 脚本调试应包括用于`.debug`通配符 * 和？。

*为了便于本地化：*

非特定区域性脚本不应包含任何区域性标识符的文件的名称。 对于包含本地化的资源的脚本，应在文件名中指定的 ISO 语言代码。 例如，`es-CO`西班牙语，哥伦比亚代表。

下表总结了文件命名约定的示例：

| Filename | 含义 |
| --- | --- |
| Script.js | 一个发行版本的非特定于区域性的脚本。 |
| Script.debug.js | 一个调试版本的非特定于区域性的脚本。 |
| Script.en US.js | 发行版适用于英语，美国脚本。 |
| Script.debug.es CO.js | 调试版本西班牙语，哥伦比亚脚本。 |

## <a name="walkthrough-create-an-localized-embedded-script"></a>演练： 创建本地化、 嵌入的脚本

*请注意： 本演练需要 Visual Studio 2008 的使用，因为 Visual Web Developer Express 不包括用于类库项目的项目模板。*

1. 使用集成的 ASP.NET AJAX Extensions 中创建新的网站项目。 创建另一个项目，一个类库项目，调用 LocalizingResources 解决方案中。
2. 添加到 LocalizingResources 项目中，调用 VerifyDeletion.js Jscript 文件以及称为 DeletionResources.resx 和 DeletionResources.es.resx.resx 资源文件。 前者将包含非特定于区域性的资源;后者将包含西班牙语语言资源。
3. 将以下代码添加到 VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

对于熟悉 JavaScript 正则表达式语法中，单个正斜杠中的文本 （在前面的示例中，/FILENAME/ 是一个例子） 表示 RegExp 对象。 MSDN 库中包含大量的 JavaScript 参考，并在 JavaScript 本机对象上的资源可找到联机。

1. 将下面的资源字符串添加到 DeletionResources.resx: 

    **VerifyDelete**： 是否确实要删除文件名？

    **删除**: FILENAME 已被删除。

1. 将下面的资源字符串添加到 DeletionResources.es.resx: 

    **VerifyDelete**： 东部 seguro 汉阳 desee quitar FILENAME？

    **删除**: FILENAME se ha quitado。
2. 将以下代码行添加到 AssemblyInfo 文件：

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. 向 LocalizingResources 项目中添加对 System.Web 和 System.Web.Extensions 的引用。
2. 添加到 LocalizingResources 项目中的网站项目的引用。
3. 在 default.aspx 中，在网站项目下，使用以下其他标记更新 ScriptManager 控件：

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. 在 default.aspx 中，任何位置的页上，包括此标记：

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. 按 F5。 如果系统提示，请启用调试。 当加载页时，按删除按钮。 请注意，你会提示您在英语中 （除非默认情况下，你的计算机设置首选西班牙语语言资源） 进行确认。
2. 关闭浏览器窗口，并返回到 default.aspx。 在@Page标头指令，与 ES-ES 区域性和 UICulture 替换自动。 再次按 F5 以启动 web 浏览器中一次应用程序。 此时，请注意，提示您删除西班牙语文件：


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([单击以查看实际尺寸的图像](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([单击以查看实际尺寸的图像](understanding-asp-net-ajax-localization/_static/image6.png))


请注意，对于本演练的几种变体。 例如，脚本不能注册与 ScriptManager 控件以编程方式在页面加载过程。

## <a name="including-a-static-script-file-structure"></a>*包括静态脚本文件结构*

当使用静态的脚本文件进行部署，你丢失部分使用的固有的.NET 本地化方案的好处。 主要可见，则为，则会失去自动从包括脚本资源文件; 生成的类型在上面的演练中，例如，资源已公开的消息调用从 ScriptManager 控件自动生成类型。

有，但是，某些优点使用静态的脚本文件结构。 无需重新编译和重新部署附属程序集，就可以执行更新和静态文件结构的使用也可以重写嵌入的脚本，以将一个次要可能尚未发运的功能集成的组件。

Microsoft 建议避免通过自动在项目编译期间生成你的脚本资源的版本控制问题。 当维护大量的脚本代码基，它可能会变得越来越困难，以确保代码更改都会反映在每个本地化的脚本。 作为替代方法，则可以只需保持一个逻辑脚本和多个本地化脚本，在生成项目时合并文件。

由于不是资源，以声明方式包括，静态文件应为的脚本引用通过添加`<asp:ScriptElement>`元素的子级作为`<Scripts>`标记 ScriptManager 控件，或通过以编程方式添加`ScriptReference`对象到`Scripts`属性`ScriptManager`在运行时页面上的控件。

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager 和本地化中的其角色*

ScriptManager 使本地化应用程序的多个自动行为：

- 它将自动定位基于设置和命名约定; 的脚本文件例如，它将加载在调试模式下，启用了调试的脚本并加载本地化基于浏览器的用户界面选择的脚本。
- 它使您能够定义区域性，包括自定义区域性。
- 它允许 HTTP 压缩的脚本文件。
- 它将缓存脚本来高效地管理多请求。
- 它将一个间接层添加到脚本中，其由管道它们通过加密的 URL。

以编程方式或通过声明性的标记，则可以将脚本引用添加到 ScriptManager 控件。 声明性的标记时特别有用在使用脚本中嵌入的程序集，而不是网站项目本身，如脚本的名称可能不会发生更改修订推送。

## <a name="summary"></a>摘要

随着 web 应用程序的增长来访问更大的受众，需要能够访问更广泛的区域性和社区变得内核数与业务模型电子商务 web 应用程序需要能够处理外货币，需要为其内容，但还其导航提示并在其他语言和公司的窗体字段需要知道这一需要是不仅可以存在内容管理系统可访问。

.NET Framework 本质上支持丰富的本地化框架中，利用附属程序集和 XML 资源 (.resx) 文件提供统一的方式来查找资源字符串和图像。 ASP.NET AJAX Extensions 中，包括 Microsoft AJAX Framework 和 Microsoft AJAX 脚本库，提供支持对此编程模型到客户端代码中，启用简单的资源字符串查找。 附属程序集支持自动包含通过 ScriptResource.axd 的脚本资源 （实际的.js 文件），只要文件名遵守给定的命名方案。 利用此支持，使用 ASP.NET AJAX Extensions 简化脚本的本地化和全球化应用程序。

## <a name="bio"></a>*简介*

Scott 类别自 1997 年以来处理与 Microsoft Web 技术，并且是 myKB.com 总裁 ([www.myKB.com](http://www.myKB.com)) 其中他专注于编写 ASP.NET 基于侧重于知识库软件解决方案的应用程序。 可以通过在电子邮件联系 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在其博客地址[ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[上一页](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
[下一页](understanding-asp-net-ajax-web-services.md)
