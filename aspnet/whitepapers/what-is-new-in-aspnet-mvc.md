---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: "什么是 ASP.NET MVC 2 中的新增功能 |Microsoft 文档"
author: rick-anderson
description: "本文档介绍了新增功能和 ASP.NET MVC 2 中引入的改进。 本文档还是可供下载的。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/20/2010
ms.topic: article
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 808f51b48b31e21848d76e7ded436ca1b17901d2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="whats-new-in-aspnet-mvc-2"></a>什么是 ASP.NET MVC 2 中的新增功能
====================
> 本文档介绍了新增功能和 ASP.NET MVC 2 中引入的改进。 本文档还为提供[下载](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[简介](#_TOC1)   
[升级到 ASP.NET MVC 2 的 ASP.NET MVC 1.0 项目](#_TOC2)   
[新功能](#_TOC3)   
[模板化帮助器](#_TOC3_1)   
[区域](#_TOC3_2)   
[对异步控制器的支持](#_TOC3_3)   
[DefaultValueAttribute 操作方法参数中的支持](#_TOC3_4)   
[对绑定的二进制数据，模型联编程序的支持](#_TOC3_5)   
[ModelMetadata 和 ModelMetadataProvider 类](#_TOC3_6)   
[对 DataAnnotations 特性的支持](#_TOC3_7)   
[模型验证程序提供程序](#_TOC3_8)   
[客户端验证](#_TOC3_9)   
[Visual Studio 2010 的新代码段](#_TOC3_10)   
[新 RequireHttpsAttribute 操作筛选器](#_TOC3_11)   
[重写的 HTTP 方法谓词](#_TOC3_12)   
[模板化帮助器的新 HiddenInputAttribute 类](#_TOC3_13)   
[Html.ValidationSummary 帮助器方法可以显示模型级别错误](#_TOC3_14)   
[在 Visual Studio 生成的代码是特定的 T4 模板到.NET framework 目标版本](#_TOC3_15)[API 改进](#_TOC4)  
[重大更改](#_TOC5)  
[Disclaimer](#_TOC6)  

## <a id="_TOC1"></a>简介

ASP.NET MVC 2 基于 ASP.NET MVC 1.0，并引入了大量的增强功能和侧重于提高工作效率的功能。 此版本是与 ASP.NET MVC 1.0 兼容，因此所有知识、 技能、 代码和扩展 ASP.NET MVC 1.0 都继续应用。

有关 ASP.NET MVC 的详细信息，请访问以下资源：

- [MSDN 上的 ASP.NET MVC 文档](https://go.microsoft.com/fwlink/?LinkId=159758)
- [ASP.NET MVC 网站](https://asp.net/mvc/)
- [ASP.NET MVC 论坛](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>升级到 ASP.NET MVC 2 的 ASP.NET MVC 1.0 项目

ASP.NET MVC 2 可以在相同的服务器，从而使应用程序开发人员可以灵活地选择何时 ASP.NET MVC 1.0 应用程序升级到 ASP.NET MVC 2 上的与 ASP.NET MVC 1.0 并行安装。 有关如何升级的信息，请参阅文档[升级 ASP.NET MVC 1.0 应用程序到 ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459)。

## <a id="_TOC3"></a>新功能

本部分介绍已引入的功能在 MVC 2 版本中。

### <a id="_TOC3_1"></a>模板化帮助器

模板化帮助器让你自动相关联的 HTML 元素，以进行编辑，并显示与数据类型。 例如，当在视图中显示 System.DateTime 类型的数据时，日期选取器 UI 元素可以自动呈现。 这是类似于字段模板在 ASP.NET 动态数据中的工作方式。 有关详细信息，请参阅[使用模板化帮助器添加到显示数据](https://go.microsoft.com/fwlink/?LinkId=159062)MSDN 网站上。

### <a id="_TOC3_2"></a>区域

区域中，可以将大型项目组织到多个较小的部分，若要管理的大型 Web 应用程序的复杂性。 通常，每个部分 （"区域"） 表示大型网站的单独部分，并用于进行分组的控制器和视图的相关的集。 有关详细信息，请参阅[演练： 将 ASP.NET MVC 应用程序按区域组织](https://go.microsoft.com/fwlink/?LinkId=158978)MSDN 网站上。

若要创建新的区域中，在解决方案资源管理器中，右键单击项目，单击添加，然后单击区域。 此时将显示一个对话框，提示您输入区域名称。 输入的区域名称后，Visual Studio 向项目中添加一个新的区域。

下图显示具有两个区域，管理员和博客的项目的布局示例。

![](what-is-new-in-aspnet-mvc/_static/image1.png)

当你创建一个区域后时，Visual Studio 将添加到每个区域从 AreaRegistration 派生的类。 此类则需要执行注册的区域和其路由中，如下面的示例中所示：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

ASP.NET MVC 2 的默认项目模板包括对 RegisterAllAreas 方法的调用中的 Global.asax 文件的代码。 通过寻找从 AreaRegistration 类派生的所有类型、 实例化类型的实例，然后调用的实例上的 RegisterArea 方法还原时，此方法注册项目中的每个区域。 下面的示例演示如何做到这一点。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

如果你不通过调用上下文 RegisterArea 方法中指定命名空间。默认情况下使用 Namespaces.Add 方法，注册类的命名空间。

### <a id="_TOC3_3"></a>对异步控制器的支持

ASP.NET MVC 2 现在允许控制器以进行异步处理请求。 这可以允许频繁调用阻止操作 （例如网络请求） 以改为调用非阻塞对应的服务器，从而导致性能提升。 有关详细信息，请参阅[在 ASP.NET MVC 中使用异步控制器](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx)MSDN 上的主题。

### <a id="_TOC3_4"></a>DefaultValueAttribute 操作方法参数中的支持

System.ComponentModel.DefaultValueAttribute 类允许为操作方法的自变量参数提供的默认值。 例如，假定定义以下默认路由：

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

此外假定以下控制器和操作方法定义：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

以下请求的任何 Url 将调用在前面的示例中定义的视图操作方法。

- / 文章/视图/123
- / 文章/视图/123？ 页 = 1 （有效地与相同以前的请求）
- /Article/View/123?page=2

DefaultValueAttribute 属性中，从前面的列表的第一个 URL 就将无法工作，因为页自变量是不可为 null 的值类型未提供其值。

如果在 Visual Basic 2010 或 Visual C# 2010年中编写代码，你可以而不是 DefaultValueAttribute 属性中，使用可选参数，如下面的示例中所示：

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>对绑定的二进制数据，模型联编程序的支持

有两个新重载 Html.Hidden 帮助程序进行编码以 base 64 编码字符串形式的二进制值：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

典型用途是要在视图中嵌入对象的时间戳。 例如，你的应用程序可能包括以下产品对象：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

编辑窗体可以呈现的形式，如下面的示例中所示的时间戳属性：

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

此标记将时间戳值的隐藏输入的元素呈现为 base 64 编码字符串类似于下面的示例：

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

此窗体可能会转入有一个参数类型产品，操作方法，如下面的示例中所示：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

在操作方法中，原因是已发布的 base 64 编码字符串转换为字节数组正确填充的时间戳属性。

### <a id="_TOC3_6"></a>ModelMetadata 和 ModelMetadataProvider 类

ModelMetadataProvider 类提供了获取的视图中的模型的元数据的抽象。 MVC 2 包括默认提供程序，可以提供 System.ComponentModel.DataAnnotations 命名空间中的特性公开元数据。 它是可以创建提供来自其他数据存储，例如数据库或 XML 文件的元数据的元数据提供程序。

ViewDataDictionary 该类会公开一个包含由 ModelMetadataProvider 类从模型中提取的元数据的 ModelMetadata 对象。 这样的模板化帮助器来使用此元数据和相应地调整其输出。

有关详细信息，请参阅的文档[ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)和[ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)类。

### <a id="_TOC3_7"></a>对 DataAnnotations 特性的支持

ASP.NET MVC 2 支持使用 RangeAttribute、 RequiredAttribute、 StringLengthAttribute 和 RegexAttribute 验证属性 （在 System.ComponentModel.DataAnnotations 命名空间中定义） 时你将绑定到一个模型以便提供输入验证。

有关详细信息，请参阅[如何： 验证模型数据使用 DataAnnotations 属性](https://go.microsoft.com/fwlink/?LinkId=159063)MSDN 网站上。 演示如何使用这些属性的示例项目是可在下载[https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753)。

### <a id="_TOC3_8"></a>模型验证程序提供程序

模型验证提供程序类表示模型中提供验证逻辑的抽象。 ASP.NET MVC 包括默认提供程序基于 System.ComponentModel.DataAnnotations 命名空间中包含的验证属性。 你还可以创建自己的验证提供程序到模型中定义自定义验证规则和自定义映射的验证规则。 有关详细信息，请参阅的文档[ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx)类。

### <a id="_TOC3_9"></a>客户端验证

模型验证程序提供程序类公开可以由客户端验证库使用的 JSON 序列化数据的窗体中的浏览器的验证元数据。 ASP.NET MVC 2 包括客户端验证库和支持前文所述 DataAnnotations 命名空间验证特性的适配器。 提供程序类还使您能够通过编写处理 JSON 数据和到备用的库的调用的适配器使用其他客户端验证库。

### <a id="_TOC3_10"></a>Visual Studio 2010 的新代码段

使用 Visual Studio 2010 安装 ASP.NET MVC 2 的 HTML 代码段的一组。 若要在工具菜单中查看这些代码段的列表，选择代码片段管理器。 代表所选语言中，选择 HTML，然后对于位置，选择 ASP.NET MVC 2。 有关如何使用代码段的详细信息，请参阅 Visual Studio 文档。

### <a id="_TOC3_11"></a>新 RequireHttpsAttribute 操作筛选器

ASP.NET MVC 2 包括一个新的 RequireHttpsAttribute 类，可应用于操作方法和控制器。 默认情况下，筛选器将非 SSL HTTP 请求重定向为启用 SSL (HTTPS) 等效项。

### <a id="_TOC3_12"></a>重写的 HTTP 方法谓词

使用 REST 体系结构样式生成网站时，HTTP 谓词用于确定要为资源执行的操作。 REST 要求的完整范围的常见的 HTTP 谓词，包括 GET、 PUT、 POST 和删除该应用程序支持。

ASP.NET MVC 2 包括可以应用于操作方法和该功能简洁的语法的新属性。 这些属性使 ASP.NET MVC，若要选择基于 HTTP 谓词的操作方法。 在下面的示例中，POST 请求将调用的第一个操作方法和 PUT 请求将调用第二个操作方法。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

在早期版本的 ASP.NET MVC，这些操作所需方法更详细的语法，如下面的示例中所示：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

由于浏览器支持仅的 GET 和 POST HTTP 谓词，则不可以发布到需要不同的谓词的操作。 因此不可能以本机方式支持 rest 样式的所有请求。

但是，以支持基于 Rest 请求在 POST 期间，ASP.NET MVC 2 引入一个新的 HttpMethodOverride HTML 帮助器方法。 此方法呈现一个隐藏的输入的元素，使窗体来有效地模拟任何 HTTP 方法。 例如，通过使用 HttpMethodOverride HTML 帮助器方法，你可以显示的提交窗体是 PUT 或 DELETE 请求。 HttpMethodOverride 的行为会影响以下属性：

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

隐藏的输入的元素，并且在其名称 X HTTP 的方法重写其值设置为 HTTP 谓词来模拟。 替代值可以还在 HTTP 标头或指定查询字符串值作为名称/值对。

实际请求时 POST 请求，则仅可以使用替代。 使用任何其他 HTTP 谓词的请求将被忽略的替代值。

### <a id="_TOC3_13"></a>模板化帮助器的新 HiddenInputAttribute 类

你可以将新的 HiddenInputAttribute 特性应用于模型属性，以指示编辑器模板中显示该模型时是否应呈现隐藏的输入的元素。 （该属性设置 HiddenInput 隐式 UIHint 值）。 该特性的 DisplayValue 属性，可以指定是否在编辑器中显示值和显示模式。 当 DisplayValue 设置为 false 时，将不会显示，甚至不通常环绕字段的 HTML 标记。 DisplayValue 的默认值为 true。

在以下情况下，可以使用 HiddenInputAttribute 属性：

- 当视图可让用户编辑的对象 ID，并且有必要的值以及显示并提供一个隐藏的输入的元素，以便它可以传递到控制器包含旧的 ID。
- 当视图允许用户编辑应永远不会显示，例如时间戳属性的二进制属性。 在这种情况下，不显示的值和 （如 label 和值） 的周围 HTML 标记。

下面的示例演示如何使用 HiddenInputAttribute 类。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

如果将属性设置为 true （或未指定任何参数），将发生以下情况：

- 在显示模板中，标签将呈现在和的值显示给用户。
- 编辑器模板中呈现标签和值呈现中隐藏的输入元素。

当该属性设置为 false 时，发生以下情况：

- 显示模板中执行任何操作被呈现为该字段。
- 编辑器模板中没有标签将呈现在和中隐藏的输入元素呈现值。

### <a id="_TOC3_14"></a>Html.ValidationSummary 帮助器方法可以显示模型级别错误

而不是始终显示所有验证错误，Html.ValidationSummary 帮助器方法具有显示仅模型级别错误的新选项。 这使模型级别错误，以验证摘要中显示和特定于字段的错误，以显示每个字段旁边。

### <a id="_TOC3_15"></a>在 Visual Studio 生成的代码是特定的 T4 模板到.NET framework 目标版本

一个新属性是可用的 T4 文件从指向指定应用程序使用的.NET framework 版本的 ASP.NET MVC T4 主机。 这使 T4 模板生成代码和特定于.NET Framework 的版本的标记。 在 Visual Studio 2008 中，此值始终是.NET 3.5。 在 Visual Studio 2010 中，值为.NET 3.5 或.NET 4。

## <a id="_TOC4"></a>API 改进

本部分介绍对现有的 ASP.NET MVC 类型和成员的更改。

- 在控制器类中添加受保护的虚拟 CreateActionInvoker 方法。 此方法调用由控制器的 ActionInvoker 属性，如果已设置没有调用程序，则允许调用方的延迟实例化。
- AuthorizeAttribute 类中添加受保护的虚拟 HandleUnauthorizedRequest 方法。 这使派生 AuthorizeAttribute 来控制授权失败时的行为的筛选器。
- ValueProviderDictionary 类中添加一个 Add （字符串，对象键值） 方法。 这使您能够为 ValueProviderDictionary，使用字典初始值设定项语法，如以下示例所示：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- 添加 get\_对象 Sys.Mvc.AjaxContext 类中的方法。 这是类似于 get JavaScript 方法\_数据方法，但如果响应的内容类型为 application/json，获取\_对象返回的 JSON 对象。
- AuthorizationContext 类中添加 ActionDescriptor 属性。
- 添加可以用于绑定到包含任何 ID 属性，该属性不存在时的模型时要解决问题的 UrlParameter.Optional 令牌在窗体发布请求。 有关详细信息，请参阅输入[ASP.NET MVC 2 可选 URL 参数](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx)Phil Haack 博客上。

## <a id="_TOC5"></a>重大更改

以下的更改可能导致现有的 ASP.NET MVC 1.0 应用程序中出现错误。

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>在实现 IDataErrorInfo 的类的属性验证行为更改

对于使用 IDataErrorInfo 来执行验证的模型对象，是来验证每个属性，无论是否已设置新值。 在 ASP.NET MVC 1.0 中，已验证设置的新值的属性。 在 ASP.NET MVC 2 中，仅当所有属性验证程序都已成功调用 IDataErrorInfo 错误属性。

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>IIS 脚本映射脚本不再可用在安装程序

IIS 的脚本映射脚本是用于配置为 IIS 6 和 IIS 7 在经典模式下的脚本映射的命令行脚本。 如果你使用 Visual Studio 开发服务器，或在集成模式下使用 IIS 7，则不需要的脚本映射脚本。 脚本都是作为单独的不受支持下载上可用[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)。

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>在 MVC Future 的 Html.Substitute 帮助器方法不再可用

MVC 视图引擎的呈现行为发生变化，由于 Html.Substitute 帮助器方法不起作用，并已删除。

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>IValueProvider 接口替换所有使用 IDictionary

现在接受 IDictionary 在 MVC 1.0 中的每个属性或方法参数接受 IValueProvider。 此更改影响仅包括自定义值在提供程序或自定义模型联编程序的应用程序。 属性和受此更改的方法的示例包括：

- ControllerBase 和 ModelBindingContext 类 ValueProvider 属性。
- 控制器类 TryUpdateModel 方法。

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Site.css 文件中添加新的 CSS 类

中的 ASP.NET MVC 项目模板的 Site.css 文件已更新以包括验证功能以及的模板化帮助器使用的新样式。

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>帮助器现在返回 MvcHtmlString 对象

为了利用新的 HTML 编码表达式语法的 ASP.NET 4 中，HTML 帮助器的返回类型现 MvcHtmlString 而不是字符串。 如果在 ASP.NET 3.5 上使用 ASP.NET MVC 2 和新的帮助器，你将不能充分利用的 HTML 编码的语法;仅当你在 ASP.NET 4 上运行 ASP.NET MVC 2 时，新的语法是可用。

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult 现在仅响应 HTTP POST 请求

以便降低劫持默认情况下，具有信息泄露的可能性攻击的 JSON JsonResult 类现在仅响应 HTTP POST 请求。 应更改 Ajax 获取对操作方法的调用返回 JsonResult 对象，以改为使用 POST。 如有必要，你可以通过设置 JsonResult 的新 JsonRequestBehavior 属性重写此行为。 有关潜在漏洞的威胁的详细信息，请参阅博客文章[JSON 劫持](http://haacked.com/archive/2009/06/25/json-hijacking.aspx)Phil Haack 博客上。

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>模型和上 ModelBindingContext ModelType 属性 setter 中已过时

新的可设置 ModelMetadata 属性已添加到 ModelBindingContext 类。 新的属性封装模型和 ModelType 属性。 尽管的模型和 ModelType 属性已过时，但对于向后兼容性属性 getter 仍然可用;它们将委派给 ModelMetadata 属性来检索的值。

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>对 DefaultControllerFactory 类更改中断从它派生的自定义控制器工厂

通过删除 requestcontext 已属性进行了修复 DefaultControllerFactory 类。 替代此属性，请求上下文实例传递给受保护的虚拟 GetControllerInstance 和 GetControllerType 方法。 此更改会影响从 DefaultControllerFactory 派生的自定义控制器工厂。

自定义控制器工厂通常用于提供应用程序的 ASP.NET MVC 的依赖关系注入。 若要更新的自定义控制器工厂，以支持 ASP.NET MVC 2，更改方法签名或签名，以匹配新的签名和请求上下文参数使用而不是属性。

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"区域"是保留的路由值密钥的现在

路由值中的字符串"区域"现在在 ASP.NET MVC 中具有特殊含义，"控制器"和"操作"执行的相同方式。 一个含意为，如果 HTML 帮助器随附了一个包含"区域"的路由值字典，帮助程序将不再追加"区域"查询字符串中。

如果你使用的区域功能，请确保不使用 {区} 作为路由 URL 的一部分。


## <a id="_TOC6"></a>  Disclaimer

这是一份初稿，并可能在本文所述软件最终商业发布之前进行大幅更改。

本文所含信息代表 Microsoft Corporation 对截至发布之日所讨论问题持有的当前观点。 由于 Microsoft 必须对不断变化的市场情况作出响应，所以不应将本文解释为是 Microsoft 做出的承诺，Microsoft 并不保证所提供的任何信息在公布之日后的准确性。

本白皮书仅用于提供信息。 MICROSOFT 对本文档中的信息不做任何明示、暗示或法定的担保。

遵守所有适用的著作权法是用户的责任。 未经 Microsoft Corporation 明确的书面许可，不得出于任何目的或以任何形式或任何手段（电子、机械、影印、记录或其他方法）复制本文档的任何部分，或者将其存储或引入检索系统，或者将其进行传播。受版权法保护的权利不受此限制。

对于本文档中的主题，Microsoft 可能具有专利、专利申请、商标、版权或其他知识产权。 除非 Microsoft 的任何书面许可协议明确提出，否则，本文档的提供并不表示 Microsoft 已将这些专利、商标、版权或其他知识产权的任何许可权限授予您。

除非另行说明，示例公司、 组织、 产品、 域名、 电子邮件地址、 徽标、 人物、 地点和事件此处所述虚构，是的无意与任何真实的公司、 组织、 产品、 域名、 电子邮件地址、 徽标、 人员、 位置或事件旨在或妄加推断。

© 2010 Microsoft Corporation. 保留所有权利。

Microsoft 和 Windows 是 Microsoft Corporation 在美国和/或其他国家/地区的注册商标或商标。

此处提到的真实公司和产品的名称可能是其各自所有者的商标。
