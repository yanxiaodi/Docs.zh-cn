---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: "了解 ASP.NET AJAX Web 服务 |Microsoft 文档"
author: scottcate
description: "Web 服务是为分布式系统之间交换数据提供跨平台解决方案的.NET framework 的必要组成部分。 尽管 Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 8eb3486c9b3f4ddb6a8bc2c1cdcac774a6852574
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-web-services"></a>了解 ASP.NET AJAX Web 服务
====================
通过[Scott 类别](https://github.com/scottcate)

[下载 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Web 服务是为分布式系统之间交换数据提供跨平台解决方案的.NET framework 的必要组成部分。 尽管 Web 服务通常用于允许不同的操作系统、 对象模型和编程语言来发送和接收数据，但它们也可动态地将数据注入到的 ASP.NET AJAX 页上或将数据从页发送到后端系统。 上述所有操作均可以而无需进行重新排序以回发的操作。


## <a name="calling-web-services-with-aspnet-ajax"></a>与 ASP.NET AJAX 一起调用 Web 服务

Dan Wahlin

Web 服务是为分布式系统之间交换数据提供跨平台解决方案的.NET framework 的必要组成部分。 尽管 Web 服务通常用于允许不同的操作系统、 对象模型和编程语言来发送和接收数据，但它们也可动态地将数据注入到的 ASP.NET AJAX 页上或将数据从页发送到后端系统。 上述所有操作均可以而无需进行重新排序以回发的操作。

ASP.NET AJAX UpdatePanel 控件提供一种简单到 AJAX 启用任何 ASP.NET 页，可能有些时候你何时需要动态而无需使用 UpdatePanel 访问服务器上的数据。 这篇文章中你将了解如何通过创建和使用 Web 服务在 ASP.NET AJAX 页内完成此操作。

本文将重点核心 ASP.NET AJAX 扩展，以及调用 AutoCompleteExtender ASP.NET AJAX 工具包中的启用的 Web 服务控件中提供的功能。 涵盖的主题包括定义启用了 AJAX 的 Web 服务、 创建客户端代理和调用 Web 服务使用 JavaScript。 你还将看到如何直接到 ASP.NET 页方法进行 Web 服务调用。

## <a name="web-services-configuration"></a>Web 服务配置

使用 Visual Studio 2008 创建新的网站项目后，web.config 文件将具有大量可能不熟悉的 Visual Studio 的早期版本的用户的新增功能。 以便它们可以用于在页中定义的其它必需 HttpHandlers 和 Httpmodule 时，这些修改的某些值映射到 ASP.NET AJAX 控件"asp"前缀。 列表 1 显示对所做的修改`<httpHandlers>`影响 Web 服务调用的 web.config 中的元素。 删除 HttpHandler 用于处理.asmx 调用默认值并将其替换为中 System.Web.Extensions.dll 程序集的 ScriptHandlerFactory 类。 System.Web.Extensions.dll 包含所有由 ASP.NET AJAX 的核心功能。

**列表 1。ASP.NET AJAX Web 服务处理程序配置**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

使用 JavaScript Web 服务代理以便允许 JavaScript 对象表示法 (JSON) 调用必须做出从 ASP.NET AJAX 网页的.NET Web 服务，进行此 HttpHandler 替换。 ASP.NET AJAX 将 JSON 消息发送到 Web 服务，而不是标准的简单对象访问协议 (SOAP) 调用通常与 Web 服务相关联。 这会导致较小的请求和响应消息总体。 它还允许将客户端处理更高效的数据由于 ASP.NET AJAX JavaScript 库进行优化，以使用 JSON 对象。 列出 2 和列出 3 显示的 Web 服务请求和响应消息序列化为 JSON 格式的示例。 列出 3 中的响应消息传递客户对象的数组和及其关联的属性时，请求消息中列出 2 所示将值为"比利时"国家/地区参数进行传递。

**列出 2。Web 服务请求消息序列化为 JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *>[!NOTE]操作名称定义为 web 服务的 URL 的一部分; 此外，请求消息也不会始终提交通过 JSON。Web 服务可以利用 ScriptMethod 属性 UseHttpGet 参数设置为 true，这会导致参数通过传递查询字符串参数。*


**列出 3。Web 服务响应消息序列化为 JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

在下一部分中，你将看到如何创建 Web 服务能够处理 JSON 请求消息和响应简单和复杂类型。

## <a name="creating-ajax-enabled-web-services"></a>创建启用了 AJAX 的 Web 服务

ASP.NET AJAX 框架提供多种不同的方式来调用 Web 服务。 你可以使用 AutoCompleteExtender 控件 （在 ASP.NET AJAX 工具包中可用） 或 JavaScript。 但是，在调用服务之前必须 AJAX 启用它，以便它可以由客户端脚本代码调用。

无论你不熟悉 ASP.NET Web 服务，你将找到它简单地创建和支持 AJAX 的服务。 .NET framework 已支持的自 2002年中其初始发布 ASP.NET Web 服务的创建和使用 ASP.NET AJAX Extensions 提供基于.NET framework 的组默认的功能的其他 AJAX 功能。 Visual Studio.NET 2008 Beta 2 创建.asmx Web 服务文件的内置支持，并自动从 System.Web.Services.WebService 类派生类旁边的关联的代码。 像将方法添加到类必须将应用以使其调用由 Web 服务使用者 WebMethod 特性。

列出 4 显示了将 WebMethod 特性应用于一个名为 GetCustomersByCountry() 方法的示例。

**列出 4。Web 服务中使用 WebMethod 特性**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

GetCustomersByCountry() 方法接受国家/地区参数，并返回一个客户对象数组。 传递给方法的国家/地区值转发到的业务层类，后者又调用以从数据库中检索数据的数据层类用数据填充客户对象属性，并返回的数组。

## <a name="using-the-scriptservice-attribute"></a>使用 ScriptService 特性

添加属性允许的标准 SOAP 消息发送到 Web 服务的客户端调用的 GetCustomersByCountry() 方法 WebMethod，时它不允许使用 JSON 调用以从 ASP.NET AJAX 应用程序在初始状态下进行。 若要允许进行你需要应用 ASP.NET AJAX 扩展的 JSON 调用`ScriptService`属性设为 Web 服务类。 这提供 Web 服务以发送响应消息使用 JSON 格式设置，并允许客户端脚本，以调用服务，将 JSON 消息发送。

列出 5 演示将 ScriptService 特性应用于一个名为 CustomersService 的 Web 服务类的一个示例。

**列出 5。使用支持 AJAX 的 Web 服务将 ScriptService 属性**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

ScriptService 属性充当一种标记，该值指示它可以 AJAX 脚本代码中调用。 实际上，它不会处理任何 JSON 序列化或反序列化的任务在后台发生。 （在 web.config 中配置） ScriptHandlerFactory 和其他相关的类执行大容量的 JSON 处理。

## <a name="using-the-scriptmethod-attribute"></a>使用 ScriptMethod 特性

ScriptService 属性是.NET Web 服务以使其以使用 ASP.NET AJAX 页中定义的唯一 ASP.NET AJAX 属性。 但是，还可以直接向服务中的 Web 方法应用名为 ScriptMethod 的另一个属性。 ScriptMethod 定义三个属性包括`UseHttpGet`，`ResponseFormat`和`XmlSerializeString`。 更改这些属性的值可能会在接受 Web 方法的请求的类型需要更改为 GET、 Web 方法需要的形式返回原始 XML 数据时的情况下有用`XmlDocument`或`XmlElement`对象或从返回数据时 始终应为而不是 JSON 的 XML 序列化服务。

当 Web 方法应接受时，可以使用属性 UseHttpGet 获取而不是发出的 POST 请求的请求。 使用 URL 与 Web 方法的输入参数转换为查询字符串参数发送请求。 UseHttpGet 属性默认为 false，仅应设置为`true`当已知操作安全且当敏感数据不会传递到 Web 服务。 列出 6 显示与 UseHttpGet 属性使用 ScriptMethod 属性的示例。

**列出 6。使用 UseHttpGet 属性 ScriptMethod 属性。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

下一步显示的列表 6 中所示的 HttpGetEcho Web 方法调用时发送的标头示例：

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

当需要从服务，而不是 JSON 返回 XML 响应时，也可以还允许以接受 HTTP GET 请求的 Web 方法，使用 ScriptMethod 属性。 例如，Web 服务可能会检索远程站点中的 RSS 源，并将其作为 XmlDocument 或 XmlElement 对象。 处理 XML 数据然后出现客户端上。

列出 7 显示使用 ResponseFormat 属性来指定应从 Web 方法返回的 XML 数据的示例。

**列出 7。使用 ResponseFormat 属性 ScriptMethod 属性。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

ResponseFormat 属性还可以与 XmlSerializeString 属性一起使用。 XmlSerializeString 属性具有默认值为 false，这意味着所有返回除从某一 Web 方法返回的字符串之外的类型扩展会序列化为 XML 时`ResponseFormat`属性设置为`ResponseFormat.Xml`。 当`XmlSerializeString`设置为`true`，从某一 Web 方法返回的所有类型扩展会都序列化为 XML 包括字符串类型。 如果 ResponseFormat 属性值为`ResponseFormat.Json`XmlSerializeString 属性将被忽略。

列出 8 演示使用 XmlSerializeString 属性以强制要序列化为 XML 的字符串的示例。

**列出 8。ScriptMethod 属性使用 XmlSerializeString 属性**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

从调用列出 8 中所示的 GetXmlString Web 方法返回的值显示下一步：

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

尽管默认 JSON 格式的请求和响应消息的总体大小降到最低，以跨浏览器的方式更轻松地使用 ASP.NET AJAX 客户端，ResponseFormat 和 XmlSerializeString 属性可以是利用时客户端如 Internet Explorer 5 或更高版本的应用程序希望要从 Web 方法返回 XML 数据。

## <a name="working-with-complex-types"></a>使用复杂类型

列出 5 介绍了返回名为从 Web 服务的客户的复杂类型的一个示例。 Customer 类内部定义为属性，例如 FirstName 和 LastName 的几种不同的简单类型。 复杂类型用作输入参数，或在启用了 AJAX 的 Web 方法的返回类型将自动序列化为 JSON 发送到客户端之前。 但是，嵌套的复杂类型 （那些在另一个类型内内部定义） 不将提供给客户端作为独立对象默认情况下。

在其中嵌套复杂类型由 Web 服务还必须使用在客户端页中的情况下，ASP.NET AJAX GenerateScriptType 属性可以添加到 Web 服务。 例如，列出 9 所示的 CustomerDetails 类包含地址和性别属性其中***表示嵌套的复杂类型。***

**列出 9。此处显示的 CustomerDetails 类包含两个嵌套的复杂类型。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

列出 9 所示 CustomerDetails 类中定义的地址和性别对象不会自动将提供用于在通过 JavaScript 客户端由于它们是嵌套的类型 （地址是一个类和性别是一个枚举）。 可以在其中使用在 Web 服务内的嵌套的类型必须在客户端上可用的情况下，使用前面所述 GenerateScriptType 属性 （请参阅列出 10）。 在其中从服务返回不同的嵌套复杂类型的情况下，此属性可以添加多个时间。 它可以应用直接与 Web 服务类或更高版本特定的 Web 方法。

**列出 10。使用 GenerateScriptService 特性定义应可供客户端的嵌套的类型。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

通过应用`GenerateScriptType`到 Web 服务、 地址和性别类型的属性将自动变为可供使用的客户端 ASP.NET AJAX JavaScript 代码。 自动生成并通过 Web 服务上添加 GenerateScriptType 属性发送到客户端 JavaScript 的示例所示列出 11。 你将看到如何更高版本在文章中使用嵌套的复杂类型。

**列出 11。提供给 ASP.NET AJAX 页上的嵌套复杂类型。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

现在，你已了解如何创建 Web 服务并使它们的 ASP.NET AJAX 页可以访问，让我们看看如何创建和使用 JavaScript 代理，以便可以检索数据，或将其发送到 Web 服务。

## <a name="creating-javascript-proxies"></a>创建 JavaScript 代理

调用标准 Web 服务 （.NET 或另一个平台） 通常涉及到创建从发送 SOAP 请求和响应消息的复杂性可保护你的代理对象。 与 ASP.NET AJAX Web 服务调用，可创建和用于轻松地调用而无需担心序列化和反序列化 JSON 消息的服务的 JavaScript 代理。 可以通过使用 ASP.NET AJAX ScriptManager 控件自动生成 JavaScript 代理。

创建可调用 Web 服务的 JavaScript 代理是通过 ScriptManager 的服务属性来完成。 此属性允许你定义一个或多个服务的 ASP.NET AJAX 页上可以以异步方式调用，以发送或接收数据，而无需回发的操作。 通过使用 ASP.NET AJAX 定义服务`ServiceReference`控件并将 Web 服务 URL 分配给控件的`Path`属性。 列出 12 显示一个引用名为 CustomersService.asmx 的服务的示例。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**列出 12。定义中的 ASP.NET AJAX 页上使用的 Web 服务。**

添加对通过 ScriptManager 控件 CustomersService.asmx 的引用会导致一个 JavaScript 代理，以动态生成并被的页引用。 通过使用嵌入代理&lt;脚本&gt;标记和动态加载通过调用 CustomersService.asmx 文件并正在并可通过 /js 追加到末尾。 下面的示例演示如何 JavaScript 代理在页中时嵌入在 web.config 中禁用调试：

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *>[!NOTE]如果你想要查看生成的实际 JavaScript 代理代码可以在 Internet Explorer 的地址框中键入所需的.NET Web 服务的 URL，还可以将并可通过 /js 追加到它的末尾。*


如果 JavaScript 代理的调试版本将在页中为嵌入的 web.config 中启用了调试显示下一步:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

创建通过 ScriptManager 的 JavaScript 代理也可以直接到页面嵌入而不是使用引用&lt;脚本&gt;标记的 src 属性。 这可以通过将 ServiceReference 控件的 InlineScript 属性设置为 true （默认值为 false）。 代理不共享跨多页和你想要减少对服务器进行网络调用的数量时，这很有用。 不会由浏览器缓存 InlineScript 当设置为 true 的代理脚本，因此在其中由 ASP.NET AJAX 应用程序中的多个页使用的代理的情况下建议的默认值为 false。 使用 InlineScript 属性的示例显示下一步：

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>使用 JavaScript 代理

一旦 Web 服务引用使用 ScriptManager 控件的 ASP.NET AJAX 页上，可以对 Web 服务进行调用，并且可以使用回调函数对返回的数据进行处理。 引用命名空间 （如果存在）、 类名和 Web 方法名称由调用 Web 服务。 以及处理返回的数据的回调函数，可以定义传递给 Web 服务的任何参数。

使用 JavaScript 代理来调用一个名为 GetCustomersByCountry() 的 Web 方法的示例所示列出 13。 当最终用户单击页面上的按钮时调用 GetCustomersByCountry() 函数。

**列出 13。调用 Web 服务时使用的 JavaScript 代理。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

此调用引用 InterfaceTraining 命名空间，在服务中定义 CustomersService 类和 GetCustomersByCountry Web 方法。 它将传递从文本框中获取一个国家/地区值，以及一个名为 OnWSRequestComplete 异步的 Web 服务调用返回时应调用的回调函数。 OnWSRequestComplete 处理从服务返回的客户对象的数组，并将它们转换为页中显示的表。 从调用生成的输出，如图 1 所示。


[![通过进行异步 AJAX 调用到 Web 服务获取的数据绑定。](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**图 1**： 将数据绑定通过进行异步 AJAX 调用到 Web 服务。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-web-services/_static/image3.png))


JavaScript 代理还可单向调用到 Web 服务在情况下，应调用 Web 方法，但代理不应等待的响应。 例如，你可能想要调用 Web 服务以启动进程如工作流，但不是等待从服务返回值。 在单向调用需要向服务发出的情况下，只需可以省略列出 13 中所示的回调函数。 由于不定义任何回调函数的代理对象不会等待 Web 服务以返回数据。

## <a name="handling-errors"></a>处理错误

对 Web 服务的异步回调可能会遇到错误，例如网络停机，Web 服务变得不可用或返回异常的不同的类型。 幸运的是，生成的 ScriptManager 的 JavaScript 代理对象允许将多个回调，以定义用于处理错误和失败除了前面所示的 success 回调。 列出 14 中所示，可以对 Web 方法的调用中的标准的回调函数后立即定义了错误的回调函数。

**列出 14。定义了错误的回调函数并显示错误。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

当调用 Web 服务发生任何错误将触发 OnWSRequestFailed() 回调函数，它接受一个表示错误作为参数的对象调用。 错误对象公开几个不同的函数，以确定错误的原因以及调用的操作已超时。列出 14 举例说明如何使用不同的错误函数和图 2 显示了由函数生成的输出示例。


[![通过调用 ASP.NET AJAX 错误函数生成的输出。](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**图 2**： 通过调用 ASP.NET AJAX 错误函数生成的输出。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>处理从 Web 服务返回的 XML 数据

前面你已了解如何 Web 方法无法返回原始 XML 数据，通过使用 ScriptMethod 属性以及其 ResponseFormat 属性。 当 ResponseFormat 设置为 ResponseFormat.Xml 时，从 Web 服务返回的数据被序列化为 xml 格式，而不是 JSON。 如果需要被直接传递到客户端使用 JavaScript 或 XSLT 处理 XML 数据，这很有用。 在当前时间，Internet Explorer 5 或更高版本提供最佳的客户端对象模型，用于分析和筛选由于它内置对 MSXML 支持的 XML 数据。

从 Web 服务检索 XML 数据是比检索其他数据类型没有什么不同。 通过调用要调用相应的函数，并定义回调函数的 JavaScript 代理启动。 在调用返回后，你可以然后处理回调函数中的数据。

列出 15 显示调用一个名为 GetRssFeed() 返回一个 XmlElement 对象的 Web 方法的示例。 GetRssFeed() 接受单个参数的 rss 源来检索表示 URL。

**列出 15。使用 Web 服务返回的 XML 数据。**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

此示例将 URL 传递给 RSS 源，并处理 OnWSRequestComplete() 函数中返回的 XML 数据。 OnWSRequestComplete() 首先进行检查，以查看浏览器是否知道 MSXML 分析器是可用的 Internet Explorer。 如果是，使用 XPath 语句查找全部&lt;项&gt;RSS 源中的标记。 然后循环访问每个项和关联&lt;标题&gt;和&lt;链接&gt;标记是位于和处理以显示每个项的数据。 图 3 显示生成进行调用通过指向 GetRssFeed() Web 方法的 JavaScript 代理 ASP.NET AJAX 的输出的示例。

## <a name="handling-complex-types"></a>处理复杂类型

通过 JavaScript 代理自动公开接受或 Web 服务返回的复杂类型。 但是，除非 GenerateScriptType 属性应用于服务，如前文所述嵌套复杂类型不直接访问客户端上。 为什么要在客户端使用嵌套的复杂类型？

若要回答这个问题，假设的 ASP.NET AJAX 页上显示客户数据，并允许最终用户以更新客户的地址。 如果 Web 服务指定地址类型 （CustomerDetails 类中定义的复杂类型），可以发送到客户端的更新过程可以分为供重复使用更好代码单独的功能。


[![从调用返回 RSS 数据的 Web 服务创建的输出。](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**图 3**： 输出从调用返回 RSS 数据的 Web 服务创建。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-web-services/_static/image9.png))


列出 16 显示模型命名空间中，在定义的地址对象时，将调用的客户端代码的示例使用更新后的数据填充它，并将其分配给 CustomerDetails 对象的地址属性。 然后将 CustomerDetails 对象传递到 Web 服务进行处理。

**列出 16。使用嵌套的复杂类型**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>创建和使用页方法

Web 服务提供将重新可用服务向客户端包括 ASP.NET AJAX 页各种的极佳途径。 但是，可能需要检索数据，而不会使用或由其他页面共享页的情况。 在这种情况下，.asmx 文件来允许页后，可以访问的数据可能看上去太过严厉因为单个页面仅使用该服务。

ASP.NET AJAX 提供了另一种机制，而无需创建独立.asmx 文件以 Web 类似于服务的调用。 这可通过使用一种技术称为"页方法"。 页方法是静态 (共享在 VB.NET) 方法直接在页或代码旁置文件中嵌入具有应用于它们的 WebMethod 属性。 通过应用 WebMethod 特性它们可以调用使用名为 PageMethods 获取在运行时动态创建一个特殊的 JavaScript 对象。 PageMethods 对象充当一个 JSON 序列化/反序列化过程中保护你的代理服务器。 请注意，为了使用 PageMethods 对象必须将 ScriptManager EnablePageMethods 属性设置为 true。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

列出 17 演示在 ASP.NET 代码旁置类定义两个页方法的示例。 这些方法从应用中的业务层类中检索数据\_的网站代码文件夹。

**列出 17。定义页方法。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

ScriptManager 在页中检测存在 Web 方法时它会生成对前面所述 PageMethods 对象的动态引用。 调用 Web 方法是通过引用该方法应传递任何必要的参数数据的名称后跟 PageMethods 类实现的。 列出 18 显示调用前面所示的两个页方法的示例。

**列出 18。调用页与 PageMethods JavaScript 对象的方法。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

使用 PageMethods 对象是非常类似于使用 JavaScript 代理对象。 你首先指定所有应传递给页方法，然后定义异步调用返回时应调用的回调函数的参数数据。 此外可以指定故障回调 （请参阅列出 14 有关的处理故障示例）。

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender 和 ASP.NET AJAX 工具包

ASP.NET AJAX 工具包 (可从[http://ajax.asp.net](http://ajax.asp.net)) 提供了几个控件，可用来访问 Web 服务。 具体而言，此工具包中包含一个名为的有用控件`AutoCompleteExtender`，可以用来调用 Web 服务和而无需编写任何 JavaScript 代码在所有页中显示数据。

AutoCompleteExtender 控件可用来扩展现有功能的文本框中，帮助用户更轻松地找到它们正在寻找的数据。 在键入到文本框中时该控件可用于查询 Web 服务，并动态显示文本框下的结果。 图 4 显示使用 AutoCompleteExtender 控件以显示支持应用程序的客户 id 的示例。 当用户在文本框中键入不同的字符，将显示不同的项在下面它基于其输入。 然后，用户可以选择所需的客户 id。

使用 ASP.NET AJAX 页中的 AutoCompleteExtender 要求 AjaxControlToolkit.dll 程序集添加到网站的 bin 文件夹。 一旦添加工具包程序集，你将想要在 web.config 中进行引用，以便它包含的控件可用于应用程序中的所有网页。 这可以通过添加以下标记内 web.config 的&lt;控件&gt;标记：

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

在其中你只需使用特定的页中的控件的情况下你可以引用它通过将引用指令添加到页面顶部，如下所示而不是更新 web.config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![使用 AutoCompleteExtender 控件。](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**图 4**： 使用 AutoCompleteExtender 控件。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-web-services/_static/image12.png))


一旦网站已配置为使用 ASP.NET AJAX 工具包，AutoCompleteExtender 控件可以添加到页面中更像添加正则的 ASP.NET 服务器控件。 列出 19 显示使用控件能够调用 Web 服务的示例。

**列出 19。使用 ASP.NET AJAX 工具包 AutoCompleteExtender 控件。**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender 具有多个不同的属性包括在服务器控件上找到的标准 ID 和 runat 属性。 除此之外，它允许你定义 Web 服务之前最终用户类型查询的数据的多少个字符。 列出 19 中所示的 MinimumPrefixLength 属性会导致在文本框中键入字符每次调用服务。 你将需要小心设置此值，因为每次用户键入的字符 Web 服务将调用以搜索在文本框中的字符匹配的值。 要调用的 Web 服务，以及目标 Web 方法的分别使用 ServicePath 和 ServiceMethod 属性定义。 最后，TargetControlID 属性标识的文本框中，将挂钩到 AutoCompleteExtender 控件。

要调用的 Web 服务必须具有应用前面所述的 ScriptService 属性，并且目标 Web 方法必须接受名为 prefixText 和计数的两个参数。 PrefixText 参数表示的最终用户键入的字符，count 参数表示要返回的项目数 （默认值为 10）。 列出 20 显示了由列出 19 前面所示的 AutoCompleteExtender 控件调用 GetCustomerIDs Web 方法的示例。 Web 方法调用，反过来调用数据层处理筛选数据并返回匹配的结果的方法的业务层方法。 数据层方法的代码所示列出 21。

**列出 20。筛选从 AutoCompleteExtender 控件发送的数据。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**列出 21。筛选基于最终用户输入的结果。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>结束语

ASP.NET AJAX 提供了用于调用 Web 服务，而无需编写大量的自定义 JavaScript 代码来处理请求和响应消息的优秀的支持。 在这篇文章你已了解如何启用 AJAX 的.NET Web 服务，以使它们能够处理 JSON 消息以及如何定义使用 ScriptManager 控件的 JavaScript 代理。 您还已了解如何可以使用代理服务器来调用 Web 服务的 JavaScript 处理简单和复杂类型和处理失败。 最后，你已了解如何使用页方法来简化创建和 Web 服务调用的过程，以及如何 AutoCompleteExtender 控件可以提供给最终用户的帮助，在键入时。 尽管 ASP.NET AJAX 中可用 UpdatePanel 肯定将选择其简易性由于许多 AJAX 程序员的控制，让我们了解如何通过 JavaScript 代理调用 Web 服务可在许多应用程序。

## <a name="bio"></a>简介

Dan Wahlin （Microsoft 最有价值专家 ASP.NET 和 XML Web 服务） 是在接口技术培训的.NET 开发教师和体系结构顾问 ([http://www.interfacett.com](http://www.interfacett.com))。 Dan 成立 ASP.NET 开发人员网站的 XML ([www.XMLforASP.NET](http://www.XMLforASP.NET))、 位于 INETA 扬声器局和在几个有关参加会议讲话时。 Dan 共同撰写 Professional Windows dna 的解码 (Wrox) ASP.NET： 提示、 教程和代码 (Sam)、 ASP.NET 1.1 内部解决方案、 Professional ASP.NET 2.0 AJAX (Wrox)、 ASP.NET 2.0 MVP Hacks 和编写的 XML for ASP.NET 开发人员 (Sam)。 当他不编写代码、 项目或丛书时，Dan 将喜欢编写和录制音乐和播放高尔夫球和儿童他妻子与支篮球队。

Scott 类别自 1997 年以来处理与 Microsoft Web 技术，并且是 myKB.com 总裁 ([www.myKB.com](http://www.myKB.com)) 其中他专注于编写 ASP.NET 基于侧重于知识库软件解决方案的应用程序。 可以通过在电子邮件联系 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在其博客地址[ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[上一页](understanding-asp-net-ajax-localization.md)
[下一页](understanding-asp-net-ajax-debugging-capabilities.md)
